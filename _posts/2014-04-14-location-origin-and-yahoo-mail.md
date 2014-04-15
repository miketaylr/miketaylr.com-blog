---
layout: post
title:  window.location.origin and Yahoo! Mail
date:   2014-04-14
---

Here's a pop quiz to keep you [on your toes][gross].

What's the relationship between `location.origin` and `navigator.registerProtocolHandler`?

So if you responded, "huh?". You are correct. There is no relationship.

Also acceptable was "these are both boring browser things."

But in [bug 996013][bug] we can see an example where someone thought having `navigator.registerProtocolHandler` entails the existence of the `location.origin` interface:

```js
if (navigator.registerProtocolHandler) {
  var Bi=location.origin+"/blah=%s";
  ...
}
```

It turns out this assumption breaks Yahoo! Mail entirely if you have one and not the other, which is the case for Firefox versions 3 to 20. (And because you're so good at math and quizzes and stuff, you've already figured out that `location.origin` was [added in Firefox 21][21].)

So how can we fake `window.location.origin` for Firefox < 20 and other browsers that don't implement it yet, such as Internet Explorer?

First, check out the algorithm on how to compute the [origin of a URI][origin] and then hop to the section on serializing that jazz into a string. Basically, return the following: `scheme + "://" + host`. You need to append the port if it's different than the default port for the scheme, e.g., port 80 in http.

You may as well read how the URL spec defines the [URLUtils and the (cleverly named) URLUtilsReadOnly interfaces][spec] since you're already in boring spec land (SPOILER: it just tells you to go back to the RFC you just read but grab the Unicode serialization, rather than ASCII).

With that in mind, you've already got the tools to build a fake `location.origin` object:

```js
if (!location.origin) {
  location.origin = location.protocol + "//" + location.host
}
```

We don't need to worry about the port, because `location.host` will include for us if needed.

So what's the lesson that we've all hopefully learned by this point? Umm, try to not base your logical-entailments-as-code on a single browser?

(Yeah, I don't know know either).

[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=996013
[gross]: https://miketaylr.com/posts/assets/on-your-toes.gif
[origin]: http://tools.ietf.org/html/rfc6454#page-10
[21]: https://bugzilla.mozilla.org/show_bug.cgi?id=828261
[urlutils]: https://developer.mozilla.org/en-US/docs/Web/API/URLUtils.origin
[spec]: http://url.spec.whatwg.org/#urlutils-and-urlutilsreadonly-members