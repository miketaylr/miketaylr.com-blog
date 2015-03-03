---
layout: post
title:  WAP Telemetry in Firefox Mobile browsers, Part 1
date:   2015-03-03
---


Telemetry in Firefox is how we measure stuff in the browser&mdash;anything from how fast GIFS are decoded, to how many people opened the Dev Tools animation inspector. You can check out the collection of gathered results at [http://telemetry.mozilla.org][t] or see what your browser is sending (or disable it, if that's your thing) in about:telemetry.

One question the Web Compat team at Mozilla is interested in is whether Firefox for Android and Firefox OS users are being sent more than their fair share of WAP content (typically [WML][wml] or [XHTMLMP][xhtmlmp] sites).

(Personally, I missed out on WAP because I was too afraid to open the browser on my Nokia and have to pay for data in the early 2000s. (Also I didn't live in Japan.))

Here's the kind of amazing content that Firefox Mobile users are served and are unable to see:

<img src="https://miketaylr.com/posts/assets/nokia-good-times.jpg" alt="image I stole (without permission) from http://www.petecampbell.com" style="border: 1px solid #ccc">

Since Gecko doesn't know how to decode WAP, the browser [calls it a day][ditch] and treats it as `application/octet-stream` which results in a prompt for the user to download a page. Check out the dependencies of [these][reparse] [bugs][china] for some more of the gritty details.

As to why we're sent WAP stuff in the first place, this is likely due to old UA detection libraries that don't recognize the User Agent header. The logical assumption therefore is that this unknown browser is some kind of ancient proto-HTML capable graphing calculator. Naturally you would want to serve that kind of user agent WAP, rather than a Plain-Old HTTP Site (POHS).

So this seems like a pretty good opportunity to use Telemetry to measure how common happens. If it's happening all the time, we can push for some form of actual support in Gecko itself. But if it's exceedingly rare we can all move on with our lives, etc.

To measure this we landed a histogram named `HTTP_WAP_CONTENT_TYPE_RECEIVED`. I made a not-very-useful and mostly buggy visualization of the data we've gathered so far (from Nightly 39 users) using the [Telemetry.js API][api] here: [http://miketaylr.github.io/compat-telemetry-dashboards/wap.html][chart]. This updates every night and will need a few months of measuring before we can make any real decisions so I won't bother publishing any results just yet.

I will note that these patches haven't been taken up yet by Mozilla China's version of Firefox for Android which is one region we suspect receives more WAP than the West.

OK, so that's Part 1 of this exciting 2 part WAP Telemetry series. In Part 2 (which might get actually written tomorrow depending on a number of factors all of which are probably just laziness) I'll write out the more mundane technical details of landing a Telemetry patch in Gecko.




[t]: http://telemetry.mozilla.org
[wml]: http://en.wikipedia.org/wiki/Wireless_Markup_Language
[xhtmlmp]: http://en.wikipedia.org/wiki/XHTML_Mobile_Profile
[reparse]: https://bugzilla.mozilla.org/show_bug.cgi?id=941241
[china]: https://bugzilla.mozilla.org/show_bug.cgi?id=997668
[ditch]: https://dxr.mozilla.org/mozilla-central/source/netwerk/streamconv/converters/nsUnknownDecoder.cpp#566
[api]: http://telemetry.mozilla.org/docs.html
[chart]: http://miketaylr.github.io/compat-telemetry-dashboards/wap.html
[two]: https://miketaylr.com/posts/2015/03/wap-telemetry-how-two.html