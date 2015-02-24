---
layout: post
title:  WebKitCSSMatrix and ehow.com and web compatibility stuff
date:   2015-02-24
---

Wow, <s>early January</s> late February 2015, time to blog about more exciting web compatibility bugs. 

Lately we've been receiving a number of reports, both in Bugzilla and on webcompat.com, that ehow.com isn't working in Firefox mobile browsers. With compelling articles like [How to blog for cash][blog] and [How do do your own SEO for your blog or website][seo], this is not a great situation for my fellow Firefox Mobile-using-"how-to-get-rich-by-web-logging" friends.

> And now for a brief message from our sponsor, [http://vart.institute/][vart]: go learn about Art and programming and [Mary Cassatt][cassatt]. // TODO(mike): ask jenn for money or how to do SEO.

The biggest obstacle to getting the site to behave is: `ReferenceError: WebKitCSSMatrix is not defined`. There are few minor CSS issues, but nothing that sprinkling a few unprefixed properties won't solve.

If you're not familiar with WebKitCSSMatrix, Apple has [some docs][apple]. It's pretty cool (and much nicer than using regular expressions to dice up serialized transform matrices and then doing math and string wrangling yourself). Microsoft even has an equivalent called [MSCSSMatrix][ms] (which WebKitCSSMatrix is mapped to for compat reasons). 

Once upon a time, this thing was specced as [CSSMatrix][w3] in the CSS3 2D Transforms spec, but eventually was removed (because I forget and am too lazy to search through www-style archives). It returned as a superset of CSSMatrix and [SVGMatrix][svgmatrix] and got the sweet new name of DOMMatrix&mdash;which now allows it to be mutuble or immutable, depending on your needs, i.e., do I want a billion new object instances or can I just modify this thing in place.

There's a handful of [polyshims available on GitHub][polyfill] if you need them, but the simplest is to just map WebKitCSSMatrix to [DOMMatrix][dommatrix], which is supported in Firefox since Gecko 33.


Impress your friends like so:

``` 
if (!'WebKitCSSMatrix' in window && window.DOMMatrix) {
  window.WebKitCSSMatrix = window.DOMMatrix
};
```

In theory that's cool and useful, but if you do a little bit more digging you'll find that it only gets you so far on the web. Back to ehow.com, here's the code that powers the [PageSlider class][pageslider] here: 

```
function r() {
  var e = document.defaultView.getComputedStyle(E[0], null),
  t = new WebKitCSSMatrix(e.webkitTransform);
  return t.m41
}

// some method does W.At = r()
// then some other method calls d(W.At + o, 'none')

function d(e, t) {
  var n = 'none' != t ? 'all ' + t + 's ease-out' : 'none';
  E[0].style.webkitTransition = n,
  E[0].style.webkitTransform = 'translate3d(' + e + 'px,0,0)'
}
```

Unfortunately, WebKitCSSMatrix and webkitTransform aren't really possible to [tease apart][ghsearch], if you're trying to be compatibile with the deployed web. So a straightforward WebKitCSSMatrix to DOMMatrix mapping won't get you very far.


[blog]: http://www.ehow.com/how_4577131_blog-for-cash.html
[seo]: http://www.ehow.com/how_4863603_own-seo-blog-website.html
[vart]: http://vart.institute/
[cassatt]: http://vart.institute/cassatt/index.html
[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=717722
[apple]: https://developer.apple.com/library/safari/documentation/AudioVideo/Reference/WebKitCSSMatrixClassReference/index.html
[dommatrix]: https://developer.mozilla.org/en-US/docs/Web/API/DOMMatrix
[ms]: https://msdn.microsoft.com/en-us/library/windows/apps/hh453593.aspx
[w3]: http://www.w3.org/TR/2011/WD-css3-2d-transforms-20111215/#cssmatrix-interface
[svgmatrix]: https://developer.mozilla.org/en-US/docs/Web/API/SVGMatrix
[polyfill]: https://github.com/search?q=cssmatrix&ref=searchresults&type=Repositories&utf8=%E2%9C%93
[pageslider]: https://gist.github.com/miketaylr/b8e37e44ff545c2dbec0
[ghsearch]: https://github.com/search?l=javascript&q=new+WebKitCSSMatrix%28window.getComputedStyle%28el%2C+null%29.webkitTransform%29&type=Code&utf8=%E2%9C%93