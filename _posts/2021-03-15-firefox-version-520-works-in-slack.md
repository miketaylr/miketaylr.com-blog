---
layout: post
title:  Slack is optimized for Firefox version 520
date:   2021-03-15
---

Last week my pal [Karl][karl] sent me a link to [web-bug 67866][67866]: which has the cool title "menu buttons don't work in Firefox version 100". It turns out that Mozilla's [Chris Peterson][chris] has been surfing the web with a spoofed UA string reporting version 100 to see what happens (because he knows the web can be a hot mess, and that [history is bound to repeat itself][ms]).

The best part of the report, IMHO, is Chris' comment:

> I discovered Slack's message popup menu's buttons (such as "Add reaction" or "Reply in thread") stop working for Firefox versions >= 100 and <= 519. They mysteriously start working again for versions >= 520.

(I like to imagine he manually bisected Firefox version numbers from 88 to 1000, because it feels like something wacky that I would try.)

<hr>
&lt;aside&gt; + spoiler
The bug described below is a slightly different class of bugs than the typical "regexp expected a fixed set of integers and fell on its face, which is where the famous Bill Gates quote "9 major browser versions ought to be enough for anybody" came from (see also [Opera version 10 drama][op], and [macOS version 11 drama][mac], etc). And still a different class of bugs from the even more [fun and possibly not true explanation][win] for Windows 10 coming after Windows 8 (because software was sniffing for strings that started with "Windows 9") and going down paths assuming Windows 95 or Windows 98).

<img alt="picture of bill gates saying 9 browser version should be enough for anybody" src="https://miketaylr.com/posts/assets/billy.png" style="border:1px solid #ccc">

&lt;/aside&gt;

<hr>

Broken website diagnosis wizard [Tom Wisniewski followed a hunch][comment] that Slack was doing string comparison on version numbers, and found the following code:

```js
  const _ = c.a.firefox && c.a.version < '52' ||
            c.a.safari && c.a.version < '11'
              ? h
              : 'button',
```

(We're just going to ignore what `h` is, and the difference between that and `button` presumably solving some cross-browser interop problem; I trust it's very clever.)

So this is totally valid JS, but someone forgot that [comparison operators for strings in JS][compare] do alphanumeric comparison, comparing each character between the two strings (we've all been there).

So that's how you get the following comparisons that totally work, until they totally don't:

```js
> "10" < "1"
false // ok

> "10" < "20"
true // sure

> "10" < "2"
true // lol, sure why not
```

So, how should you really be comparing stringy version numbers? Look, I don't know, this isn't leetcode. But maybe "search up" (as the kids say) [`String.prototype.localeCompare`][lc] or [`parseInt`][pi] and run with that (or don't, I'm not in charge of you).

[karl]: https://la-grange.net/
[67866]: https://github.com/webcompat/web-bugs/issues/67866
[chris]: https://cpeterso.com/blog/
[ms]: https://devblogs.microsoft.com/oldnewthing/20040213-00/?p=40633
[comment]: https://github.com/webcompat/web-bugs/issues/67866#issuecomment-796980688
[op]: https://web.archive.org/web/20091106191317/http://my.opera.com/chooseopera/blog/2009/05/29/changes-in-operas-user-agent-string-format
[mac]: https://bugzilla.mozilla.org/show_bug.cgi?id=1679929#c4
[win]: https://www.reddit.com/r/technology/comments/2hwlrk/new_windows_version_will_be_called_windows_10/ckwq83x/
[compare]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#comparing_strings
[lc]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare
[pi]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt