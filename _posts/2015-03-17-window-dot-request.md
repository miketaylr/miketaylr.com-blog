---
layout: post
title:  Renaming your homebrew window.Request
date:   2015-03-17
---

If you've defined your own global `window.Request` object and have users running Firefox 39 and Chrome 42 (and Opera and soon others), you're gonna have a bad time.

[Webcompat issue #793][bug] details how dailymotion.com breaks (thankfully the videos of awesome Japanese public toilets still work, but all the sidebar content is missing) because they define their own `Request` object.

`Uncaught TypeError: Request.getHashParams is not a function.`

So, anyways. If you're defining your own `window.Request` your code is going to break and you should pick a new global identifier. Here's a few suggestions inspired by mid-March conference synergy-fests in Austin, TX:

`window.Oppressed`
`window.WaspsNest`
`window.Unimpressed`
`window.SouthBySouthDepressed`

Picking any one of those should fix the bugs you're about to have.

[bug]: https://github.com/webcompat/web-bugs/issues/793