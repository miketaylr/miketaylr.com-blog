---
layout: post
title: Goosebumps! empty img src error events in Firefox
date: 2016-09-07
---

It's almost Septemberween, which means it's that time of the year we gather 'round our spinning MacBook fans and share website ghost stories:

The tale of the disappearing burger and lobster site content (i.e., [webcompat bug 2760][wc]).

Chapter 1.
----------

Once upon a time (that time being the present), there was this burger and lobster "restaurant" called, um well, [burger and lobster][bl]. In Firefox, as of today, the site content never renders &mdash; you just end up with some fancy illustrations (none of which are burgers or lobsters).

Opening devtools, you've got the following mysterious stacktrace:

```js
Error: node is undefined
compositeLinkFn@http://www.burgerandlobster.../angular-1.2.25.js:6108:13
compositeLinkFn@http://www.burgerandlobster.../angular-1.2.25.js:6108:13
nodeLinkFn@http://www.burgerandlobster.../angular-1.2.25.js:6705:24
(...stack frames descend into hell...)
jQuery@http://www.burgerandlobster.../jquery-1.11.1.js:73:10
@http://www.burgerandlobster.../jquery-1.11.1.js:3557:1
@http://www.burgerandlobster.../jquery-1.11.1.js:34:3
@http://www.burgerandlobster.../jquery-1.11.1.js:15:2
```

Cool, time to debug AngularJS.

But it turns out that leads nowhere besides the abyss, AKA funtions that return functions that compose with other functions with dollar signs in them... and the bug is elsewhere. Besides, Chrome has a similar error, and the page works there. Just a haunted node, maybe.

Dennis Schubert [discovered that adding a `<base href="/">` fixes the site][disc], which [happens to be required by Angular is later versions for `$locationProvider.html5Mode`][nb]. But this bug has nothing to do with `pushState` or history, or even [SVG `xlink:href`s][svg], all of which the `<base>` element can affect.

Another (dead) rabbit hole, another dead end (spooky).

At some point, all your debugging tricks and intuitions fail and it's time to just page a thousand lines of framework-specific JS into your brain and see where that leads. Two hours later, if you're lucky, you notice something weird like so:

```js
var illustArr = [
  {
    "url": "/Assets/images/illustrations/alert-man.png",
    "x": "2508",
    "y": "2028"
  },
...
(bunch of similar objects objects, then...)
  {
    "url": "",
    "x": "",
    "y": ""
  };
```

And you recall a method that dispatches a `allImagesLoaded` event which tells the app it's OK to load the rest of the page content. It looks like this:

```js
b.allImagesLoaded = function() {
  d += 1,
  d === a.imageArr.length && $("body").trigger("allImagesLoaded")
}
```

But it only does that once it's counted that all images have loaded (or fired an error event):

```js
l.loadImage = function(a) {
  var b = $(document.createElement("div"))
    , c = $(document.createElement("img"));
  [...]
  c.attr("src", a.url),
  [...]
  c.bind("load error", function(e) {
      $(this).addClass("illustration--show"),
      h.allImagesLoaded()
  })
}
```

So yeah, that looks fishy. And that's why Firefox gets stuck&mdash;it doesn't fire `error` events when `img.src` is set to the empty string, which is [required per HTML][spec]. Here's a small [test case][tc], which also demonstrates why the `<base href="/">` fixed the page&mdash;it'll fire an `error` event when requesting an image from the site root (and eventually barf on the HTML, I guess).

Anyways, the Gecko bug for that is [Bug 599975][bzbug]. That will probably get fixed soon.

Epilogue.
----------

So what's the moral of this ghost story? There is none. Septemberween is cruel that way.

[wc]: https://github.com/webcompat/web-bugs/issues/2760
[disc]: https://github.com/webcompat/web-bugs/issues/2760#issuecomment-237747696
[bz]: https://bugzilla.mozilla.org/show_bug.cgi?id=599975
[bl]: http://www.burgerandlobster.com/home/
[nb]: https://docs.angularjs.org/error/$location/nobase
[svg]: https://github.com/emilbjorklund/svg-weirdness/issues/18
[tc]: http://miketaylr.com/bzla/burger-load.html
[spec]: https://html.spec.whatwg.org/multipage/embedded-content.html#update-the-image-data
[bzbug]: https://bugzilla.mozilla.org/show_bug.cgi?id=599975