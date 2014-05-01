---
layout: post
title:  A quiz about RegExp.prototype.exec return values and numeric indexes (or indicies or however you people pluralize that word).
date:   2014-05-01
---

Given the following user agent string `ua` and function expression `r`,

``` js
var ua = "Mozilla/5.0 (Android; Mobile; rv:31.0) Gecko/31.0 Firefox/31.0"

r = function (e) {
  if (e.indexOf('android') !== - 1) {
    var t = /android (\d)/gi,
    n = parseInt(t.exec(e)[1], 10);
    return !isNaN(n) && n >= 4
  }
  return !1
}
```

What's the value of `parseInt(t.exec(e)[1], 10)`?

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
.
.

Trick question. You get a `TypeError` because the [1]th index of `null` doesn't̗̦͇̮͕̾ ̝̠̥̩̞͙̣ͤ̑ͧ̏ͥe̹͉̘̯̎͒̃x̤͑̿i̬ͮ͑̇̎̉s͉̝̩̲̹͒́t̻͍̻̯̗̅ͅ.̩ͧͯ̆̑̇̒



So what do you win for taking this quiz?

If you happen to work for Comedy Central, your prize is now you know [why you have a bug][bug]!

If you don't, you can follow [this link][te] to entertain yourself with similar bloopers in the wild.

[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=1001459#c9
[te]: http://typeerror.tumblr.com/