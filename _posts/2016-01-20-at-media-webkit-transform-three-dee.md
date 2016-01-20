---
layout: post
title:  🙅 @media (-webkit-transform-3d)
date:   2016-01-20
---

`@media (-webkit-transform-3d)` is a funny thing that exists on the web.

It's like, a [media query feature][mqf] in the form of a prefixed CSS property, which should tell you if your (once upon a time probably Safari-only) browser supports 3D transforms, invented back in the day before we had [`@supports`][sup].

(According to [Apple docs][docs] it first appeared in Safari 4, along side the other `-webkit-transition` and `-webkit-transform-2d` hybrid-media-query-feature-prefixed-css-properties-things that you should immediately forget exist.)

Older versions of Modernizr [used this (and only this)][oldm] to detect support for 3D transforms, and that seemed pretty OK. (They also did the polite thing and tested `@media (transform-3d)`, but no browser has ever actually supported that, as it turns out). And because they're so consistently polite, they've since [updated the test][test] to prefer `@supports` too (via a pull request from Edge developer Jacob Rossi).

As it turns out other browsers have been [updated to support 3D CSS transforms][caniuse], but sites didn't go back and update their version of Modernizr. So unless you support `@media (-webkit-transform-3d)` these sites break. Niche websites like [yahoo.com][yahoo] and [about.com][wc].

So, anyways. I added [`@media (-webkit-transform-3d)` to the Compat Standard][spec] and we [added support for it Firefox][fx] so websites stop breaking.

But you shouldn't ever use it&mdash;use `@supports` or [`matchMedia`][mm] instead. In fact, don't even share this blog post. Maybe delete it from your browser history just in case.

[spec]: https://compat.spec.whatwg.org/#css-media-queries-webkit-transform-3d
[docs]: https://developer.apple.com/library/safari/documentation/AppleApplications/Reference/SafariCSSRef/Articles/OtherStandardCSS3Features.html#//apple_ref/doc/uid/TP40007601-SW3
[mqf]: https://drafts.csswg.org/mediaqueries-4/#mq-features
[sup]: https://developer.mozilla.org/en-US/docs/Web/CSS/@supports
[mm]: https://developer.mozilla.org/en-US/docs/Web/API/Window/matchMedia
[test]: https://github.com/patrickkettner/Modernizr/commit/a54308e47e269a058472854b1ef417bd54f4e616
[wc]: https://github.com/webcompat/web-bugs/issues/2151
[yahoo]: https://bugzilla.mozilla.org/show_bug.cgi?id=1239136
[fx]: https://bugzilla.mozilla.org/show_bug.cgi?id=1239799
[oldm]: https://github.com/Modernizr/Modernizr/blob/66c694d136241d356e0d24fcbaa5c068b0b0cdae/feature-detects/css/transforms3d.js#L26-L27
[caniuse]: http://caniuse.com/#feat=transforms3d