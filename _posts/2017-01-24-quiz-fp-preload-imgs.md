---
layout: post
title: A quiz about preloading images and for loops
date: 2017-01-24
---

It's been about a year since the [last quiz][quiz]. Please direct yourself to the nearest whiteboard.

Given the following block of JS from [Bug 1328542][bug] (don't look yet, cheaters. Ugh), how many new `Image`s are preloaded?

```
function FP_preloadImgs() {//v1.0
 var d=document,a=arguments;
 if(!d.FP_imgs) d.FP_imgs=new Array();
 for(var i=0; i<a.length; i  ) {
   d.FP_imgs[i]=new Image;
   d.FP_imgs[i].src=a[i];
 }
}
```
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.

If you guessed "as many as it takes until the browser hits a [OOM][oom] (out of money) or crashes", congratulations. To bad you just spent all your browser money, you should probably buy that `FP_preloadImgs` v2.0 upgrade license.

The good news is that the DIY patch is much cheaper: just be sure to increment your, uh,   expression thingies (`i` in this case) in for loops when pre-loading images (and generally everywhere else).

[quiz]: https://miketaylr.com/posts/2016/02/block-scoped-funk.html
[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=1328542
[oom]: https://en.wikipedia.org/wiki/Out_of_memory