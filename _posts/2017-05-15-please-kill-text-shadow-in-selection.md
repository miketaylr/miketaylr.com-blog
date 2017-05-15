---
layout: post
title: text-shadow in ::selection, still not great
date: 2017-05-15
---

7 years ago I tweeted [my only good tweet][tweet]:

> please kill the text-shadow in ::selection. obsessive compulsive text highlighters like myself go blind

<img src="https://miketaylr.com/post/cc16d5c3.png" style="border: 1px solid #ccc;" alt="screenshot of text selection ugliness">

(apologies for hideous screenshot, 2010 was a weird time for web design,  I guess)

Some internet hipsters agreed, so they put a default `text-shadow: none` rule for `::selection` [in html5 boilerplate's main.css][main].

Anyways, we recently got a [bug][bug] about nearly same exact issue: if you have a white background and set a white text-shadow on the copy (wat), things can get weird when someone makes a selection:

```css
#wrapper {
  background-color: #fff;
}
.post-meta {
  text-shadow: 2px 0px 1px #fff;
}
```

So don't do that?

Anyways. The most important takeaway (for me) is that the devs over at thegunmag.com don't follow me on twitter, which is super rude when you think about it.

[main]: https://github.com/h5bp/html5-boilerplate/blob/master/src/css/main.css#L17-L28
[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=1364518
[tweet]: https://twitter.com/miketaylr/status/12228805301