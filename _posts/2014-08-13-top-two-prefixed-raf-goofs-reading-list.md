---
layout: post
title:  prefixed requestAnimationFrame greatest hits
date:   2014-08-13
---

Vendor prefixes are a funny thing. Mix them with global WebIDL own properties and it gets weird.

From [blink-dev][blinkdev],

``` js
var requestAnimationFrame = window.webkitRequestAnimationFrame ||
    window.opRequestAnimationFrame ||
    window.mozRequestAnimationFrame ||
    window.msRequestAnimationFrame ||
    window.requestAnimationFrame;
```

If you run that in Chrome it probably won't do what you think it might.

``` js
> requestAnimationFrame
function webkitRequestAnimationFrame() { [native code] }
```

So don't hold your breath waiting for `webkitRequestAnimationFrame()` to get removed any time soon.

Now from [bugzilla][bugzilla],

``` js
var requestAnimationFrame = function (win, t) {
      return win["webkitR" + t] || win["r" + t] || win["mozR" + t]
          || win["msR" + t] || function (fn) {
          setTimeout(fn, 1000 / 60)
      }
  }(window, "equestAnimationFrame");
```

Let me quote [bz][bz],

>Before [bug 932322][932322], this used to shadow the default requestAnimationFrame with undefined and therefore pick up the mozRequestAnimationFrame.  Now it picks up the default requestAnimationFrame.
>
>In Chrome, it picks up webkitRequestAnimationFrame, since the code puts that before the standard version, unlike mozRequestAnimationFrame.
>
>If I comment out this code so that the standard requestAnimationFrame is used in all browsers, the page stops working in Chrome as well.  If instead I change that code to put the moz-prefixed version next to the webkit-prefixed one, so it's picked up in preference to the standard one, then the page works in Firefox.

And now I'll quote [Erik Arvidsson][erik] from the blink-dev thread as to the why for both (ignoring the `DOMHighResTimeStamp` problem in the bugzilla issue),

> Remember that VariableDeclarations hoist and are initialized to
undefined, as well as all global bindings become own properties on the
global object (which is the window in a web browser).
>
> There are a few solutions (Adam mentioned one).
>
> 1. Do not use global variables. The original code is fine inside a
function scope.
> 2. Use `window.requestAnimationFrame = ...`
> 3. You can also skip the var if you are in non-strict mode.

Fun times on the internet.


[blinkdev]: https://groups.google.com/a/chromium.org/forum/#!topic/blink-dev/5S7qeLSXT5Q
[bugzilla]: https://bugzilla.mozilla.org/show_bug.cgi?id=1048196#c2
[932322]: https://bugzilla.mozilla.org/show_bug.cgi?id=932322
[bz]: https://twitter.com/bz_moz
[erik]: https://twitter.com/ErikArvidsson