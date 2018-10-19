---
layout: post
title: Don't rely on the shape of (Native)Error.prototype.message
date: 2018-10-19
---

Over in [Bug 1359822][improve], the fine folks on the JS team tried to make error messages for `null` or `undefined` property keys nicer in Firefox. So rather than something super lame (and sometimes confusing) like:

```
TypeError: window.pogs is undefined
```

You'd get something more helpful like:

```
TypeError: win.pogs is undefined, can't access property "slammer" of it
```

This was supposed to land in the Firefox 64 release cycle, but we ran into a [few][beefree] [snags][flipkart].

It turns out that sites (big ones like Flipkart) and libraries depend on the exact shape of an error message, and will fail is craptastic ways if their regular expressions expectations are not met. Like, blank pages and non-stop loading loops.

Facebook's [idx][idx]'s library is [an example][fb]:

```js
const nullPattern = /^null | null$|^[^(]* null /i;
const undefinedPattern = /^undefined | undefined$|^[^(]* undefined /i;
```

[Bug 1359822][improve] adds a comma to the error message, so `undefinedPattern` doesn't match anymore.

And yeah, you can't really break the [sixth biggest site in India][rank] and expect to have users, so we backed out the error improvements patches.

But what does the spec say?, you ask, because you're like, really smart and care about specs and stuff (obviously, because you're reading this blog for and by really smart people™).

[ECMAScript® 3000, in section 19.5.6][spec] says:

> When an ECMAScript implementation detects a runtime error, it throws a new instance of one of the NativeError objects defined in [19.5.5][five]. Each of these objects has the structure described below, differing only in the name used as the constructor name instead of NativeError, in the name property of the prototype object, and in the <i><font color=red>implementation-defined</font></i> message property of the prototype object.

That's spec-talk for "these error messages can be different between browsers, and there's no guarantee they won't change, so attempting to rely on them is harmful to making things better in the future" (approximately).

[I filed a bug][bug], but getting upstream open source libraries fixed is way less hard than getting downstream sites to update. 

<img src="https://miketaylr.com/posts/assets/pogmath.jpg" width="450px" style="border:1px solid #ccc" alt="A kid doing pog math">

(Almost as hard as pog math.)

The moral of the story is: don't write regular expressions against unstandardized browser-specific messages. Or maybe just never write regular expressions ever again. Seems easier?

[gore]: https://www.youtube.com/watch?v=JDUjeR01wnU
[beefree]: https://bugzilla.mozilla.org/show_bug.cgi?id=1490772#c7
[flipkart]: https://bugzilla.mozilla.org/show_bug.cgi?id=1498257
[improve]: https://bugzilla.mozilla.org/show_bug.cgi?id=1259822
[fb]: https://github.com/facebookincubator/idx/blob/9609fe1a728caaea8f621c44e2e7190a1c5689f6/packages/idx/src/idx.js#L73-L84
[idx]: https://www.npmjs.com/package/idx
[bug]: https://github.com/facebookincubator/idx/issues/55
[spec]: https://www.ecma-international.org/ecma-262/9.0/index.html#sec-nativeerror-object-structure
[rank]: https://www.alexa.com/siteinfo/flipkart.com
[five]: https://www.ecma-international.org/ecma-262/9.0/index.html#sec-native-error-types-used-in-this-standard
