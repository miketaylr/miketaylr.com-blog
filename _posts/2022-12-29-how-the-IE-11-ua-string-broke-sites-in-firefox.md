---
layout: post
title:  Remember when the IE 11 User-Agent forced Mozilla to freeze part of its User-Agent string (last week)
date:   2022-12-29
---

If you happen to be using Firefox Beta 109 on an overpriced MacBook Pro that has a sticky letter s today (the 29th of December, 2022), this is what the User-Agent string looks like:

`Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/109.0`

And as of [last week][bug], the UA string in Firefox for versions 110 and higher look like:

`Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/110.0`

If you managed to visually discover the difference (I guess in the world's lamest "Spot the Difference" game), congrats. If you didn't, take note that `rv:109.0` _did not_ change in the second one&mdash;but `Firefox/110.0` _did_.

So why did Mozilla just freeze `rv:109.0` in the User-Agent string? Perhaps forever, or just [perhaps until Firefox 120 is released][bug2]?

Presumably in an attempt to unburden itself from a legacy of UA-sniffing-driven workarounds for a browser that hadn't historically supported a lot of useful things (like WebGL, or some ES5 or ES6 stuff - I don't really remember and can't be bothered to look it up), the IE team decided to change up their User-Agent string back in 2013.

Here's the IE10 UA, which followed the same-ish predictable pattern they had since the IE 2.0:

`Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; Trident/6.0)`

And here's an IE11 UA:

`Mozilla/5.0 (Windows NT 6.1; WOW64; Trident/7.0; AS; rv:11.0) like Gecko`

Basically they were trying to solve the problem of "now that we've invested seriously in web standards, how do we get access to content that makes use of those features, and still have a detectable version number for analytics (or whatever). And it makes sense to borrow Firefox's `rv:` convention (slapping an extra "like Gecko" in there for good luck can't hurt, I suppose (but more realistically, there was probably some bank or government site that sniffed for Safari's `like Gecko` token)).

And then cut to today, about 9 years later where a [handful of sites][110] (including popular ones like bestbuy and cvs) tell Firefox users to upgrade to a modern browser, because there's probably something really clever like `var isIE = /rv:11/i.test(navigator.userAgent);`.

That's obviously lame, and hence, Mozilla has frozen another part of its UA string for compatibility. Anyways, happy new years, especially to the folks working to make sure the web is still usable in Firefox.


 [bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=1805967
 [bug2]: https://bugzilla.mozilla.org/show_bug.cgi?id=1806690
 [c14]: https://bugzilla.mozilla.org/show_bug.cgi?id=1805967#c14
 [ieua]: https://web.archive.org/web/20080221094426/http://msdn2.microsoft.com/en-us/library/ms537509.aspx
 [110]: https://github.com/webcompat/web-bugs/labels/version110