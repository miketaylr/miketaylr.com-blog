---
layout: post
title:  A case for why parkrun.com needs to update their friggin site in the year 2020
date:   2019-12-17
---

In what will probably be the 3rd best blog post I write this year, I'd like to make a case for why the [parkrun.com][pr] website needs to update one of their JS dependencies (and I guess anyone else using it).

Let me set the stage: in [Bug 1388931][1388931], Rick Byers filed a bug asking Gecko to consider removing the SVGPathSeg interfaces (see ([1][spec1])([2][spec2]) if you love reading old specs and ([3][remove]) if you love seeing things get ripped out of specs), as [Chrome successfully removed them in late 2015][crbug].

Some people weren't (aren't?) happy about this, and some sites and apps were broken. e.g., [svg-edit][svgedit]. But a polyfill was created and it seems to be pretty actively maintained, and that generally seems OK (though polyfills are [rarely perfect][perfect]).

OK, now the stage is set for the rest of this amazing blog post.

Now, back to the [parkrun.com][pr] site. If you load this in Chrome you get to see a lovely SVG animation of birds and trees and learn about exciting topics like running and parks and running in parks. Really good stuff. Now if you open it in Firefox, you just get a never-ending loading animation (to be fair, it's a very well done animation and a super compelling (never-ending) loading experience).

So that's [the bug that someone reported][wb] via webcompat.com.

<img style="width: 75%; height: 75%; border: 1px solid #ccc;" src="https://miketaylr.com/posts/assets/pathseg.png" alt="screenshot of browser console errors">

Digging into the error from the above screenshot: `TypeError: d.replaceItem is not a function`, a younger, more handsome Mike Taylor from the past commented:

> There's a file called pathseg.js that adds [the `replaceItem`] method to the SVGPathSegList interface:

```js
if (!("SVGPathSegList" in window)) {
  // Spec: http://www.w3.org/TR/SVG11/single-page.html#paths-InterfaceSVGPathSegList
  window.SVGPathSegList = function(pathElement) {
    this._pathElement = pathElement;
    // ..... etc.
}

```

> And then it goes on to define the replaceItem method on its prototype.

> So, since we do have SVGPathSegList (Chrome doesn't anymore), we don't get that method defined and it throws. 💣

So to recap, Chrome removed `SVGPathSegList` and a nice polyfill was created to unbreak sites. In a way that couldn't be predicted at the time the polyfill was authored, years later it inherited a bug* which unintentionally broke (a) site(s) in Firefox because Firefox 59 didn't entirely remove `SVGPathSegList`. Instead Gecko just [removed the WebIDL methods for creating and mutating SVG path data][59]. In the end, the kind polyfill maintainer [released a patch to fix that bug][patch] in Firefox.

(*I'm not even sure that counts as a bug to be honest, but it would be great if open source maintainers could try to do a better job at predicting bizarro future scenarios when writing cross-browser code.)

So anyways, back to this never-ending animation that's been taunting me for all of 2019. Despite my best efforts, I can't get ahold of anyone who works on the site to actually fix the bug. That's why it's my 2020 resolution for someone to read this that has the power to update the [pathseg polyfill on parkrun.com][old] and for them to do that. Then I can finally watch the nicely animated scene in Firefox and feel bad for not running this week (or the week before that).


[wb]: https://github.com/webcompat/web-bugs/issues/23877
[1388931]: https://bugzilla.mozilla.org/show_bug.cgi?id=1388931
[pathseg]: https://github.com/progers/pathseg/blob/master/pathseg.js#L368
[patch]: https://github.com/progers/pathseg/commit/e629e014517d4b7047f051b61435b70565b1e1dd#diff-f3550d4df03ff978beff27f184890c57
[spec1]: https://www.w3.org/TR/SVG11/single-page.html#paths-InterfaceSVGPathSeg
[spec2]: https://www.w3.org/TR/SVG11/paths.html#InterfaceSVGPathElement
[remove]: https://github.com/w3c/svgwg/commit/25ad470b300a1274a9a45734811c9a5f809233cf#diff-f5e97d5d9a830d1273c9621f3a91ebc2
[crbug]: https://bugs.chromium.org/p/chromium/issues/detail?id=539385#c16
[perfect]: https://bugs.chromium.org/p/chromium/issues/detail?id=539385#c77
[svgedit]: https://github.com/SVG-Edit/svgedit/issues/19
[pr]: https://www.parkrun.com
[59]: https://bugzilla.mozilla.org/show_bug.cgi?id=1436438
[old]: https://www.parkrun.com/wp-content/themes/parkrun2014/library/js/plugins/pathseg.js