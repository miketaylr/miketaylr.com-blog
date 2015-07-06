---
layout: post
title:  Upcoming changes to the Firefox for Android UA string
date:   2015-07-06
---

If there's one thing that developers love more than the confusing nature of User Agent strings, it's when they change. 

Great news, everybody.

Beginning in Firefox for Android 41, the default UA string will contain the Android version in the platform token ([bug here][bug]):

`Mozilla/5.0 (Android <Android version>; Mobile; rv:<Gecko version>) Gecko/<Gecko version> Firefox/<Gecko version>`

And perhaps the only things that developers love more than changing UA strings is when changes **conditionally**.

So, for interoperability with the [wild and crazy web][bc], if a user is on a version of Android lower than 4 (which we still support), we will report the Android version as 4.4. Versions 4 and above will accurately reflect the Android version.

And in case you've forgotten, Firefox for Android is the same across Android versions, so sniffing the version won't tell you if we do or do not support the latest cool feature.

As always, send all complaints to our [head of customer satisfaction][mike].

[bc]: https://bugzilla.mozilla.org/show_bug.cgi?id=1164877#c0
[mike]: https://avatars1.githubusercontent.com/u/67283?v=3&s=460
[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=1169772