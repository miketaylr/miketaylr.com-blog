---
layout: post
title: Everything is terrible (but more so inside a keypress event handler)
date: 2019-01-22
---

Happy new years.

We got really, really close to being able to ship [`window.event`][ev], but had to back that out from shipping to Firefox 65 release in [Bug 1520756][bug1] (like we [had to do in 63][63]).

Back in 2014's classic post ["Why FF says that window.event is undefined? (call function with added event listener)*"][old] I mentioned the global `event` that Gecko never supported (because it was a non-standard IE-ism). Years passed and things didn't get better&mdash;mostly because WebKit and thus Blink inherited said IE-ism&mdash;so we added some [hot legacy garbage][garb] to the DOM Standard and got it implemented in Firefox.

Things looked really promising on mobile until our desktop beta population started reporting bugs against banks and government sites (especially in India). Like, "can't type in this input" kinds of bugs.

[Bug 1479964][kc] has the details, but the basic gist is as follows (inside a `keypress` event handler):

`var keyCode = window.event ? event.keyCode : event.which;`

The presence of `window.event` presumes you can get a non-zero `keyCode` from the event object, and then you can take that and do something amazing on your bank and government login form.

Let's look at the Blink source to see how they handle `keyCode` inside a keypress handler:

```
  // Firefox: 0 for keydown/keyup events, character code for keypress
  // We match Firefox
  if (type() == EventTypeNames::keypress)
    char_code_ = key.text[0];

  if (type() == EventTypeNames::keydown || type() == EventTypeNames::keyup)
    key_code_ = key.windows_key_code;
  else
    key_code_ = char_code_;

```

For `keypress`, they set `charCode` to something meaningful, but also set `keyCode` to `charCode`. So that's what we're trying to do now. Like most things that break websites, there isn't like, a spec for any of this stuff yet (but if someone wants to take care of that, [be my guest][spec], probably good to read over [JavaScript Madness: Keyboard Events][mad] as you tackle that).

But like I wrote in the title, everything is terrible. We discovered late in the 65 beta cycle that [Atlassian Confluence was busted][conf]. Like, "can't type the period in a comment" busted.

The reason it's broken there is due to some code that tries to handle delete and backspace key presses in selections, and sometimes prevent that from happening (I guess?).

```
// Webkit will fire keyDown and keyUp for backspace and delete (even if one is suppressed).
ed.onKeyDown.add(deleteAndBackspaceKeyHandling);
ed.onKeyUp.add(deleteAndBackspaceKeyHandling);
tinymce.isGecko && ed.onKeyPress.add(deleteAndBackspaceKeyHandling);
```

So you've got 3 event listeners trying to do the same thing, but only Firefox sees the `keypress` one. 

```
function deleteAndBackspaceKeyHandling(ed, e) {
    var keyCode = e.keyCode;
    ...

    if (!ed.selection.isCollapsed()) {
        addCursorTargetParagraphsToContent(ed.getBody());
    } else if (keyCode === KEY_CODE.BACKSPACE && someOtherJunk()) {
        tinymce.dom.Event.prevent(e);
    } else if (keyCode === KEY_CODE.DELETE && someOtherJunk()) {
        tinymce.dom.Event.prevent(e);
    }
    return true;
}
```

`KEY_CODE.BACKSPACE` is 46. 46 is the code for a PERIOD in a keypress event, but the code for BACKSPACE in keydown/keyup events. So in Firefox, and Firefox only, it thinks its seeing a backspace and prevents the default action (which is to let the user type the period).

Confluence is especially tricky because they offer a self-hosted version in addition to their cloud-hosted thing. Breaking intranet webapps is a non-starter, so we've prevented this set of changes from shipping to release (again). And we'll try to come up with some creative hack so we can fix a ton of mobile sites but not break outdated intranet Confluence instances.

Anyways, like I said, happy new years or whatever.


[ev]: https://bugzilla.mozilla.org/show_bug.cgi?id=218415
[kc]: https://bugzilla.mozilla.org/show_bug.cgi?id=1479964
[bug1]: https://bugzilla.mozilla.org/show_bug.cgi?id=1520756
[old]: https://miketaylr.com/posts/2014/04/why-ff-says-that-window-event-is-undefined.html
[garb]: https://dom.spec.whatwg.org/#interface-window-extensions
[spec]: https://github.com/w3c/uievents/issues/213
[conf]: https://bugzilla.mozilla.org/show_bug.cgi?id=1514940
[63]: https://bugzilla.mozilla.org/show_bug.cgi?id=1493869
[mad]: https://unixpapa.com/js/key.html
