---
layout: post
title:  Chrome 100 Breakage Playbook
date:   2022-03-21
---

If you somehow found this blog post because you googled or binged "site not working Chrome 100", well, congrats my SEO trap worked successfully.

Also, don't panic.

The quickest way to test if your site is broken due to a 3-digit version parsing bug is to temporarily enable the `chrome://flags/#force-major-version-to-minor` flag and restart the browser. This will change the version that Chrome reports in the User-Agent string and header from <mark>100</mark>.0.4896.45 (or whatever the real version number will be) to 99.<mark>100</mark>.4896.45. If the site works again, you know you have a UA string parsing bug. Congrats again!

(Also, test your site in [Firefox Nightly][fx] - not all three digit parsing bugs will affect both Chromium browsers and Firefox, but it's good to verify in case you need to fix your bugs in multiple places.)

At this point, please file a bug at [crbug.com/new][bug]. That will automatically cc me. Or just feel free to [tweet at me][tweet] or email me. Swing by the house if you want, but we have dinner around 6pm. After dinner is better.

Or, best yet, just fix your site bugs without me being in the loop and I will be so proud of you.

<img alt="a hand-drawn star that says good job" src="https://miketaylr.com/posts/assets/goldstar.png" style="border:1px solid #ccc; width: 100%">


[tweet]: https://twitter.com/miketaylr
[bug]: https://bugs.chromium.org/p/chromium/issues/entry?template=Defect&cc=miketaylr@chromium.org
[fx]: https://www.mozilla.org/en-US/firefox/channel/desktop/