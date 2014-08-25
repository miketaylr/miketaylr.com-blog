---
layout: post
title:  document.write()ing some Zeptos
date:   2014-08-25
---

Sometimes debugging broken websites is tricky. If you're lucky you'll get yelled at by a console full of errors, other times you just need to poke around a bit and see what sticks. [Bug 1015725][bug] is one of those instances: a bug about the sub-menus of a burger menu not expanding when touched on wired.com's mobile site.

Our resident JavaScript archeologist [Hallvord][hallvord] pointed out that the menu items were opened by a `tap()` method, which means they were probably using [Zepto.js][notzepto]. But the weird thing is `window.Zepto` is undefined in Firefox for Android yet alive and kicking in Chrome Mobile. But web compat bugs sometimes boil down to different files being sent to different user agents (like [this old medium.com bug][mediumbug])&mdash;so that was my next line of investigation.

Another thing to remember when debugging broken things is viewing a page's source can be a little misleading if you're not paying attention (and apparently I never do). Inspecting the DOM in devtools is really inspecting a parsed DOM, vs. looking at the pre-parsed document source via a `view-source:` scheme (or whatever it's called) or right-clicking and selecting View Page Source, etc.

Based on my [comment][comment], I had forgotten this distinction and thought Wired was sending one script to Chrome Mobile and not to Firefox for Android:

> Looking at the source, they're not serving us mobify.js? (see [screenshot][screenshot])

What I didn't know, but Hallvord did, was that Mobify.js uses `document.write()` to re-write the entire content of a site with a mobile-ified version of a site (or something like that). And naturally it turns out that Gecko and Blink/WebKit have different behavior with respect to keeping references to script after `document.write()` overwrites the entire document.

Here's a [test page][test] that shows us that any global references before `document.write()` replaces a document do not survive in Firefox/Presto Opera/(maybe IE?), but do survive in Chrome/Safari.

So with Zepto defined before the call to `document.write()` to moblify the site, Blink/WebKit browsers can call Zepto's `tap()` method and get some sweet expanding hamburger helper sub-menu action. But nobody else can.

But what does the [HTML Standard][spec] say? As far as I can tell, when `document.open()` is called (and `document.write()` will implicitly call `open()`), step 15 of the `document.open()` algorithm says, 

> Replace the Document's singleton objects with new instances of those objects. (This includes in particular the Window, Location, History, ApplicationCache, and Navigator, objects, the various BarProp objects, the two Storage objects, the various HTMLCollection objects, and objects defined by other specifications, like Selection...

There's also a Note that says,

> The new Window object has a new [script settings object][sso].

When you get a new script settings object, you're supposed to get a new global object. That seems to mean that any `window.Zepto`s you had hanging on your old global object should now be `undefined`. (And caitp from #whatwg found this [Chromium bug][crbug] which seems to confirm that reading.)

So the takeway lesson from this bug is: don't rely on global variables surviving `document.write()` if you expect your app to work in more than a single browser engine because all the serious VC investors are super passionate about a cross-browser mobile web. But you already knew that.

[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=1015725
[mediumbug]: https://bugzilla.mozilla.org/show_bug.cgi?id=946737
[comment]: https://bugzilla.mozilla.org/show_bug.cgi?id=1015725#c3
[screenshot]: https://bug1015725.bugzilla.mozilla.org/attachment.cgi?id=8430267
[notzepto]: http://jquery.com/download/
[hallvord]: https://twitter.com/hallvord
[test]: https://miketaylr.com/bzla/docwrite.html
[spec]: http://www.whatwg.org/specs/web-apps/current-work/#dom-document-open
[sso]: http://www.whatwg.org/specs/web-apps/current-work/#script-settings-object
[crbug]: https://code.google.com/p/chromium/issues/detail?id=149785