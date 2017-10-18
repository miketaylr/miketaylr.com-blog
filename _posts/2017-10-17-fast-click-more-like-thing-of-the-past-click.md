---
layout: post
title: FastClick.js (more like Thing-of-the-Past-Click.js)
date: 2017-10-17
---

`<select>` elements have always been kind of awkard, IMO.

[web-bug 12553][bug] describes an issue where a pretty famous 3rd party library, [FastClick.js][fc] turns `<select>`-level awkward to middle-school-dance-party-level awkward.

(In that the `<select>` doesn't function at all if you're on Android, unless you're using Chrome Mobile. Just like middle school&mdash;trust me this analogy makes total sense, 7th grade was really hard for me and I'm still working through it, OK.)

The issue here is a mixture of web standards interoperability failure (more on that in a second) and inability to predict the future by FastClick (more on that now).

So if you don't know much about FastClick, or why it was so popular, put on 2012's "Now That's What I Call Music! Volume 44" to set the mood and read their GitHub page.

<img src="https://miketaylr.com/posts/assets/music.png" style="border: 1px solid #ccc;" alt="Cover of Now Thats What I Call Music Volume 44 album">

Back in 2012, when tapping on things in mobile browsers was slow (because browsers had a 300ms delay between a `touchend` event and a `click` event), it was cool to use FastClick to make tapping on things fast. Jake Archibald has a [good article on this][and] and how to get rid of it these days (tl;dr make your site's viewport mobile friendly).

(Note: the article is a bit dated given that Firefox didn't support `touch-action: manipulation` when it was authored, but it [does now][mdn], so consider that another option.)

OK, so. Anyways. The way this library works is to dispatch its own synthetic `click` event after a `touchend` event, without the 300ms delay. This is all great and fine for links. Things get trickier for inputs like `<select>`, because there's never really been a web standards way to programatically open them via JS.

[Per DOM level 3(000)][dom], untrusted events (think `event.isTrusted == false`...except for `click`) shouldn't trigger the default action. And the default action for `<select>` is to open the widget thingy and display the 400 countries you don't live in.

OK, back to FastClick in the year 2012. Things either broke on `<select>` elements, or never worked properly in Chrome Mobile, but developers (being developers) found a workaround with `mousedown` events for Chrome Mobile (in a stackoverflow thread, naturally), [put it into FastClick][pr]. This unintentionally broke it for other browsers, and later on some fixes were introduced to unbreak that for Firefox and Blackberry. [But... that's only some of the select-related bugs][bugs].

Fast foward a few years and Chrome [fixed their untrusted events default action bug][fix], and as a result [broke `<select>`s on pages using FastClick][bustage]. Fortunately for interop, they decided to not back out the fix ([except for Android WebView!][wv]).

Anyways, this blog post is getting too long.

In conclusion, if you run into bugs with FastClick, maybe consider deleting it from your app. [It's basically unmaintained and they don't want you to use it either][readme]. And besides, we have [`touch-action: manipulation`][tam] anyways.



[bug]: https://webcompat.com/issues/12553
[fc]: https://labs.ft.com/fastclick/
[update]: https://github.com/ftlabs/fastclick/commit/4e409926198147f24a49c293923d2a2a047c3774
[and]: https://developers.google.com/web/updates/2013/12/300ms-tap-delay-gone-away
[mdn]: https://developer.mozilla.org/en-US/docs/Web/CSS/touch-action#Browser_compatibility
[dom]: https://www.w3.org/TR/DOM-Level-3-Events/#trusted-events
[fix]: https://bugs.chromium.org/p/chromium/issues/detail?id=520519
[bustage]: https://bugs.chromium.org/p/chromium/issues/detail?id=642698
[wv]: https://cs.chromium.org/chromium/src/third_party/WebKit/Source/core/dom/events/EventDispatcher.cpp?q=wideViewportQuirkEnabled&sq=package:chromium&dr=C&l=303
[pr]: https://github.com/ftlabs/fastclick/pull/163/files
[bugs]: https://github.com/ftlabs/fastclick/search?q=select&type=Issues&utf8=%E2%9C%93
[readme]: https://github.com/ftlabs/fastclick/commit/d4107a4e8aa1698bb5f234b4e685d7f717c4e818
[tam]: https://developer.mozilla.org/en-US/docs/Web/CSS/touch-action#manipulation