---
layout: post
title:  Introducing webcompat.com
date:   2014-04-22
---

For the past couple of months&mdash;together with [Alexa Roman][alexa] and the rest of the [MozWebCompat][moz] crew&mdash;I've been working on getting [webcompat.com][wc] to something much closer to a hyperlinkable state. Today I think it's probably good enough to remove the lame "coming soon" page (designed by yours truly; apologies) that used to exist and ship.

So how does this work?

1) [Internet][surf]
2) Site/app/lib/whatever works in cool-guy browser A but not cool-guy browser B
3) Get a little emotional, it's okay
4) Report an issue at [webcompat.com][wc]

You can report with your GitHub credentials if you're into that, or if the required "public_repo" OAuth scope freaks you out you can report anonymously via one of our sweet GitHub [proxy][nep] [bots][gig].

At this point, (in theory) someone will be able to diagnose what's going on and reach out to the site to advocate a fix or submit a patch, etc. If it's browser bug territory we want be able to upstream issues to a browser vendor bug tracker (see [issue #84][84]).

Right now all issues are collected in a "web-bugs" GitHub repo: [https://github.com/webcompat/web-bugs/issues][bugs] but we'll soon be working on displaying those on webcompat.com itself to make it easier to understand what issues have been reported for which domains or browsers, etc.

So. If any of that was remotely interesting to you, and if you're into open source, Python, HTML, CSS, JS, or whatever, there's [work to be done][issues].

Happy bug reporting.

[wc]: http://webcompat.com
[alexa]: http://alexaroman.com/
[moz]: https://twitter.com/mozwebcompat
[issues]: https://github.com/webcompat/webcompat.com/issues?state=open
[bugs]: https://github.com/webcompat/web-bugs/issues
[surf]: https://miketaylr.com/posts/assets/surf.jpg
[84]: https://github.com/webcompat/webcompat.com/issues/84
[gig]: https://github.com/GIGANTOR
[nep]: https://github.com/Neptr