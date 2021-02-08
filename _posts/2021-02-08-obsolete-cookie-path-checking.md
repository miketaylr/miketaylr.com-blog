---
layout: post
title:  Obsolete RFCs and obsolete Cookie Path checking comments
date:   2021-02-08
---

The other day I was reading Firefox's [CookieService.cpp][cs] to figure out how Firefox determines its maximum cookie size (more on that one day, maybe) when the following comment (from 2002, according to blame) caught my eye:

```
The following test is part of the RFC2109 spec.  Loosely speaking, it says that a site cannot set a cookie for a path that it is not on.  See bug 155083.  However this patch broke several sites -- nordea (bug 155768) and citibank (bug 156725).  So this test has been disabled, unless we can evangelize these sites.
```

Note 1: Anything having to do with broken websites is wont to catch my attention, especially olde bugs (let's face it, in 2002 the internet was basically the High Middle Ages. Like yeah, we were killing it with the technological innovation on top of windmills and we're getting pretty good at farming and what not, but it's still the Middle Ages compared to today and kind of sucked).

Note 2: The two sites referenced in the Firefox comment are banks (see [155768][155768] and [156725][156725]). And one of the axioms of web compatibility is that if you break a bank with some cool new API or non-security bug fix, game over, it's getting reverted. And I'm *pretty sure* you can't _legally_ create test accounts for banks to run tests against and [Silk Road][sr] got taken down by the feds.

But at the time, the now obsolete [rfc2019][r] had this to say about cookies `Path` attributes:

```
To prevent possible security or privacy violations, a user agent rejects a cookie (shall not store its information) if any of the following is true:

  * The value for the Path attribute is not a prefix of the request-URI.
```

Well, as documented, that wasn't really web-compatible, and it's kind of a theoretical concern (so long as you enforce path-match rules before handing out cookies from the cookie jar. ¯\\\_(ツ)_/¯). So Firefox commented out the conditional that would reject the cookie and added the comment above in question. As a result, people's banks starting working in Firefox again (in 2002, remmeber, so people could check their online balance then hit up the ATM to buy Beyblades and Harry Potter merch, and whatever else was popular back then).

My colleague Lily pointed out that Chromium has a similar comment in [canonical_cookie.cc][cc]:

```
The RFC says the path should be a prefix of the current URL path. However, Mozilla allows you to set any path for compatibility with broken websites.  We unfortunately will mimic this behavior.  We try to be generous and accept cookies with an invalid path attribute, and default the path to something reasonable.
```

These days, rfc6265 (and [6265bis][6265]) is much more pragmatic and states exactly what Firefox and Chromium are doing:

```
…the Path attribute does not provide any integrity protection because the user agent will accept an arbitrary Path attribute in a Set-Cookie header.
```

Never one to pass up on an opportunity to delete things, I wrote some patches for [Firefox][bz] and [Chromium][cr] so maybe someone reading Cookie code in the future doesn't get distracted.

Aside 1: Awkwardly my moz-phab account has been disabled, so I just attached the patch file using Splinter like it's 2002 (more Medieval code review tech references).

Aside 2: Both of these comments have two spaces after periods. Remember that?

[bz]: https://bugzilla.mozilla.org/show_bug.cgi?id=1691113
[cr]: https://bugs.chromium.org/p/chromium/issues/detail?id=1175151
[r]: https://tools.ietf.org/html/rfc2109
[6265]: https://tools.ietf.org/html/draft-ietf-httpbis-rfc6265bis-07
[cs]: https://searchfox.org/mozilla-central/source/netwerk/cookie/CookieService.cpp
[155768]: https://bugzilla.mozilla.org/show_bug.cgi?id=155768
[156725]: https://bugzilla.mozilla.org/show_bug.cgi?id=156725
[cc]: https://source.chromium.org/chromium/chromium/src/+/master:net/cookies/canonical_cookie.cc
[sr]: https://en.wikipedia.org/wiki/Silk_Road_(marketplace)