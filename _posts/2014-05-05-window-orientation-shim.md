---
layout: post
title:  A shim for window.orientation based on window.screen.orientation
date:   2014-05-05
---

Last week [I wrote a Firefox for Android add-on][gh] that shims support for the non-standard `window.orientation` property, mapping to the values returned by the [Screen Orientation API][spec] (which are [currently prefixed in Gecko][prefix] as `screen.mozOrientation`).

In a nutshell, it's pretty simple:

``` js
const orientationMap = {
  "portrait-primary": 0,
  "portrait-secondary": 180,
  "landscape-primary": 90,
  "landscape-secondary": -90
}

window["orientation"] = orientationMap[window.screen.mozOrientation]
```

You just need to worry about writing adding the `orientation` property before any other JS runs and take care to call `orientationchange` handlers, dispatch events, etc.

I've only got two manual tests at [http://miketaylr.github.io/orientation-shim/][tests] so it's probably loaded with bugs&mdash;but a few quick tests on sites like [http://newyork.schmap.com/][schmap] seem promising.

(Sadly I couldn't find any `window.orientation` tests in the Android or WebKit projects (probably becuase I'm lousy at binging things?)).

You can [grab the .xpi][xpi] (or [build it][build], whatever) and give it a spin if you're into that. For a limited time I'm [taking bug reports][bugs] at no charge to you.

UPDATE:

Rich Tibbett writes ([1][1] & [2][2]):

"Nice post but I don't think it's that simple. Lots of tablets are [landscape-primary] yet window.orientation == 0. Any relationship between orientation angle and portrait/landscape is coincidental. That's why we need the orientation angle."

So perhaps I have more work to to.

[tests]: http://miketaylr.github.io/orientation-shim/
[spec]: http://www.w3.org/TR/screen-orientation/
[prefix]: https://developer.mozilla.org/en-US/docs/Web/API/Screen.orientation#Notes
[xpi]: https://github.com/miketaylr/orientation-shim/blob/master/bin/orientation-shim.xpi?raw=true
[build]: https://github.com/miketaylr/orientation-shim#building--installation
[gh]: https://github.com/miketaylr/orientation-shim
[schmap]: http://newyork.schmap.com/
[bugs]: https://github.com/miketaylr/orientation-shim/issues
[1]: https://twitter.com/richtibbett/status/463370882560565248
[2]: https://twitter.com/richtibbett/status/463371309838528512