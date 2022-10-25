---
layout: post
title:  How to get the Chrome major version from the User-Agent or UA-CH headers
date:   2022-10-24
---

Over the weekend frontend-firebrand [Alex Russell tweeted][art] that it's confusing these days to know how to get the major version of a Chromium browser, either via the `User-Agent` header (given [User-Agent Reduction][uar]), or from [User-Agent Client Hints][uach].

(btw, I asked DALLE2 Alex's question "Can *you* make hide or hare of this?", and yeah, it mostly can.)

<img alt="a photorealistic AI-generated image of a rabbit in a field. both of its ears appear to be on the same side of its head" width="600" height="475" src="https://miketaylr.com/posts/assets/hide-or-hair.png" style="border:1px solid #ccc; width: 100%">

Before I spend any energy improving the docs ([thanks for the bug, Alex][bug]), let me try to give the simplest answer to the question: how do I find out what major version of Chromium a user is one in the year 2022+?

There's two ways to go about this.
<br>
### User-Agent

In Chrome 101, we shipped the terribly named "[Phase 4][p4]" (that's on me) part of the User-Agent Reduction project. Prior to version 101, you would see something in the User-Agent (either via HTTP or reflected by `navigator.userAgent`) that looks like so:

`"Chrome/100.0.4896.75"`

In Phase 4, we reduced the MINOR.BUILD.PATCH portions of the version to the constant "0.0.0":

`"Chrome/101.0.0.0"`

But in terms of browser version changes coming to the UA string, that's it. So if your use case is simple and you just need the major version of the browser, you can continue to parse the UA string as before (hopefully using an actively maintained project like [ua-parser][uap])&mdash;it will continue to be updated for each new major version update (as I write this, my browser reports `Chrome/106.0.0.0`).

<br>
### User-Agent Client Hints

If you want to get slightly fancier, and ditch your existing UA parsing library dependency (but maybe pick up a [structured headers][sh] [parsing library][shnpm] dependency), you can get the Chrome version from the new `Sec-CH-UA` header.

`Sec-CH-UA: "Chromium";v="106", "Google Chrome";v="106", "Not;A=Brand";v="99"`

Admittedly, it looks kinda weird. But the good news is you don't have to care about Client Hints to receive the header&mdash;it's sent by default. Feel free to consult [https://web.dev/migrate-to-ua-ch/][m] if you do have to care about those.

The other cool thing is you can use the new `navigator.userAgentData.brands` API to get the equivalent of `Sec-CH-UA` header and do something like the following (maybe working around a known bug) and it will work in Chrome, Chromium, Edge, Opera, Brave, etc.

```js
navigator.userAgentData.brands.some(
  item => item.brand == "Chromium" && item.version > 105
)
```

So that's it. If you need the major version of a Chromium browser you can keep using `User-Agent`, or use `Sec-CH-UA`.

But if you need the _full version_ of the browser, or the OS, that's a different blog post I'll get to once someone tweets their way onto my TODO list.

[art]: https://twitter.com/slightlylate/status/1583917784676806656
[uach]: https://wicg.github.io/ua-client-hints/
[uar]: https://blog.chromium.org/2021/05/update-on-user-agent-string-reduction.html
[uap]: https://github.com/ua-parser
[shnpm]: https://www.npmjs.com/package/structured-headers
[sh]: https://www.rfc-editor.org/rfc/rfc8941.html
[m]: https://web.dev/migrate-to-ua-ch/
[bug]: https://github.com/GoogleChrome/web.dev/issues/8916
[p4]: https://www.chromium.org/updates/ua-reduction/#sample-ua-strings-phase-4
