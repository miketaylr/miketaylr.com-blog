---
layout: post
title: Report Site Issue button in Firefox Nightly
date: 2017-01-12
---

In [Bug 1324062][bug] we're landing a new button to the default hamburger menu panel in Firefox Nightly, like we did in [Firefox for Android Nightly and Aurora][fennec]. For now, the plan is for this feature to be Nightly-only, maybe one day graduating up to Beta.

If you click it, Firefox will take a screenshot of the page you're on and open the new issue form on [webcompat.com][wc] (you can remove the screenshot before submitting if you prefer). You can then report an issue with your GitHub account, if you have one ([they're free!][gh]), or anonymously.

<img src="http://miketaylr.com/posts/assets/report-site-issue.png" style="width:75%" alt="screeshot of the Report Site Issue button in Firefox">

What this does is allow our Nightly population to more easily report compatibility issues, which in turn helps us discover regressions in Firefox, find bugs in sites or libraries, and understand what APIs and features the web relies on (standard and non-standard).

What's a compat issue? In a nutshell, when a site works in one browser, but not another. You can [read more about that on the Mozilla Compat wiki page][wiki] if you're interested.

If you find bugs related to this new button, or have meaningful feedback [please file a bug][wcg]!

(Also if you would prefer it to live somewhere else in your UI, you can click the Customize button in the menu panel and go wild, or hide it from sight.)

[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=1324062
[wc]: https://webcompat.com
[gh]: https://github.com/join
[wiki]: https://wiki.mozilla.org/Compatibility/Guide#What.3F
[wcg]: https://bugzilla.mozilla.org/enter_bug.cgi?product=Web%20Compatibility&component=General
[fennec]: https://miketaylr.com/posts/2014/10/report-site-issue-in-firefox-for-android-nightly.html