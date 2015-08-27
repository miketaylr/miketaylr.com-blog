---
layout: post
title:  Dynamically updating &lt;meta viewport&gt; in the year 2015.
date:   2015-08-27
---

18 months after writing the [net-ward-winning][netaward] article [Dynamically updating &lt;meta viewport&gt; in the year 2014][2014], I [wrote some patches][bug] for Firefox for Android to make it possible to update a page's existing `meta[name=viewport]` element's `content` attribute and have the viewport be updated accordingly.

So when version 43 ships (at some point in 2015), code like [this][gh] will work in more places than it did in 2014:

```js
if(screen.width < 760) {
    viewport = document.querySelector("meta[name=viewport]");
    viewport.setAttribute('content', 'width=768');
}
if(screen.width > 760) {
    viewport = document.querySelector("meta[name=viewport]");
    viewport.setAttribute('content', 'width=1024');
}
```

I'll just go ahead and accept the 2015 netaward now, thanks for the votes everyone, wowowow.


[netaward]: https://thenetawards.com/mike-is-so-cool-and-popular/and-wow-what-a-blogger.html
[2014]: https://miketaylr.com/posts/2014/02/dynamically-updating-meta-viewport.html
[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=976616
[gh]: https://github.com/mrkittin/nodeapp/blob/7d8c9a322db873991e0889b69e48772db69e6b8b/public/js/viewport.js#L1