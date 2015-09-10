---
layout: post
title:  initTouchEvent is a rat's nest.
date:   2015-09-10
---

Reading my bugmail this morning, as one does, I came upon a [comment by my colleague Hallvord in a bug][bug] about FoxNews video not working in Firefox for Android due to this cool error `"Argument 10 of TouchEvent.initTouchEvent is not an object."`.

(Arguably FoxNews video not working is a competitive advantage in the mobile browser market landscape. But I don't work in marketing.)

Alright, let's check the [spec][spec] for `initTouchEvent` and see what's up. Oh wait, this method isn't specced. Coooool:

> Some user agents implement an initTouchEvent method…The initTouchEvent method is not standardized and is superseded by the TouchEvent constructor.

So that's how we end up with [awesome stuff like this][lol], forever and ever and ever.

```js
if (touchEvent && touchEvent.initTouchEvent) {
  if (touchEvent.initTouchEvent.length == 0) { //chrome
    touchEvent.initTouchEvent(a,b,c,d,e,f,g,h,i);
} else if ( touchEvent.initTouchEvent.length == 12 ) { //firefox
    touchEvent.initTouchEvent(a,b,c,d,e,f,g,h,i,j,k,l);
} else { //iOS length = 18
    touchEvent.initTouchEvent(a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r);
  }
}
```

That probably looks like garbage on anything that isn't an iPad Pro, so here's a screenshot of the original.

<img src="https://miketaylr.com/posts/assets/barf.png" style="border:1px solid #ccc" alt="Screenshot of some gross code.">

Anyways kids, `TouchEvent` constructors are the future. We've got too many nests for rats as it is.

[spec]: https://w3c.github.io/touch-events/
[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=1043592#c10
[lol]: https://github.com/Huii/develop_with_vr/blob/78599da0e0469743fc33013d7bd3cdd64cabfa11/src/renderer/events/touch.js#L66-L81