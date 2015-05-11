---
layout: post
title:  CircleKSunkus and CSS Zoom
date:   2015-05-11
---

In this, the Nth installation of the Weird World Wide Web Websites series we take a look at [circleksunkus.jp](http://www.circleksunkus.jp/), purveyor of fine Japanese soft drinks and snacks.

If you load this site on a Firefox mobile browser you might think you're up against some old -webkit- prefixed flexbox code. Or maybe one of those fun edgecase float differences between WebKit and Gecko.

<img style="border:1px solid #ccc; max-width: 100%;" src="https://miketaylr.com/posts/assets/circlek.png" alt="screenshot of circlesunkus.jp website">

But nope, it's even weirder.

```js
//viewport
$(window).bind('resize load', function(){
	$("html").css("zoom" , $(window).width()/640 );
});
```

They're using IE's olde non-standard CSS zoom property (that WebKit implemented back in the Safari 4 days) to retrigger a zoomed out layout on page load, which leads to a pretty harsh Flash of Un-Zoomed-Out Content (FOUZOC) if you're on a third world internet connection (like the ones we have here in Texas).

Anyways, if you happen to work on circlesunkus.jp, here's how to do this the right way™:

`<meta name="viewport" content="width=640">`

More info in the [webcompat bug](https://github.com/webcompat/web-bugs/issues/952).
