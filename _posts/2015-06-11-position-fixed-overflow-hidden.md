---
layout: post
title:  position&colon; fixed  + overflow&colon; hidden + (plus some relative positioning and z-index stuff)
date:   2015-06-11
---

If you have an element with `position: fixed` *inside* of an element that has `overflow: hidden`, what's the expected rendering when you need to, uh, overflow? Should the inner fixpos element be clipped or not?

The [spec appears to be pretty clear][spec].

> Fixed positioning is similar to absolute positioning. The only difference is that for a fixed positioned box, the containing block is established by the viewport.

So, according that spec text, the parent element's overflow shouldn't have any effect because the fixpos' parent element is the viewport.

Neat. But how do browsers behave? Open this testcase and have a look:

[https://miketaylr.com/bzla/fixed-overflow-1.html][tc1]

Everything behaves as the spec describes. `overflow: hidden` on the parent is ignored. High-fives all around.

Now if you throw in *both* a `z-index: 1` (any number will do) and a `position: relative` on the parent element, things get...different.

[https://miketaylr.com/bzla/fixed-overflow-2.html][tc2]

**Same as first testcase (what I would expect)**:
Mobile + Desktop Firefox
Mobile Chrome
Desktop Edge

**Different from first testcase**:
Mobile + Desktop Safari: fixpos element is clipped by parent (meaning `overflow: hidden` worked).
Mobile + Desktop Opera (Presto): same as Safari
Desktop Chrome: *if* the viewport is smaller than the containing parent, then `overflow: hidden` on the parent kicks in (resize the browser window to see it).
Mobile + Desktop Opera (Blink): same as Chrome

And now, a 3rd testcase which adds `user-scalable=no` to the meta (viewport) element (the same effect happens if you constrain `intial-scale` and `maximum-scale` to 1.

[https://miketaylr.com/bzla/fixed-overflow-3.html][tc3]

The only browser this seems to make a difference in is Chrome on Android, which now clips the child element. I think I discovered this on accident.

So it seems like non-Edge and non-Firefox browsers treat a `position: fixed` element as a `position: absolute` element (or something?), when contained by a `position: relative` parent that also has a `z-index` set.

Unfortunately at least one site relies on this bug (see [this comment][bug]).

If you happen to know why, send a self-addressed stamped envelope to [twitter dot com slash miketaylr][tw] and let me know.

[spec]: http://dev.w3.org/csswg/css-position/#fixed-pos
[tc1]: https://miketaylr.com/bzla/fixed-overflow-1.html
[tc2]: https://miketaylr.com/bzla/fixed-overflow-2.html
[tc3]: https://miketaylr.com/bzla/fixed-overflow-3.html
[bug]: https://github.com/webcompat/web-bugs/issues/917#issuecomment-106949164
[tw]: https://twitter.com/miketaylr