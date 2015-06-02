---
layout: post
title:  Firefox for iOS User Agent String
date:   2015-06-02
---

A few months back we [settled on a UA String for Firefox on iOS][bug]. The universe still seems (mostly) intact so it should probably be documented.

Similar to [Chrome for iOS][crdocs], we're using the default Safari Mobile UA string with an additional `FxiOS/<FxiOSVersion>` token. For example,

`Mozilla/5.0 (iPhone; CPU iPhone OS 8_3 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) FxiOS/1.0 Mobile/12F69 Safari/600.1.4`

Here's a handy FAQ:

Q. As a web developer, what should I do now that I know this?
A. Probably nothing.

[crdocs]: https://developer.chrome.com/multidevice/user-agent#chrome_for_ios_user_agent
[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=1147658