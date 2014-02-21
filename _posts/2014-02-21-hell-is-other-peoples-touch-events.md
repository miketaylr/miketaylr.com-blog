---
layout: post
title:  Hell is other people's touch events.
date:   2014-02-21
---

Vimeo has some code in its [fancy new HTML5 player][player] that's [problematic for Firefox for Android on tablets][bug]. In a nutshell, if you click the play button, it's as if it were clicked twice really quickly.

I've got the bug reduced down the `attachClickHandler` method where, for whatever reason, `click` _and_ `touchend` events are firing, causing the handler to be called twice&mdash;hence pressing play results in a pause.

(Apparently a play button that results in a video pause is not a great user experience... but what if we're unintentionally on the verge of a video player UX revolution?!)

Anyhow, here's the relevant (truncated) code:

``` js
attachClickHandler: function(...) {
  ...
  var m = function(c) {
    if ("touchend" === c.type) {
        if (c.changedTouches) {
            ... && f.call(this, c);
        }
        ... k.call(this, c);
        return !1;
    }
    return f.call(this, c);
  };
  if ("function" === typeof e)
    Gator(c).on(["click",
                 "touchend"], m);
  else
    Gator(c).on(["click",
                 "touchend"], e, m);
}
```

So if you know anything about working with touch events (which I barely do), you know that you can prevent the default action of the touch event to cancel any of the following mouse events (synthetic clicks and mouseovers and whatnot).

You can see Vimeo doing this by checking the `event.type`. If it's a `touchend` event, return false (or `!1` if you're super cool and use uglify or whatever). So in theory this prevents click from happening.

Until recently, the only way to do this cross-browser was to cancel the `touchstart` or `touchmove` events&mdash;not `touchend`. My angry German friend Patrick explained there [was/is some old Android default browser bug related to this][pat].

(If anyone has more info on that specific bug, shoot me an email to mike@dotbiz.info, or feel free to comment in the [bug][bug]).

You can see in this [simple test][test]&mdash;if you open on your phone, tablet or one of them internet-of-things internet touch things with a recent browser&mdash;that calling `preventDefault` (or returning `false`) on either `touchstart` or `touchend` will cancel the `click` event. This is apparently a [very recent change][change] in behavior for Firefox for Android (before only `touchstart` would work).

So, anyways. I learned all that stuff but I still don't know why Vimeo's play button is busted&mdash;it's still got the broken behavior in Nightly. There might be some 3rd-party code dispatching events or something, I dunno. TBD.

[player]: http://vimeo.com/blog/post:606
[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=966919
[pat]: https://twitter.com/patrick_h_lauke/status/436629251207725056
[test]: https://miketaylr.com/bzla/touchend.html
[change]: https://bugzilla.mozilla.org/show_bug.cgi?id=966919#c21