---
layout: post
title:  Background Positions X and Y. (And ehow.com).
date:   2013-11-13
---

```
> When it's obvious, we must _make standarts_
> - not just follow existing ones.
> Is'nt it, Captain?

> FoxFire wanna to be outsider on position-x/position-y ?
> For what?!! )))))))
> It's out of my imagination! )))

- FlameStorm
```

It's been a while since I've been a fulltime frontend-developer*, so I totally forgot about the non-standard `background-position-x` and `background-position-y` properties.

(*Note to recruiters, don't worry I'm still super up-to-speed on XSLT and Java and have 15 years of AngularJS and EmberJS experience, so keep those awesome LinkedIn invitations coming! Got a 2 month on-site contract in South Jersey? Perfect!)

[Bug 733791][bug] jostled my memory though. As I noted in my [Thunder Plains talk][talk], ehow.com has been kind enough to update their -webkit-prefixed background gradients and layout styles, but the layout of the button text is still a little wonky.

<img src="https://miketaylr.com/posts/assets/ehow-buttons.png" alt="screenshot of some buttons on the mobile version of ehow.com">

You see, the "home food", "mom tech", "style money" and "health crafts" buttons should actually look like this:

<img src="https://miketaylr.com/posts/assets/ehow-webkit-buttons.png" alt="screenshot of some buttons on the mobile version of ehow.com, as seen in a WebKit browser">

{% highlight css %}
 .ChannelListing .mom-tech{
    background-color:#cfb793;
    background-image:url(blah.png);
    background-position-y:8px;
    background-position-x:7px;
 }

{% endhighlight %}

Of course, it's not too much work to combine those into a single `background-position` property and then it works everywhere. What's more interesting to me is the fact that Firefox is now the only browser that doesn't support this non-standard IE invention (with Opera having sacrificed Presto to pay tribute to Blink). [Bug 550426][bug2] was opened in 2010 requesting that support be added. [Comment 21][21] suggests that it's a damned if you do (possibly break `getComputedStyle`, damned if you don't situation (have wonky sprite layout in sites like ehow.com).

I haven't made up my own mind whether I'm for or against Gecko supporting these properties, but it's something I'll be keeping on my radar. But if you're making sites today I would shy away from its usage (and stick to the good ol' `background-position`) until browser support is 100%.

[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=733791
[bug2]:  https://bugzilla.mozilla.org/show_bug.cgi?id=550426
[talk]: https://miketaylr.com/pres/thunderplains/?full#37
[21]: https://bugzilla.mozilla.org/show_bug.cgi?id=550426#c21