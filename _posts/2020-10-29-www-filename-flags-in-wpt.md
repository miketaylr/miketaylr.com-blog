---
layout: post
title:  .www filename flags in web-platform-tests
date:   2020-10-29
---

I added a small feature to [web-platform-tests][wpt] that allows you to load a test automatically on the `www` subdomain by using a `.www` [filename flag][flags] (here's the [original issue][issue]).

So like, if you ever need to load a page on a different subdomain to test some kind of origin-y or domainy-y thing, you can just name your test something amazing like `origin-y-test.www.html` and it will open the test for you at `www.web-platform.test` (rather than `web-platform.test`, or similarly, however your system or server is configured).

Now you'll never need to embed an `<iframe>` or call `window.open()` ever again (unless you actually need to do those things).

🎃

[wpt]: https://web-platform-tests.org/index.html
[flags]: https://web-platform-tests.org/writing-tests/file-names.html
[issue]: https://github.com/web-platform-tests/wpt/issues/20932