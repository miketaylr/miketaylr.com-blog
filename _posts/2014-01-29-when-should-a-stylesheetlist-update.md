---
layout: post
title:  When should a StyleSheetList collection update?
date:   2014-01-29
---

Depends on who you ask, I guess.

If you add a dynamic stylesheet to a document, Firefox, Internet Explorer, and Presto Opera (RIP) will immediately add a `CSSStyleSheet` object to its `StyleSheetList` collection (that's what you get back from `document.styleSheets)`.

Chrome and Safari (and presumably other WebKittish browsers) will wait at least until the stylesheet is loaded (approximately when the `load` event of the `link` element fires).

Here's a [test page][testpage] that demonstrates the behavior. 

(Uh, apologies to Steve Souders for hotlinking his slow CSS file&mdash;but I said hi to him once in a bar at the 2nd JSConf so I think we're cool).

The [CSSOM spec][spec] isn't crystal clear on which behavior is correct, so they might both be considered valid.

As always, though, relying on the behavior of a single engine is a recipe for broken web pages. If you try to access the `cssRules` object of a stylesheet in the `StyleSheetList` collection before it's loaded in Firefox, for example, it will explode (and you'll be left wondering what "InvalidAccessError: A parameter or an operation is not supported by the underlying object" actually means).

See [this library][chikuwa] for an example where such a thing happens, causing the $.load callbacks to fire too early in non-WebKit browsers.

Perhaps the real lesson here is that "Scampi Bug Yeti" is an actual meaning<strike>ful</strike>less anagram for "spec ambiguity".


[testpage]: http://dhtml5.com/tmp/stylesheets-length.html
[spec]: http://dev.w3.org/csswg/cssom/#css-style-sheet-collections
[chikuwa]: https://github.com/ameba-proteus/chikuwa.js/blob/master/chikuwa.js#L1067-L1069