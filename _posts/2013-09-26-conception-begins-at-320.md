---
layout: post
title:  Conception begins at 320 pixels
date:   2013-09-26
---

I was chatting with some friends in [IRC][ircLOL] trying to debug this [strange issue][twitterbug] reported by my friend Brian Brennan. (I'll write about that some other day, perhaps, but if you're truly curious you can check out [Bug 920342][bug]). Celebrated Medium author [Jenn $chiffer][medium] then pointed out another site to me that does something similar: [http://fortydaysofdating.com/][40].

If you happen to visit that site from a desktop browser, feel free to resize the viewport and ooh and aah at the amazing content breakpoint transformations. It's really well done. And then, right around 480px wide, you'll see this interesting suggestion. If you're using the Chrome browser (as all proper [Lady Gaga fans do][gaga]), you can even resize it as small as the browser window will allow, 320px:

<img src="https://miketaylr.com/posts/assets/rotate.png" style="border:1px solid #ccc" alt="Screenshot of a website suggesting I rotate my (desktop) browser.">

Believe it or not, rotating my Macbook didn't fix it. :(

So let's look at the code that is responsible:

{% highlight css %}
/* 320px and greater (iPwn portrait) */
@media only screen and (min-width: 320px) {
  #mobile_portrait {
    display: block;
    width: 100%;
    height: 204px;
    position: absolute;
    left: 0px;
    top: 125px;
    background-image: url('img/spinny_phoney.png');
    background-size: 250px 204px;
    background-position: center;
    background-repeat: no-repeat;
  }

#content, #masthead ul, footer {display:none;}
}

/* 480px and greater (iPwn landscape) */
@media only screen and (min-width: 480px) {
  /* ...turn everything back on */

{% endhighlight %}

(I'd just like to state for the record that the "iPwn" comment is straight from the site's source. I don't get it either.)

So the deal is, if your viewport is 320px wide you're an iPhone and you get an image telling you to rotate your iPhone meanwhile everything useful gets hidden. Killer UX. 480px means it's an iPhone in landscape mode and you're finally prepared to take in the story of... I actually have no idea. Dating advice or something.

Now let me blow your little minds, dear readers:

<img src="https://miketaylr.com/posts/assets/presto.png" style="border:1px solid #ccc" alt="Screenshot of Opera 12 at a width smaller than 320px.">

Opera Classic™ allows you to go even smaller. Suddenly the rotate graphic is gone and you find yourself in the midst of a nether breakpoint where anything goes (and frankly it feels a _little_ weird. Maybe even a good weird?). 

So remember kids, be very careful what you do with `min-width`. It's powerful stuff.

But I shouldn't complain too much. At least you can see the content without trying (and failing) to rotate your laptop.

[ircLOL]: http://en.wikipedia.org/wiki/Indian_River_County,_Florida?lol=JOKES
[twitterbug]: https://twitter.com/brianloveswords/status/382672513136070656
[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=920342
[medium]: https://medium.com/cool-code-pal/fa21a8762a15
[40]: http://fortydaysofdating.com/
[gaga]: http://vimeo.com/24094764