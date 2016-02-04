---
layout: post
title: A quiz about ES2015 block-scoped function declarations (in a with block statement)
date:   2016-02-04
---

Quiz time, nerds.

Given the following bit of JS, what's the value of `window.f` when `lol` gets called outside of the `with` statement?

```js
with (NaN) {
  window.f = 1;
  function lol(){window.f = 2};
  function lol(){window.f = 3};
}
lol()

```
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.

Trick question, your program crashed before it got to call `lol()`!

According to ES2015, it should be a `SyntaxError`, because you're redefining a function declaration in the same scope. Just like if you were re-declaring a `let` thingy more than once.

However, the *real* trick is that Chrome and Firefox will just ignore the first declaration so your program doesn't explode (for now, anyways). So the answer is really just `3` (which you probably guessed).

Double-tricked!

Not surprisingly there are sites out there that depend on this funky declared-my-function-twice-in-the-same-scope pattern. webex.com was one ([bug here][Fx]), but they were super cool and fixed their code already. The Akamai Media Player on foxnews.com ([bug here][Chrome]) is another (classic foxnews.com move).

It would be really cool if browsers didn't have to do this, so if you know anybody who works on the [Akamai Advanced Media Player][AMP], tell them to delete the second declaration of `RecommendationsPlugin()`? And if you see an error in Firefox that says `redeclaration of block-scoped function 'coolDudeFunction' is deprecated`, go fix that too &mdash; it might stop working one day.

Now don't forget to like and subscribe to my Youtube ES2016 Web Compat Pranks channel.

(The `with(NaN){}` bit isn't important, but it was lunch time when I wrote this.)

[Chrome]: https://code.google.com/p/chromium/issues/detail?id=579395
[Fx]: https://bugzilla.mozilla.org/show_bug.cgi?id=1235590
[AMP]: https://www.akamai.com/us/en/solutions/products/media-delivery/adaptive-media-player.jsp