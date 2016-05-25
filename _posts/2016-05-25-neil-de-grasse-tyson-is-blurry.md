---
layout: post
title: Neil deGrasse Tyson is blurry
date: 2016-05-25
---

In [Bug 1275069][bug] (which I stumbled upon thanks to one of my "Neil deGrasee Tyson" bugmail filters), we run into an interesting bug that's the result of same origin policy restrictions and vendor prefixes.

For those unfamilar with how Buzzfeed works, frequently you get a list of blurry images and click on them for a dramatic reveal. Riveting stuff.

To set up the blur, if you're using Firefox, they [serve you an SVG `<filter>`][svg], because before Firefox 35, CSS filters weren't yet supported. Other browsers get a vendor prefixed CSS `filter` (and whatever the `progid:DXImageTransform` junk for IE is called).

But, there's a problem for Firefox users. There's no blurry image&mdash;just a blank space. A literal Neil deGrasse Tyson vacuum.

<img style="width: 75%; height: 75%; border: 1px solid #ccc;" src="https://miketaylr.com/posts/assets/busted.png">

Here's what that SVG `<filter>` looks like in devtools:

<img style="width: 75%; height: 75%; border: 1px solid #ccc;" src="https://miketaylr.com/posts/assets/sop.png">

The problem is that `img.buzzfeed.com` is a [different origin][sop] from `www.buzzfeed.com` and that server isn't sending any CORS headers, so Firefox doesn't actually render the filter (...or the image at all, that seems weird to me, see [1105145][1105145]).

OK, whatever. I still want to click on that to see some sensuous lounging the user is probably thinking.

And the plan is that once you click the (missing) blurry image, they remove the inline style with the filter `url()` function, but, oops now the following class applies (they remove the entire class in other browsers):

```css
.graphic_image {
...
  -webkit-filter: blur(30px);
  -moz-filter: blur(30px);
  -o-filter: blur(30px);
  -ms-filter: blur(30px);
...
}
```

So just when you thought you were gonna get the big Neil deGrasse Tyson reveal, you're stuck with the following:

<img style="width: 75%; height: 75%; border: 1px solid #ccc;" src="https://miketaylr.com/posts/assets/blurry.png">

Until we [added support for `-webkit-filter` as an alias of (unprefixed) `filter`][filter] (`-moz-filter` was never like, a thing), that rule was ignored.

Anyways, in your own web journalism going forward, you can just use unprefixed CSS blur filters and skip the [super fragile UA sniffing code paths][sniff].

And if you'd like to stick with SVG filters, make sure you're serving those from the same origin as your content, or at the very least go copy pasta some CORS headers from stack overflow for your asset servers.

[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=1275069
[bf]: https://www.buzzfeed.com/katienotopoulos/play-23-rounds-of-the-deadliest-game-ever
[sop]: https://en.wikipedia.org/wiki/Same-origin_policy
[1105145]: https://bugzilla.mozilla.org/show_bug.cgi?id=1105145
[filter]: https://bugzilla.mozilla.org/show_bug.cgi?id=1236506
[sniff]: https://gist.github.com/miketaylr/f4161de8c621a65b2692f722c02d8ae5
[svg]: https://developer.mozilla.org/en-US/docs/Applying_SVG_effects_to_HTML_content