---
layout: post
title:  2 ELEMENT 2 FURIOUS
date:   2015-11-12
---


Both [Bug 1205244][bugz] and [webcompat bug #1886][wcbug] reported an issue where Firefox users can't select menu items from a flyout-style menu in the Steam store website, but it's working in other browsers.

Web pages, amirite? (yes, mike. u r rite.)

The offending code is here, so put on your retro bug hunting hats and see if it jumps out to you:

```js
$J(document).on('mouseleave.Flyout', '.flyout_tab', function(e) {
  ...
  if ( !$Content.length ||
       $Content.data('flyout-event-running') ||
       bResponsiveSlidedownMenu ||
       $Content.is( e.toElement ) ||
       $J.contains( $Content[0], e.toElement ) )
      return;
```
.
.
.
.
.
.
.
.

In the likely case that you weren't developing sites during the DHMTL glory days (I sure wasn't), the bug is: `e.toElement`.

`event.toElement` is an [IE4-ism][2elm] that WebKit added or possibly inherited from KHTML for compatibility with IE. (I'm willing to eat my bug hunting hat if it wasn't added to fix a bunch of broken DHTML menus back in the day.)

The standard `MouseEvent` property to use when you're trying to figure out what element you're on after a `mouseout` or `mouseleave` event is `event.relatedTarget`. This is the one that is supported basically everywhere (in IE since version 9), and like, exists in a spec.

(And if you're into C++ or whatever, you can see that WebKit's `event.toElement` is [just an alias for `relatedTarget`][wkimp].)

Prototype.js&mdash;which the Valve site uses in various places&mdash;knew this property was goofy so it extends the `event` object with a polyfilled `relatedTarget`, but naturally only for IE.

```js
if (window.attachEvent) {
  function _relatedTarget(event) {
    var element;
    switch (event.type) {
      case 'mouseover':
      case 'mouseenter':
        element = event.fromElement;
        break;
      case 'mouseout':
      case 'mouseleave':
        element = event.toElement;
        break;
      default:
        return null;
    }
    return Element.extend(element);
  }
  ...
  Object.extend(event, {
    ...
    relatedTarget: _relatedTarget(event),
    ...
  });
```

You might be thinking, uh oh, Edge dropped support for `attachEvent`, this should be broken there too! But they've apparently kept that for compat reasons ([simple test here][test]). So yay I guess.

Anyways. The good folks at Valve [have fixed the menu internally][fix] and it should be possible to buy {insert hilarious video game joke so people think I'm cool here} from a dropdown menu item using Firefox (or any other future browser that doesn't implement this quirk). If you happen to have commit access to any code using `toElement` feel free to rip it out and replace it with `relatedTarget`.


[wcbug]: https://github.com/webcompat/web-bugs/issues/1886#issuecomment-153962858
[proto]: https://ajax.googleapis.com/ajax/libs/prototype/1.7.3.0/prototype.js
[2elm]: https://msdn.microsoft.com/en-us/library/ms534684(v=vs.85).aspx
[bugz]: https://bugzilla.mozilla.org/show_bug.cgi?id=1205244
[wkimp]: https://github.com/WebKit/webkit/blob/71fe97d9c1793ae7e90ee3c8ff94a8616bd522ad/Source/WebCore/dom/MouseEvent.cpp#L206-L215
[test]: http://jsbin.com/zomaguxadu/1/edit?html,js,console,output
[fix]: https://bugzilla.mozilla.org/show_bug.cgi?id=1205244#c19