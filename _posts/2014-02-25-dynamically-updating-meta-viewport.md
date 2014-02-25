---
layout: post
title:  Dynamically updating &lt;meta viewport&gt; in the year 2014.
date:   2014-02-25
---

If you want to control the viewport of a mobile browser, you're likely already using the so-called [meta viewport][mv]. If you want to _update_ the viewport&mdash;or perhaps set its value depending on some loadtime condition&mdash;there are (at least?) two ways to do this:

1) Append a new `meta` element to the page.
2) Update the existing `meta` element's `content` attribute.

Here's a sweet test page with some sweet buttons you can click on your mobile device to see both methods: [[really sweet test page]][stp].

(Naturally I just hacked up a page from Andreas Boven's cool [Understanding Viewport][uv].)

If you scroogle "dynamically update meta viewport", you'll likely end up at [PPK's blog post][ppk] on the topic&mdash;but that was written back in 2011 when Beatlemania and Communism were still a thing.

Here's an updated account:

Appending a new `meta` element works in every iOS and Android browser I could get my hands on (unfortunately I don't have any Windows Phone devices&mdash;but my friend reported that it didn't update on his Windows Phone).

Updating existing `meta @content` works in everything I tried except Mobile Firefox browsers and Opera Mini (and IE Mobile 10-ish?). I don't expect Opera Mini to ever be updated, but you never know&mdash;Communism did eventually fail.

I filed bugs for [Firefox for Android][ffa] and [Firefox OS][fxos] because I think they should behave like Android and iOS browsers for compatibility reasons. (I also think it's more intuitive to change an attribute, rather than add a new element to the document.)


[ppk]: http://www.quirksmode.org/blog/archives/2011/06/dynamically_cha.html
[mv]: http://dev.opera.com/articles/view/an-introduction-to-meta-viewport-and-viewport/
[stp]: https://miketaylr.com/bzla/viewport-change.html
[uv]: http://andreasbovens.github.io/understanding-viewport/
[fxos]: https://bugzilla.mozilla.org/show_bug.cgi?id=976618
[ffa]: https://bugzilla.mozilla.org/show_bug.cgi?id=976616