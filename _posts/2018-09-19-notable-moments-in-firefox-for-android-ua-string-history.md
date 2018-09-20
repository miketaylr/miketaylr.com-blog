---
layout: post
title: Notable moments in Firefox for Android UA string history
date: 2018-09-19
---

Back by popular demand*, here's a follow-up to my [blog post on Firefox Desktop UA string changes][prev].

(\*actual demand for this blog post asymptotically approaches zero the further you read)

Since version 41, Firefox for Android has (generally) followed the following UA string format:

Mozilla/5.0 (Android &lt;androidversion>; &lt;devicecompat>; rv: &lt;geckoversion>) Gecko/&lt;geckoversion> Firefox/&lt;firefoxversion>


<style>table, th, td {
   border: 1px solid black;
   padding: 5px;
}table {padding: 1px;}</style>
<table>
<tr>
  <th>Gecko Version</th>
  <th>Sample Firefox for Android UA string</th>
</tr>
<tr>
  <td>11</td>
  <td>Mozilla/5.0 (Android; Tablet<sup><a href="#fn1">1</a></sup>; rv:11.0) Gecko/11.0 Firefox/11.0 Fennec/11.0</td>
</tr>
<tr>
  <td>11</td>
  <td>Mozilla/5.0 (Android; Tablet; rv:11.0) Gecko/11.0 Firefox/11.0<sup><a href="#fn2">2</a></sup></td>
</tr>
<tr>
  <td>41</td>
  <td>Mozilla/5.0 (Android 4.4.4<sup><a href="#fn3">3</a></sup>; Mobile; rv:41.0) Gecko/41.0 Firefox/41.0</td>
</tr>
<tr>
  <td>46</td>
  <td>Mozilla/5.0 (Android 4.4.4; Mobile; CoolDevice<sup><a href="#fn4">4</a></sup>; rv:46.0) Gecko/46.0 Firefox/46.0</td>
</tr>
<tr>
  <td>46</td>
  <td>Mozilla/5.0 (Android 4.4.4; Mobile; Custom CoolDevice/ABCDEFG<sup><a href="#fn5">5</a></sup>; rv:46.0) Gecko/46.0 Firefox/46.0</td>
</tr>
</table>


Footnotes:
<sup id="fn1">1. Version 11 added the notion of a [&lt;devicecompat&gt; token to distinguish between Tablet and Mobile][fn1].</sup>

<sup id="fn2">2. Version 11 also [dropped the Fennec/&lt;version&gt;][fn2] token for Native UI (non-XUL) builds.</sup>

<sup id="fn3">3. For versions running on Android older than KitKat (v4), the Android version number is set to 4.4<sup><a href="#fn2tfn-1">1</a></sup> to avoid [UA sniffing assumptions tied to the Android platform capabilities][fn3].
</sup>
<sup id="fn4">4. Version 46 also added the ability to add the Android device name, controlled by the pref `general.useragent.use_device`. This is probably not widely used, if at all.</sup>

<sup id="fn5">5. Version 46 added the ability to add a custom device string, with optional device ID, controlled by the pref `general.useragent.device_string`. This is also probably not widely used, if at all.
</sup>

Footnotes to the Footnotes:
<sup id="fn2tfn-1">1. If I had to write this patch again, I would choose an obviously non-real 4.4.99, so it would be sniffable as a spoofed value. Yes, I am familiar with the concept of irony.</sup>

[fn1]: https://bugzilla.mozilla.org/show_bug.cgi?id=671634#c79
[fn2]: https://bugzilla.mozilla.org/show_bug.cgi?id=671634#c69
[fn3]: https://bugzilla.mozilla.org/show_bug.cgi?id=1169772#c7

[prev]: https://miketaylr.com/posts/2018/07/notable-moments-in-firefox-desktop-ua-string-history.html
