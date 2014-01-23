---
layout: post
title:  Differences between YUI 2.x and YUI3 setStyle
date:   2014-01-23
---


Much like [Google Images][gi], my colleague Hallvord discovered that the cause of [a bug over at Flickr][bug] is due to passing the `style` property of a DOM element a css-cased (`background-image`) property, rather than camel-cased (`backgroundImage`) one.

In this particular instance, Flickr is using YUI3's `setStyle` like so: 

``` js
elm.setStyle('background-image', superArtsyImageHashTagNoFilter)
```

The docs are pretty clear that you're [not supposed to do that][docs], and should use `backgroundImage` camel-cased.

However, the previous incarnation of YUI, YUI2, would [automatically camel-case it][yui2] for you, i.e., `var property = Y.Dom._toCamel(args.prop)`. YUI3, though, [does not][yui3], i.e., `style[att] = val`.

(9 commas in a run-on sentence, a personal record).

So if you're upgrading from 2.x to 3, but you're used to the YUI2 argument style *and* you only test in a WebKit-based browser you'll never notice that you've introduced a bug.

WebKit-based browsers (for whatever reason) are happy to allow you to set styles either way. Give this [test case][tc] a spin in Safari or Chrome and compare to Firefox or Internet Explorer.

A few things from the [CSSOM spec][spec] for the nerds:
<blockquote>

For each CSS property property that is a supported CSS property, the following partial interface applies where camel-cased attribute is obtained by running the CSS property to IDL attribute algorithm for property.

partial interface CSSStyleDeclaration {
           attribute DOMString _camel-cased attribute;
};

[...blah blah blah...]

Setting the camel-cased attribute attribute must invoke setProperty() with the first argument being the result of running the IDL attribute to CSS property algorithm for camel-cased attribute, as second argument the given value, and no third argument. Any exceptions thrown must be re-thrown.
</blockquote>

Step 3 of the `setProperty` method of the `CSSStyleDeclaration` interface is defined as:
<blockquote>

If property is not a case-sensitive match for a supported CSS property, terminate this algorithm.
</blockquote>

So the spec seems pretty clear that you should bail if you try to set something like `elm.style[-satire-considered] = "harmful"` (a valid property: value pair in [California Style Sheets][css]).

The moral of the story is if you find yourself wondering why your [YUI3 setStyle code isn't working in other browsers (like these dudes on StackOverflow)][so], now you know. Uh that doesn't seem like a moral, you're thinking&mdash;don't ask.

(BTW, some sweet, sweet stackoverflow points are up for grabs if you want to update that answer. I can't remember my username or password. ¯\\_(ツ)_/¯).

[gi]: http://www.whatcouldbewrong.com/articles/7/google-image-search-s-image-pile
[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=732355#c32
[spec]: http://dev.w3.org/csswg/cssom/#the-cssstyledeclaration-interface
[docs]: http://yuilibrary.com/yui/docs/api/classes/Node.html#method_setStyle
[yui2]: https://github.com/yui/yui2/blob/master/src/dom/js/Dom.js#L207-L251
[yui3]: https://github.com/yui/yui3/blob/master/src/dom/js/dom-style.js#L59-L83
[tc]: https://miketaylr.com/bzla/setstyle.html
[so]: http://stackoverflow.com/questions/5034878/yui3-js-setstyle-using-vendor-prefixes
[css]: https://medium.com/cool-code-pal/1f6430781393