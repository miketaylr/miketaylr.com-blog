---
layout: post
title:  Differences in cookie length (size?) restrictions
date:   2020-12-10
---

I was digging through some of the [old http-state tests][old] (which got ported into web-platform-tests, and which I'm rewriting to be more modern and, mostly work?) and noticed an interesting difference between Chrome and Firefox in [disabled-chromium0020-test][dc] (no idea why it's called disabled and not, in fact, disabled).

That test looks something like:

```
Set-Cookie: aaaaaaaaaaaaa....(repeating a's for seemingly forever)
```

But first, a little background on expected behavior so you can begin to care.

[rfc6265][6265] talks about cookie size limits like so:

> At least 4096 bytes per cookie (as measured by the sum of the length of the cookie's name, value, and attributes).

(It's actually trying to say at most, which confuses me, but a lot of things confuse me on the daily.)

So in my re-written version of `disabled-chromium0020-test` I've got (just assume a function that consumes this object and does something useful):

```js
{
  // 7 + 4089 = 4096
  cookie: `test=11${"a".repeat(4089)}`,
  expected: `test=11${"a".repeat(4089)}`,
  name: "Set cookie with large value ( = 4kb)",
},
```

Firefox and Chrome are happy to set that cookie. Fantastic. So naturally we want to test a cookie with 4097 bytes and make sure the cookie gets ignored:

```js
// 7 + 4091 = 4098
{
  cookie: `test=12${"a".repeat(4091)}`,
  expected: "",
  name: "Ignore cookie with large value ( > 4kb)",
},
```

If you're paying attention, and good at like, reading and math, you'll notice that 4096 + 1 is *not* 4098. A+ work.

What I discovered, much in the same way that [Columbus discovered Texas][limits], is that a "cookie string" that is 4097 bytes long currently has different behaviors in Firefox and Chrome (and probably most browsers, TBQH). Firefox (sort of correctly, according to the current spec language, if you ignore attributes) will only consider the `name` length + the `value` length, while Chrome will consider the entire cookie string including `name`, `=`, `value`, and all the attributes when enforcing the limit.

I'm going to include the current implementations here, because it makes me look smart (and I'm trying to juice SEO):

Gecko (which sets `kMaxBytesPerCookie` to 4096):
```cpp
bool CookieCommons::CheckNameAndValueSize(const CookieStruct& aCookieData) {
  // reject cookie if it's over the size limit, per RFC2109
  return (aCookieData.name().Length() + aCookieData.value().Length()) <=
         kMaxBytesPerCookie;
}
```

Chromium (which sets `kMaxCookieSize` to 4096):
```cpp
ParsedCookie::ParsedCookie(const std::string& cookie_line) {
  if (cookie_line.size() > kMaxCookieSize) {
    DVLOG(1) << "Not parsing cookie, too large: " << cookie_line.size();
    return;
  }

  ParseTokenValuePairs(cookie_line);
  if (!pairs_.empty())
    SetupAttributes();
}
```

Neither are really following what the spec says, so [until the spec tightens that down][issue] (it's currently only a SHOULD-level requirement which is how you say pretty-please in a game of telephone from browser engineers to computers), I bumped the test up to 4098 bytes so both browsers consistently ignore the cookie.

(I spent about 30 seconds testing in Safari, and it seems to match Firefox at least for the 4097 "cookie string" limit, but who knows what it does with attributes.)


[old]: https://github.com/abarth/http-state
[dc]: https://github.com/web-platform-tests/wpt/blob/d68891ce207bdc4e0eccd1476a944ff4a772514a/cookies/http-state/resources/test-files/disabled-chromium0020-test#L1
[6265]: https://tools.ietf.org/html/rfc6265#section-6.1
[chrome]: https://searchfox.org/mozilla-central/rev/ffdb6a4d934b3f5294f18cf0e1df618109ccb36b/netwerk/cookie/CookieCommons.cpp#193-197
[fx]: https://searchfox.org/mozilla-central/rev/ffdb6a4d934b3f5294f18cf0e1df618109ccb36b/netwerk/cookie/CookieCommons.cpp#193-197
[issue]: https://github.com/httpwg/http-extensions/issues/1340
[limits]: http://www.ruslog.com/tools/cookies.html