---
layout: post
title:  Making WPTouch compatible with Firefox OS
date:   2013-10-14
---

There are a lot of WordPress based sites using the [WPTouch plugin][wptouch] to serve mobile sites. As of today, though, [WPTouch doesn't recognize the Firefox OS user agent string as a mobile device][bug]. Hopefully that will change in the near future; we're working on it.

But you can easily configure your instance of WPTouch so it can serve the mobile version of your site to Firefox OS users.

From the admin dashboard, navigate to Plugins > Installed Plugins, find the WPTouch Plugin and click on Settings. In the *Advanced Options* section you can add a comma-separated list of user-agents to enable the mobile version for. But don't worry about the entire UA string; it will accept a substring. In the input simply enter the string "mobi" and then click on the "Save Options" button and you're all set. (Since you're in there, go ahead and click the "Allow zooming on content" option because that's just common courtesy.)

<img src="https://miketaylr.com/posts/assets/custom.png" style="border:1px solid #ccc" alt="Screenshot of WPTouch advanced settings.">

One last thing to note is you might want to fix the broken gradient in the `.comment-bubble` style (otherwise it will have a transparent background for modern Gecko-powered browsers). Replace the old prefixless style in `plugins/wptouch/themes/default/style.css` with the following:

{% highlight css %}
.comment-bubble {
  /* ... */
  background: #be4958;
  background: linear-gradient(-45deg, #a40717, #be4958 50%, #de939e);
  /* ... */
}

{% endhighlight %}

With that done, you now have a Wordpress blog that serves mobile optimized content to Firefox OS users and are one step closer to swimming through your vault of VC cash money.

<img src="https://miketaylr.com/posts/assets/mobile.png" style="border:1px solid #ccc" alt="Screenshot of our site with a working mobile theme applied.">

[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=909420
[wptouch]: http://wordpress.org/plugins/wptouch/