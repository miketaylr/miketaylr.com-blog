---
layout: post
title:  ReferenceError onTouchStart is not defined jquery.flexslider.js
date:   2015-04-29
---

I was supposed to write this blog post like a year ago, but have been very busy in the last 12 months not writing this blog post. But yet here we are.

Doing some compatibility research on top Japanese sites, [I ran into my old nemesis][jp]: `ReferenceError: onTouchStart is not defined jquery.flexslider.js:397:12`.

I first ran into this in [January of 2014][bug] in its more spooky form `ReferenceError: g is not defined`. Eventually I figured out it was a [problem in a WooThemes jQuery plugin called FlexSlider][tick], the real bug being the undefined behavior of function declaration hoisting in conditions (everyone just nod like that makes sense).

In [JavaScript-is-his-co-pilot Juriy's words][kangax],

    Another important trait of function declarations is that declaring them conditionally is non-standardized and varies across different environments. You should never rely on functions being declared conditionally and use function expressions instead.

In this case, they were conditionally declaring a function, but referencing it before said declaration in the if block, as an event handler, i.e.,

```js
if (boop) {
  blah.addEventListener("touchstart", wowowow);

  function wowowow() {}
}
```

No big deal, easy fix. I wrote a patch. Some people manually patched their sites and moved on with their lives. I tried to.

We ran into this a few more times in [Bugzilla][fp] and [Webcompat.com][wc] land.

TC-39 [bosom buddies][bb] Brendan and Allen [noted][c8] [that][c9] ES6 specifies things in such a way that these sites will eventually work in all ES<strike>6</strike>2015 compliant browsers. Here's the [bug][sm] to track that work in SpiderMonkey.


Cool! Until then, my lonely pull request is still hanging out at [https://github.com/woothemes/FlexSlider/pull/986][pr] (16 months later). The good news is FlexSlider is open source, so you're allowed to fix their bugs by manually apply that patch on your site. Then your touch-enabled slider widget stuff will work in Mobile Firefox browsers.

[jp]: https://github.com/webcompat/web-bugs/issues/1008#issuecomment-97145732
[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=936433
[tick]: https://github.com/woothemes/FlexSlider/issues/958
[kangax]: http://kangax.github.io/nfe/
[fp]: https://bugzilla.mozilla.org/show_bug.cgi?id=973463
[wc]: https://github.com/webcompat/web-bugs/issues/145
[c8]: https://bugzilla.mozilla.org/show_bug.cgi?id=973463#c8
[c9]: https://bugzilla.mozilla.org/show_bug.cgi?id=973463#c9
[pr]: https://github.com/woothemes/FlexSlider/pull/986
[sm]: https://bugzilla.mozilla.org/show_bug.cgi?id=950547
[bb]: https://miketaylr.com/posts/assets/brendan-and-allen.jpg