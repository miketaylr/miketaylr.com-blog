---
layout: post
title: How not to feature detect
date: 2017-02-06
---


[Bug 1331638][bug] is a good example of why feature detection is important, but also a good reminder to test in enviroments where the feature totally doesn't exist.

A website (Twitter, in this example) tries to do the right thing and check for support before throwing notifications at you:

```
this.isSupported = function() {
  var a = "PushManager" in window,
      b = this.getNotificationPermission() !== "denied",
      c = "serviceWorker" in navigator,
      d = c && "showNotification" in ServiceWorkerRegistration.prototype;
  return d && a && b && c
}
```

Digging into `getNotificationPermission`:

```
this.getNotificationPermission = function() {
    return Notification && Notification.permission
}
```

OK, that looks pretty good. A boolean expression checking for the existence of `Notification` then returning its `permission` property value. So why the bug report with `ReferenceError: Notification is not defined`?

In Firefox, you can turn off the [Notifications API][api] via the `dom.webnotifications.enabled` pref. If you choose to do that, the `Notification` [interface global][glob] totally doesn't exist.

And what happens if you refer to something that doesn't exist? `ReferenceError`. Boom. The page totally explodes.

Well, that's dumb. My app doesn't support users who do that, you're thinking. But the situation won't be any different for older browsers that don't implement the API.

The fix in this case is very simple (and to their credit, Twitter deployed the fix within a few hours of them finding out), check for the existence of the interface on the global first, then do the rest as usual.

```
this.getNotificationPermission = function() {
    return window.Notification && window.Notification.permission
}
```

(Note that they get this right in the `"PushManager" in window` feature detect in `isSupported`.)


[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=1331638
[api]: https://developer.mozilla.org/en-US/docs/Web/API/notification
[glob]: https://heycam.github.io/webidl/#es-interfaces