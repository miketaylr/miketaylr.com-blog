---
layout: post
title:  The location of a synthetic dispatched mouse event
date:   2014-09-11
---

(Everybody reading this blog is smarter than me, so even though this is probably common knowledge I'm going to write it down so I can look back next time I'm dealing with fishy touch events.)

We had an interesting issue get reported on webcompat.com, ["Issue 171: time.com - Clicking on 'Tap' doesn't toggle menu"][issue] that was only affecting Firefox Mobile. Essentially, tapping on a little "rail-handle" to slide in some side content would result in the content sliding in then immediately sliding out.

Lucky for me, the site isn't that complicated&mdash;it's your run-of-the-mill Backbone.js app with obligatory can't-read-anything-until-you-dismiss-the-in-your-face-underwear advertising.

<img src="http://miketaylr.com/posts/assets/undies.png" alt="screenshot of time.com">

The listeners for the "rail handle" are set up here:

``` js
this.$notHeader.find(".rail-handle").on("touchend click", function(_this) {
  return function() {
    _this.toggle_left_rail()
  }
...
```

`toggle_left_rail()` sets some classes on some parent elements which transitions the content out. I think. It doesn't actually matter how the handle and articles move out&mdash;just that they do.

I eventually got the issue reduced down to [https://miketaylr.com/bzla/wc-171-2.html][tc]. If you open that up on an Android device with Firefox Mobile and Chrome you can see the difference in behavior.

The actual bug here is Firefox Mobile dispatches the click event on the original target while Chrome (and iOS and Firefox OS and probably whatever wearable-with-a-browser is currently strapped to your body) will dipatch the event to the original location of the `touchend` event. In my test case this is probably `<body>`, in time.com this is probably `<div class="left-rail>`. So Firefox for Android ends up calling `toggle_left_rail()` twice, while other browsers only call it once as intended.

I filed a [BugZilla issue][bz] on behalf of the original webcompat.com reporter. Feel free to follow that link if the internal details of how Firefox OS gets this right while Fennec doesn't (for now) interests you.

And naturally the [spec][spec] says,

> If the user agent intreprets a sequence of touch events as a click, then it should dispatch mousemove, mousedown, mouseup, and click events (in that order) at the location of the touchend event for the corresponding touch input. If the contents of the document have changed during processing of the touch events, then the user agent may dispatch the mouse events to a different target than the touch events.

So yeah. You all already knew that a synthetic event will be dispatched on whatever target is at the location of the originating touch event. I learned the slow way. Now let's get back to surfing underwear ad interlaced journalism.

[spec]: https://dvcs.w3.org/hg/webevents/raw-file/v1-errata/touchevents.html
[issue]: http://webcompat.com/issues/171
[tc]: https://miketaylr.com/bzla/wc-171-2.html
[bz]: https://bugzilla.mozilla.org/show_bug.cgi?id=1066157