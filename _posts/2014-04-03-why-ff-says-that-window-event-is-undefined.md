---
layout: post
title:  Why FF says that window.event is undefined? (call function with added event listener)*
date:   2014-04-03
---



So I quit my blog a month ago but nobody even noticed or complained on social media so I'm back in the blogging game (out of spite).

If you know anything about me, you know I enjoy a good bargain on dad wardrobe staples like khakis and beige cargo pants and, uh, carry on luggage&mdash;naturally entailing that I'm on the Kohl's mobile website like all the time.

So of course I was intrigued when [bug 905860][kohls] was brought to my attention.

On the Kohl's website there's a little "scratcher" thingy that you can rub with your finger or mouse to reveal some exciting promotion like 5% off a George Foreman grill, or something else that will probably be re-gifted. Problem is, it doesn't work in mobile Firefox browsers. But it does work in WebKit mobile browsers. (TODO(mike): insert saddest emoji possible)

Let's open our devtools console and see if anything exploded.

`ReferenceError: event is not defined`

In the browser developement industry, this is what we call a "Blue Paw Print". (That means a clue, for the under 5 crowd.)

A quick view-source reveals they're using a jQuery plugin called [wScratchPad][wsp]. BAM. [Bug city][bug].

```
bindMobile: function($el)
{
  $el.bind('touchstart touchmove touchend touchcancel', function ()
  {
    var touches = event.changedTouches, first = touches[0]… 
```

Two things are really obvious: 1) not passing in an explicit event object argument to the `$.fn.bind` callback; 2) they're using jQuery.

IE and WebKit browsers have something called a global event object, so even if you don't pass in `event` to the callback, they'll be OK. However, if the author did, the script would suddenly break everywhere, for a different reason.

Why? Because this is a jQuery plugin using `$.fn.bind`, the `event` object that gets passed in isn't a TouchEvent object, but a magical [jQuery event object][jqe]. You will only hurt its feelings if you ask it to give you the `changedTouches` property. To get to that, you need to access the `originalEvent` property to get to the, uh, original event.

```
bindMobile: function($el)
{
  $el.bind('touchstart touchmove touchend touchcancel', function (event)
  {
    var touches = event.originalEvent.changedTouches, first = touches[0]… 
```

Now if the authors of the plugin and the developers that work on Kohl's could just update their respective code bases I could get back to buying some sweet dad jeans.

*Shout out to my co-author [anony_root][ff] for coming up with the title.

[ff]: http://stackoverflow.com/questions/9813445/why-ff-says-that-window-event-is-undefined-call-function-with-added-event-list
[kohls]: https://bugzilla.mozilla.org/show_bug.cgi?id=905860
[wsp]: https://github.com/websanova/wScratchPad
[bug]: https://github.com/websanova/wScratchPad/blob/5555155815f02739ae91207c88df981107534e67/wScratchPad.js#L164-L166
[jqe]: https://api.jquery.com/category/events/event-object/