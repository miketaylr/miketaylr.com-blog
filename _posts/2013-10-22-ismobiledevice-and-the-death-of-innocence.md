---
layout: post
title:  isMobileDevice and the death of innocence [SPOILERS]
date:   2013-10-22
---

Trying to keep up with the [cool kids][coolkids] I spent a little time last night reading through some of the source code of [healthcare.gov][thx]. Naturally the one thing that caught my attention was a snippet of script which attempts to do mobile device detection from [healthcare.gov/js/all.js][all]

{% highlight js %}
// checks screen size
var isMobile = {
    any: function() {
        return($(window).width()<=599);
        // nexus7 width is 600 (window.innerWidth)
        // this will not run any reformatting for the phone layout on nexus
    }
};

// checks device
var isMobileDevice = {
    Android: function() {
        return navigator.userAgent.match(/Android/i);
    },
    BlackBerry: function() {
        return navigator.userAgent.match(/BlackBerry/i);
    },
    iOS: function() {
        return navigator.userAgent.match(/iPhone|iPad|iPod/i);
    },
    Opera: function() {
        return navigator.userAgent.match(/Opera Mini/i);
    },
    Windows: function() {
        return navigator.userAgent.match(/IEMobile/i);
    },
    any: function() {
        return (isMobileDevice.Android() ||
          isMobileDevice.BlackBerry() ||
          isMobileDevice.iOS() ||
          isMobileDevice.Opera() ||
          isMobileDevice.Windows());
    }
};
{% endhighlight %}

What's most interesting to me about this particular chunk of code, other than hard-coding multiple [SPOF][spof] (MSOPS?!) for your mobile strategy is the fact that variations of this script [keep][keeps] [popping][popping] [up][up]. It's all over the web.

As far as I can tell this script originates on a blog post from 2011, [http://www.abeautifulsite.net/blog/2011/11/detecting-mobile-devices-with-javascript/][blah]. And somehow via the magic of copy and paste it ends up in healthcare.gov in 2013.

(Good luck getting that script updated for the thousands of sites and applications, you say to yourself, where it's laying dormant just waiting to send devices the wrong content based on a UA substring.)

This kind of pattern we see time and time again makes me a little bit sad (blog once, destroy everywhere). Don't be too hard on yourself if you've used this same script though&mdash;I've been prone to sadness ever since I saw E.T. as a small child. :(

<img src="https://miketaylr.com/posts/assets/et.jpg" alt="Picture of dying E.T.">

Spoiler alert: E.T. actually pulled through; so let's continue trying to build a better web for everyone.



[coolkids]: http://www.youtube.com/watch?feature=player_detailpage&v=zQ00laVt62c#t=37
[thx]: https://miketaylr.com/posts/assets/thxobama.gif
[all]: https://www.healthcare.gov/js/all.js
[keeps]: https://github.com/cemerson/PhoneGap-3-Boilerplate/blob/2e8f2fc9df048a91f6b7519940b5398455b8ca0c/www/js/index.js#L47-L63
[popping]: https://github.com/nbicramiel/nbicramiel.github.com/blob/00e86b5e2748d454d1794f639784d69a09b4b611/_site/Out-of-Control/index.html#L67-L86
[up]: https://github.com/mozilla/togetherjs/blob/5164f3c82299a962c90b4bf5b0d65f958f4e1d36/site/js/parallax.js#L3-L23
[spof]: http://en.wikipedia.org/wiki/Single_point_of_failure
[blah]: http://www.abeautifulsite.net/blog/2011/11/detecting-mobile-devices-with-javascript/