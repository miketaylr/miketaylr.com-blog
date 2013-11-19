---
layout: post
title:  Something about backformation or analogy to vendor prefixed APIs.
date:   2013-11-19
---

There's a few APIs that exist on the web created by [backformation-y][bf] [analogy][anal] from vendor-prefixed APIs, but they won't actually work. In web browsers...which is weird.


**Prefixed**
`document.mozRequestFullScreen`
`document.mozFullScreenElement`

**Not Real**
`document.requestFullScreen`
`document.fullScreenElement`

**Real**
`document.requestFullscreen`
`document.fullscreenElement`

There may be code with similar capitalization bugs for the rest of the Fullscreen APIs. I wouldn't be surprised.

Here's an example from [Hulu][hulu]:

``` js
AppHelper.isFullScreen = function() {
  return document.fullScreenElement ||
         document.mozFullScreen ||
         document.webkitIsFullScreen ||
         $('video').get(0).webkitDisplayingFullscreen;
};
```

(My favorite part is uppercase Screen in `webkitIsFullScreen` vs lowercase screen in `webkitDisplayingFullscreen`.)

And here's one I randomly grabbed from [Github search][ghs]:

``` js
document.body.requestFullScreen = document.body.webkitRequestFullScreen ||
                                  document.body.mozRequestFullScreen ||
                                  document.body.requestFullScreen;
```

Can you guess what will happen when (if?) browsers remove their prefixed versions? `undefined` city. (You can actually observe this using Opera 12, which implemented the Fullscreen API without prefixes.)

I remember [complaining about this on Twitter][beef] a year ago and my friend Lon had a pretty great [solution][soln] (editorialized to add semicolons; sorry not sorry):

``` js
for (var k in window) {
  window[k.toUpperCase()] = window[k];
}
window.REQUESTFULLSCREEN();
```

Update: Don't actually copy and past that into your site. It's not entirely clear if tweet-code has license issues. Will update again when Legal unblocks me from twitter.

Update 2: Seriously don't even look at that code.

```
<lawnsea> no i didn't open source that. there's a seat license
```

[bf]: http://en.wikipedia.org/wiki/Back-formation
[anal]: http://en.wikipedia.org/wiki/Analogy#Linguistics
[hulu]: http://www.hulu.com/site-player/html5/js/app_v2.js
[beef]: https://twitter.com/miketaylr/status/243747209970073600
[soln]: https://twitter.com/lawnsea/status/243749158689853440
[ghs]: https://github.com/samdutton/simpl/blob/d5c70e248dd102a077f8fecbc0462d33e3d5b647/fullscreen/js/main.js#L4-L5