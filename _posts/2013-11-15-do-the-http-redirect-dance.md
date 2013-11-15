---
layout: post
title:  Requesting 🍔 or, if you don't see a cheeseburger, reconsider your life's decisions.
date:   2013-11-15
---

Let's imagine you were crazy and decided to make your own mobile web browser. And instead of copying bits of User Agent strings past for web compatibility reasons, you just stuck to your guns (and maybe your branding your nephew who is good at Photoshop came up with) and put in your new browser's name. Let's call this imaginary (but oh-so-disruptive!) browser the 🍔 browser.

So it sends the User-Agent header like so: `User-Agent: 🍔`

Let's compare that to two new-ish browsers and their UA strings:

[IE11][IE11]: `Mozilla/5.0 (Windows NT 6.3; Trident/7.0; rv:11.0) like Gecko`

[Ubuntu Phone Mobile][ubu]: `Mozilla/5.0 (Ubuntu; Mobile) WebKit/537.21`

Some parts are authentic. Some parts are lies, i.e., they opted for web compatibility over some sweet viral maketing via HTTP header (LOL NOOBS).

So in this new mobile browser you want to visit the [abril.com.br][ab] site. You just ditched your iPhone because your dad didn't get you the gold one (ugh, dads). And besides, you wanna dogfood your new 🍔 browser.

Luckily your 🍔 browser is able to import iCloud Bookmarks (or whatever they're called), so you click on [http://wap.abril.com.br][wap] (which you bookmarked from your gross old-non-gold iPhone).

(Note that I'm just printing the trimmed response headers below.)


``` http
$ http --print=h GET http://wap.abril.com.br/ User-Agent:"🍔"
HTTP/1.1 302 Found
Location: http://wap.abril.com.br/tc/abril/
```

OK, nbd, your browser gets a 302 and needs to jump to http://wap.abril.com.br/tc/abril/.

``` http
$ http --print=h GET http://wap.abril.com.br/tc/abril/ User-Agent:"🍔"
HTTP/1.1 302 Found
Location: http://www.abril.com.br/celular
```

Uh, OK. Let's march along. The `/celular` URL looks more appropriate than a WAP site, given that it's not the Victorian era anymore.

``` http
$ http --print=h GET http://www.abril.com.br/celular User-Agent:"🍔"
HTTP/1.1 301 Moved Permanently
Location: http://www.abril.com.br/celular/
```

Oh right. 301 to `/celular/`.

``` http
$ http --print=h GET http://www.abril.com.br/celular/ User-Agent:"🍔"
HTTP/1.1 301 Moved Permanently
Location: http://www.abril.com.br/maismob
```

Um.. or not. Let's go to `/maismob`?

``` http
$ http --print=h GET http://www.abril.com.br/maismob User-Agent:"🍔"
HTTP/1.1 301 Moved Permanently
Location: http://www.abril.com.br/maismob/
```

You're literally crying at this point. But you didn't come this far to give up, you say to yourself as you turn up some inspirational Katy Perry music.

``` http
$ http --print=h GET http://www.abril.com.br/maismob/ User-Agent:"🍔"
HTTP/1.1 200 OK
wapbypass: 1
```

Success! The server rewards you with a 200 OK and 1 `wapbypass` point, which is probably like bitcoins. Your browser parses and downloads all the assets, renders it and... you're actually on some kind of [weird site that allows you to subscribe to SMS updates of half-naked people][maismob], not the mobile site you entered into the 🍔 browser.

So you give up on the mobile web browser business entirely and think about disrupting the VHS rental card membership market by coming up with some cloud platform to consolidate those cumbersome [Blockbuster][bb] and [West Cost Video][wc] cards into a single card. BAM.

[IE11]: http://blogs.msdn.com/b/ieinternals/archive/2013/09/21/internet-explorer-11-user-agent-string-ua-string-sniffing-compatibility-with-gecko-webkit.aspx
[ubu]: http://askubuntu.com/questions/372930/which-user-agent-string-does-the-web-browser-on-mobile-devices-have
[maismob]: http://www.abril.com.br/maismob/
[wc]: http://en.wikipedia.org/wiki/West_Coast_Video#2000s:_New_formats.2C_store_closures.2C_chain_defunct_and_independence_of_remaining_stores
[bb]: http://en.wikipedia.org/wiki/Blockbuster_LLC#Continued_decline_and_exit_from_the_retail_market
[wap]: http://wap.abril.com.br
[ab]: http://abril.com.br