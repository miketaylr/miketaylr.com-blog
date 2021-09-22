---
layout: post
title:  Testing Chrome version 100 for fun and profit (but mostly fun I guess)
date:   2021-09-21
---

Great news readers, my self-imposed 6 month cooldown on writing amazing blog posts has expired.

My pal [Ali][ali] just added a flag to Chromium to allow you to test sites while sending a User-Agent string that claims to be version 100 (should be in version 96+, that's in the latest [Canary][can] if you download or update today):

<img alt="screenshot of chrome://flags/#force-major-version-to-100" src="https://miketaylr.com/posts/assets/chrome-100.png" style="border:1px solid #ccc; width: 100%">

I'll be lazy and let Karl Dubost do the explaining of the why, in his post ["Get Ready For Three Digits User Agent Strings"][karl].

So turn it on and report all kinds of bugs, either at [crbug.com/new][cr] or [webcompat.com/issues/new][wc].

[ali]: https://twitter.com/alibeyad
[cr]: https://crbug.com/new
[wc]: https://webcompat.com/issues/new
[karl]: https://www.otsukare.info/2021/04/20/ua-three-digits-get-ready
[can]: https://www.google.com/chrome/canary/