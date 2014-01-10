---
layout: post
title:  Thanks Google?
date:   2014-01-10
---

One of the bug challenges of mobile web compatibility is getting served the appropriate mobile content, usually because you're dealing with legacy software that doesn't know about `$YOUR_MOBILE_BROWSERS_USER_AGENT_STRING`.

Take [http://nydailynews.com][nyd], fine purveyor of New Jersey Governor Chris Christie bridge scandal reporting, as an example.

Firefox for Android:

``` http
$ http --print=Hh GET http://www.nydailynews.com/ User-Agent:"$FFA"
GET / HTTP/1.1
Host: www.nydailynews.com
User-Agent: Mozilla/5.0 (Android; Mobile; rv:26.0) Gecko/26.0 Firefox/26.0

HTTP/1.0 302 Found
Location: http://m.nydailynews.com/
```

Firefox OS:

``` http
$ http --print=Hh GET http://www.nydailynews.com/ User-Agent:"$FFOS"
GET / HTTP/1.1
Host: www.nydailynews.com
User-Agent: Mozilla/5.0 (Mobile; rv:18.1) Gecko/18.1 Firefox/18.1

HTTP/1.1 200 OK
Blah: blah blah
```
So you can see that NYDailyNews somehow knows that Firefox for Android is a mobile browser and redirects it to the m-dot site, while Firefox OS get the regular desktop content.

Not the end of the world, but not ideal ([Bug 957701][bug] describes what is likely an OOM tab crash as a result).

(OK, so why are you even blogging about this Mike, it happens everywhere, you're probably thinking to yourself.)

This week I've started to notice that if you click on search results from Google.com in the Firefox OS browser, they'll automatically send you to the mobile site. The link for the first result for "nydailynews" is actually `http://www.google.com/url?q=http://m.nydailynews.com/&blahblah=gibberish`.

It seems like Google is sending mobile users directly to the mobile site if it a) knows you're on a mobile device, and  b) it knows about a mobile site. I've noticed this on makemytrip.com, rebus.in and in.bookmyshow.com as well. Thanks Google?

[nyd]: http://www.nydailynews.com/
[http]: https://github.com/jkbr/httpie
[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=957701#c8