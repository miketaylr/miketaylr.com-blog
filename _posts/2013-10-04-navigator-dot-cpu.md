---
layout: post
title:  Navigator Dot CPU Class
date:   2013-10-04
---

[1.30000000000000004 min read]

<!-- Totally not just written by hand for a joke -->

The other day people were sharing a link to a WebGL game that Microsoft made with Three.js, [Hover][hover]. It's pretty fun, you should try it out. It's basically Duke Nukem meets Neopets. I think&mdash;I didn't actually play it. Instead I jumped right into the JS source and ran into something I had never seen before: `navigator.cpuClass`.

{% highlight js %}
b.arm = "arm" === window.navigator.cpuClass, b.arm
  && a("html").addClass("arm")
{% endhighlight %}

Just to be clear, the `arm` property is an expando on `b`, an `HTMLCanvasElement`:

{% highlight js %}
var b = document.createElement("canvas");
{% endhighlight %}

So if `navigator.cpuClass` returns the string "arm", add an "arm" class to the document element. ok.jpg

There's a [page on MSDN][msdn] that defines `cpuClass`, which appears to be an IE-specific property. And I also learned `navigator.oscpu` is [also a thing][oscpu] implemented in Gecko.

The good news is that WebKit and Blink browsers don't implement either of these wacky interfaces.

Y'all, sniffing for CPU architecture on the web is weird.

 So let's not start using them now in 2013, deal? I have no idea what kind of optimizations the developers of Hover are trying to sneak in for Windows RT* tablets (or phones or whatever). But I think they're probably doing it at the wrong abstraction level.

\*<s>Retweet</s>
\*<s>Realtime</s>
\*<s>Really Touchy</s>
\*<s>Radical Tablet</s>
\*Nobody actually knows what RT means


[hover]: http://hover.ie/
[msdn]: http://msdn.microsoft.com/en-us/library/ie/ms533697%28v=vs.85%29.aspx
[oscpu]: https://developer.mozilla.org/en-US/docs/Web/API/Navigator.oscpu