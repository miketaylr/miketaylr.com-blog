---
layout: post
title:  A blog post about window.controllers being removed from then re-added to Firefox.
date:   2014-07-08
---

[Bug 794943][bug1] documents removing access to the XUL `window.controllers` property to the web (along with some other junk). But as you can see in [1010577][bug2], removing that breaks the web. Sadly `window.controllers` is used by [many][many], [many][many2] sites as a proxy sniff for Gecko-based browsers.

Here's an example from the Ace editor's [useragent.js][uajs]:

``` js
// Is this Firefox or related?
exports.isGecko = exports.isMozilla = (window.Controllers || window.controllers) && window.navigator.product === "Gecko";
```

It turns out you can't just break websites and rich text editors for all your users&mdash;years of HCI research suggests that just makes them sad. But what you can do is add back a fake `controllers` object, such that `window.controllers === "[object XULControllers]"`, but only for release builds.

Now when developers are testing their sites in Nightly or Aurora and they notice their sniffy scripts are breaking, a console warning will let them know what's up.

And since you're all testing your sites and apps in Aurora [I don't need to link to the download page][aurora].

[bug1]: https://bugzilla.mozilla.org/show_bug.cgi?id=794943
[bug2]: https://bugzilla.mozilla.org/show_bug.cgi?id=1010577
[many]: https://www.google.com/search?q=ua+detection+"window.controllers"
[many2]: https://github.com/search?q=%22window.controllers%22+Gecko&ref=cmdform&search_target=global&type=Code
[uajs]: https://github.com/ajaxorg/ace/blob/8db9dc255ab0077603dd45a1ee2a96fdff809932/lib/ace/lib/useragent.js
[aurora]: http://www.mozilla.org/en-US/firefox/channel/#aurora