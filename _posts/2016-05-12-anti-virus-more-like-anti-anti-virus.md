---
layout: post
title: MITM compatibility issues
date: 2016-05-12
---

(Alternate title: Kaspersky is one typo away from being called Kaspesky)

[Bug 1271875][bug] is an interesting case of a compat issue not caused by a website, or a browser, but by a 3rd party. In this case, Kaspersky AntiVirus.

Apparently they [Malcolm in the Middle][MITM] you to keep you safe:

<img style="width: 75%; height: 75%;" src="https://miketaylr.com/posts/assets/cert.png" alt="screenshot of facebook.com cert">

I guess that's normal for Anti-Virus programs?

(Personally I stay safe via a combination of essential oils and hyper-link homeopathy.)

Anyways, the issue is that Facebook just turned on [Brotli compression][brotli] for some of their HTML resources. Which is great! [Firefox supports that since v44][hacks], and it makes Facebook faster for its users.

Kaspersky's MITM happily sends `Accept-Encoding: br` in the request but strips `Content-Encoding: br` from the response. Suddenly Facebook looks like ISIS is trying to hack you:

<img style="width: 75%; height: 75%;" src="https://miketaylr.com/posts/assets/brotlibook.png" alt="screenshot of facebook.com all jacked up">

So, in this instance, if Facebook.com looks like binary garbage in your Firefox (and you have Kaspersky AV installed), consider a new anti-virus strategy (ideally they'll also have an update very soon if you're somehow stuck with it).

[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=1271875
[MITM]: https://en.wikipedia.org/wiki/Man-in-the-middle_attack
[brotli]: https://github.com/google/brotli
[hacks]: https://hacks.mozilla.org/2015/11/better-than-gzip-compression-with-brotli/