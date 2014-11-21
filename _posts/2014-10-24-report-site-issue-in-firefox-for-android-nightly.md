---
layout: post
title:  Report Site Issue in Firefox for Android Nightly
date:   2014-10-24
---

Last week we [landed][landed] support for a "native" version of the Webcompat Reporter addon in Firefox for Android Nightly.

If you're using [Nightly][nightly] on your phone, you might just be the kind of person who notices and reports broken, weird, or otherwise bogus sites. To make that easier, select "Report Site Issue" menu item and you'll end up on [webcompat.com][wc] with the URL of the page pre-filled. You can fill out the rest of the details and either report the problem anonymously, or with your GitHub credentials.

<img src="https://miketaylr.com/posts/assets/reporter.png" alt="screenshots of report site issue menu item" style="max-width:100%">

Additionally, if you select "Request Desktop Site" (for whatever reason) you'll get a prompt to report an issue.

If you have any feedback or run into any other issues, please [file a bug][file] and feel free to cc miket@mozilla.com. He's a pretty cool guy.

[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=1024807
[landed]: https://hg.mozilla.org/mozilla-central/rev/bb58a5fc381b
[nightly]: https://nightly.mozilla.org/
[file]: https://bugzilla.mozilla.org/enter_bug.cgi?product=Firefox%20for%20Android&component=General
[wc]: https://webcompat.com