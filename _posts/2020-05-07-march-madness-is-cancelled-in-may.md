---
layout: post
title:  March Madness 2020 is cancelled (in May)
date:   2020-05-05
---

Welcome to May 2020, where everything is terrible.

Let's take a look at a [bug reported against a site][wc] rendered irrelevant by the world we find ourselves currently living in, one where March Madness 2020 was cancelled.

That site is [bracketchallenge.ncaa.com][bc], which I've never used, but I gather is fun cross between Pogs and college basketball. But sponsored by a Fortune 500 American bank.

If you were to visit that site in a non-Chrome browser on Android, you'd see this really scary message:

<img src="https://miketaylr.com/posts/assets/ncaa.png" style="border:1px solid #ccc" width="70%" alt="Screenshot that reads: Incompatible Device. Your device default browser has security flaws that allow for exploits of authentication.  Please install the Chrome Browser to safely sign up or log in">

Whoa! I'm super interested to find out about these security issues, so we can get them fixed for Firefox Mobile users. So let's dig in.

The first clue to figuring out what's up here is in the HTML tag:

```html
<html class="is-old-android">
```

Digging around in their application I see that if a global variable `Et` is truthy, then this scary modal is displayed. `Et` is defined as `t.is_old_android`. And `t` is just the global object which has some cool expandos on it (and I only made up one of them, believe it or not):

```js
ie_version: 9999,
isFirefox: true,
is_2_android: false,
is_android: true,
is_old_android: true,
is_2_android_final_backup: false,
```

Given that I tested this site on a Pixel 3 running Android 10, I'm slightly skeptical that I'm actually on "old Android". But let's take a look at how that value is computed, because I've been wrong before!

```js
t.is_old_android = t.is_android && !/Chrome/g.test(e),
```

Oh, right. It's not so much old as "old" (wink, wink).

So what are the "security flaws that allow for exploits of authentication" in non-Chrome Mobile browsers? The first rule of [playing basketball pogs][pogs], it turns out, is to just fabricate nonsense. Actually, I can see why children and adults (and banks!) love March Madness so much.

Maybe I'll check it out next year.

[wc]: https://github.com/webcompat/web-bugs/issues/49886
[bc]: https://bracketchallenge.ncaa.com/
[pogs]: https://en.wikipedia.org/wiki/Milk_caps_(game)#Gameplay