---
layout: post
title:  document.body.scrollTop vs document.documentElement.scrollTop
date:   2014-11-21
---

[Here's a track][bug] from Web Compatibility's Greatest Hits Album (Volume I) that just doesn't want to go away&mdash;with the latest club remix titled "scrolling to sections from the menu in the mobile Google News site doesn't work due to setting `scrollTop` position on `document.body` in Firefox for Android".

Here's some background for those with less refined musical tastes.

(Why yes I can do this bad metaphor stuff all day long, why do you ask?)

If you want to get or set the vertical scroll position of a document, you can use `element.scrollTop`. According to the [CSSOM View Module spec][spec], if you're in standards mode you need to operate on the document's root element (the `<html>` element&mdash;or `document.documentElement` in DOM land). In quirks mode you would use the `<body>` element, via `document.body`.

This works in IE and Firefox and the late Presto Opera.

In Blink and WebKit browsers, it's the exact opposite. Both have attempted to implement the standard ([safari][safaribug], [chrome][chromebug]), but both have had to back out their patches due to sites breaking (some Google properties and webkit.org among them, as luck would have it).

The bug that was filed against WebKit for [Facebook breaking][fbbug] as a result of changing to the standard is especially interesting because it shows the tension between following standards (and other browsers) and breaking sites for their own users.

It's also a good example of how user-agent-string-based development can sometimes make it hard, if not impossible, to remove some of the crappier stuff from the web platform.

Here's some excerpts, but the whole bug is a good read.

[Comment 15](https://bugs.webkit.org/show_bug.cgi?id=122882#c15):

> It really doesn't matter how faithfully you implemented the spec.  If it causes a major backward compatibility with the Web, we can't have it.

[Comment 31](https://bugs.webkit.org/show_bug.cgi?id=122882#c31):

> Yes, the regression doesn't reproduce if we fake the UA string as I mentioned in the comment #31.

Maybe sites will update one day and let other browsers do the right thing™. (Not that I'm holding my breath over here.)

Until then I guess we get to have fun writing stuff like this (found on apple.com a few weeks back):

```
(document.documentElement ||
 document.body.parentNode ||
 document.body).scrollTop;
```



[spec]: http://dev.w3.org/csswg/cssom-view/#dom-element-scrolltop
[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=1083932#c5
[chromebug]: https://code.google.com/p/chromium/issues/detail?id=157855
[safaribug]: https://bugs.webkit.org/show_bug.cgi?id=106133
[fbbug]: https://bugs.webkit.org/show_bug.cgi?id=122882