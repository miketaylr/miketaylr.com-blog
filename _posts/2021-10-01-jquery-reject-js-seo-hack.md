---
layout: post
title:  How to delete your jQuery Reject Plugin in 1 easy step.
date:   2021-10-01
---

In my [last post on testing Chrome version 100][blog], I encouraged everyone to flip on that flag and report bugs. It's with a heavy heart that I announce that [Ian Kilpatrick][ian] did so, and found a [bug][bug].

(⌣_⌣”)

The predictable bug is that [parks.smcgov.org][parks] will tell you your browser is out of date, and recommend that you upgrade it via a modal straight out of the year 2009.

<img alt="screenshot of a modal telling you to upgrade your browser, with a farmville image because that was popular in 2009?" src="https://miketaylr.com/posts/assets/park.png" style="border:1px solid #ccc; width: 100%">

(Full Disclosure: I added the FarmVille bit so you can get back into a 2009 headspace, don't sue me Zynga).

The bug is as follows:

```
r.versionNumber = parseFloat(r.version, 10) || 0;
var minorStart = 1;

if (r.versionNumber < 100 && r.versionNumber > 9) {
  minorStart = 2;
}

r.versionX = r.version !== x ? r.version.substr(0, minorStart) : x;
r.className = r.name + r.versionX;
```

Back when this was written, a version 100 was unfathomable (or more likely, the original authors were looking forward to the chaos of a world already dealing with the early effects of climate change, and now we have to deal with this?, a mere 11 years later) so the `minorStart` offset approach was perhaps reasonable.

There's a few possible fixes here, as I see it:

I. Kick the can down the road a bit more:

 ```
if (r.versionNumber < 1000 && r.versionNumber > 99) {
  minorStart = 3;
}
 ```

 I don't really plan on being alive when Chrome 999 comes out, so.

II. Kick the can down the road like, way further:

 ```
r.versionX = Math.trunc(parseFloat(r.version)) || x;
 ````

 [According to jakobkummerow][jakob], this should work until browsers hit version 9007199254740991 (aka `Number.MAX_SAFE_INTEGER` in JS).

III. (Recommended) Just remove this script entirely from your site. It's outlived its purpose.

Also, if you happen to work on any of the following [1936 sites][sites] using this script, you know what to do (pick option Roman numeral 3, just to be super clear).

[blog]: https://miketaylr.com/posts/2021/09/chrome-version-100-testing.html
[ian]: https://twitter.com/bfgeek
[bug]: https://github.com/webcompat/web-bugs/issues/88391
[parks]: https://parks.smcgov.org/
[jakob]: https://github.com/webcompat/web-bugs/issues/88391#issuecomment-930668678
[sites]: https://docs.google.com/spreadsheets/d/1uk2EiDrsuqBGFTjT5QGhliAuenzDgfZD9EXZifzzEz8/edit#gid=0