---
layout: post
title: Google Tier 1 Search in Firefox for Android Nightly
date: 2018-07-03
---

Late last week we [quietly landed][bug] a Nightly-only addon that spoofs the Chrome Mobile user agent string for Google Search (well, Facebook too, but that's another blog post).

Why?

[Bug 975444][bug2] is one of the most-duped web compat bugs, which documents the fact that the version of Google Search that Firefox for Android users receive is a less rich version than the one served to Chrome Mobile. And people notice (hence all the dupes).

In order to turn this situation around, we've been working on a number of [platform][platform] [interop][interop] bugs (in collaboration with some friendly members of the Blink team) and have hopes in making progress towards receiving Tier 1 search by default. 

Part of the plan is to sniff out bugs we don't know about (or new bugs, as the site changes very quickly) by exposing the Nightly population to the spoofed Tier 1 version for 4 weeks (which should be [July 27, 2018][backout]). If things get too bad, we can back out the addon earlier.

If you've found a bug, please report it at <a href="https://webcompat.com/issues/new">https://webcompat.com/issues/new</a>. 

And in the meantime, if the bugs are too annoying to deal with, you can disable it by going to `about:config` and setting `extensions.gws-and-facebook-chrome-spoof.enabled` to false (just search for `gws`).

**Note: don't hit reset; instead, tap the `true`/`false` value and then hit toggle when that appears.**

<img style="width: 75%; height: 75%; border: 1px solid #ccc;" src="https://miketaylr.com/posts/assets/about-config.png">

(yeah, yeah, I'll go charge my phone now.)


[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=1453691#c52
[bug2]: https://bugzilla.mozilla.org/show_bug.cgi?id=975444
[interop]: https://github.com/webcompat/web-bugs/labels/type-GWS-interop
[platform]: https://bugzilla.mozilla.org/buglist.cgi?list_id=14217487&status_whiteboard_type=anywordssubstr&status_whiteboard=%5Bwebcompat%3Ap1%5D%20%5Bwebcompat%3Ap2%5D&resolution=---&query_format=advanced&bug_status=UNCONFIRMED&bug_status=NEW&bug_status=ASSIGNED&bug_status=REOPENED&bug_status=RESOLVED&bug_status=VERIFIED&bug_status=CLOSED
[filter]: about:config?filter=gws
[backout]: https://bugzilla.mozilla.org/show_bug.cgi?id=1472220
