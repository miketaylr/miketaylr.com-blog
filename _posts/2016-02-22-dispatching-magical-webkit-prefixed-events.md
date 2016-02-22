---
layout: post
title: Dispatching legacy webkit prefixed events (but only some of the time)
date:   2016-02-22
---

Here's a twist on the classic "browser has bug, developers change to work around it, sites depend on it, browser has to implement weird workaround to fix their bug without breaking those sites, and then other browsers need to match weird workaround bug compatibility" cycle.

In this case, [Bug 1236930][bug] reported that zooming in on Strava maps only worked once. Strava (and a ton of other popular sites) uses a slick little mapping libary called [Leaflet.js][leaflet], and if our work to ship WebKit-prefixed CSS and DOM aliases (check out [Bug 1170774][aliases]) is actually going to uh, ship, we obviously can't break it.

You can read the whole bug later; here's the problem distilled to two lines of JS (see if you can find it):

```js
o.DomUtil.TRANSFORM=o.DomUtil.testProp(
   ["transform","WebkitTransform",
    "OTransform","MozTransform","msTransform"]),
o.DomUtil.TRANSITION=o.DomUtil.testProp(
   ["webkitTransition","transition",
    "OTransition","MozTransition","msTransition"])
```

You see how they're trying to do the nice thing and test for the right transition and transform properties to use?

Once they know that, they construct the `TRANSITION_END` string to attach listeners with elsewhere in the code.

```js
o.DomUtil.TRANSITION_END="webkitTransition"===o.DomUtil.TRANSITION||"OTransition"===o.DomUtil.TRANSITION?o.DomUtil.TRANSITION+"End":"transitionend",
```

Did you notice how `o.DomUtil.TRANSITION` is actually testing for `webkitTransition` *before* the prefixless `transition`? (I actually missed that my first time staring at this code, classic rookie move).

Once upon a time, Leaflet.js did the logical thing and tested for prefixless `transition` first, but in this [sweet bugfix commit][commit], that changed.

You can click through to get references to the bug it fixed, but here's a comment in the patch that gives you a gist of why they did this:

```
// webkitTransition comes first because some browser versions that drop vendor prefix don't do
// the same for the transitionend event, in particular the Android 4.1 stock browser
```

So at some point in time\*, some stock Android browser versions supported prefixless CSS transitions, but forgot to unprefix the `transitionend` event. And websites broke, and libraries updated to workaround them.

So we added support to sometimes send `webkit` prefixed `transitionend` events (and `animationend`, `animationiteration` and `animationstart`) to Gecko in [bug 1236979][consider], matching WebKit, Blink, and Edge's behavior.

If you want more details on *when* to send those events, check out the bug. Or for extra credit, read the [DOM spec][dom spec]. We updated that too.

(\* Wikipedia says Jelly Bean was released in June 2012, which was when Gotye's 'Somebody That I Used to Know' Feat. Kimbra was the #1 song so I guess we all sort of deserve this mess, honestly.)


[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=1236930
[aliases]: https://bugzilla.mozilla.org/show_bug.cgi?id=1170774
[leaflet]: http://leafletjs.com/
[commit]: https://github.com/Leaflet/Leaflet/commit/64ca0af124b7145e8c2a746e2fb087b2298a4b72
[consider]: https://bugzilla.mozilla.org/show_bug.cgi?id=1236979
[dom spec]: https://dom.spec.whatwg.org/#concept-event-listener-invoke