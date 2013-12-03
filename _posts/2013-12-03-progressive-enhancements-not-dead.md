---
layout: post
title:  For the love of all that's holy, please progressively enhance modal "download our app" dialogs
date:   2013-12-03
---

[Bug 944767][bug] is the classic example of how un-progressive-ly enhanced "Download our app!" modals can ruin your users <strike>lives</strike> on-site experiences.

Don't bother reading the bug, I'll break it down for you here (because I get paid per kilobyte of html served).

It all starts with a function `getDeviceType`. Now, as hard-earned experience and primetime television adversiting have taught us, there are only three classes of devices on the web: "android", "iPhone", and "unsupported"*.

*commonly used by the unwashed masses.

``` js
A.getDeviceType = function(a) {
  var ua = a ? a : navigator.userAgent;
  return ua.match(/Android\s+[\d.]+/) ? "android" : ua.match(/iPhone/) ? "iPhone" : "unsupported";
};
```

If you happen to be cool enough to surf the web with an "android" or "iPhone" device, your browser will then set up some click event handlers on some `href`-less `<a>`nchor elements:

``` js
A.bindEvents = function() {
  if (A.device !== "unsupported") {
    A.bindClickEvents();
    A.disableTouchMove();
  }
};
```

I'll spare you the details of `A.bindClickEvents`, but eventually it calls a method that manually sets `location.href` to a URL.

All of this sounds pretty great, right? (let's ignore the fact that you can add an `@href` attribute to an `<a>`nchor element and browsers will magically navigate there, with or without the power of JavaScript&mdash;because that's like, super démodé).

So what if you were using an "android" browser to visit [instructables.com][ins], but it didn't match the `/Android\s+[\d.]+/` regex? Let's say, for example, you use the [Firefox for Android][ffa] browser which has the following UA string:

`Mozilla/5.0 (Android; Mobile; rv:24.0) Gecko/24.0 Firefox/24.0`

Can you guess what happens when you try to click on either of the following?

``` html
<a id="splash-link-getapp">
  <i class="icon devices-icon"></i>
  <b>Download Free App »</b>
</a>
<a id="splash-link-continue">No thanks, I'll keep browsing</a>
```

You're hosed.

[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=944767
[ins]: http://www.instructables.com/id/Coffee-Burr-Grinder-Attachment-for-KitchenAid-Mixe/
[ffa]: http://www.mozilla.org/en-US/mobile/