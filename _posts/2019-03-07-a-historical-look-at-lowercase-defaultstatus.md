---
layout: post
title:  A historical look at lowercase defaultstatus
date:   2019-03-08
---

The other day I was doing some research on DOM methods and properties that Chrome implements, and has a usecounter for, but don't exist in Firefox.

[`defaultstatus`](https://www.chromestatus.com/metrics/feature/timeline/popularity/358) caught my eye, because like, there's also a use counter for [`defaultStatus`](https://www.chromestatus.com/metrics/feature/timeline/popularity/357).

(The discerning reader will notice there's a lowercase and a lowerCamelCase version. The less-discerning reader should maybe slow down and start reading from the beginning.)

As far as I know, there's no real spec for these old BOM (Baroque Object Model) properties. It's supposed to allow you to set the default value for `window.status`, but it probably hasn't done anything in your browser for years.

<img src="https://miketaylr.com/posts/assets/bom.png" alt="image of some baroque art shit">

Chrome inherited lowercase `defaultstatus` from Safari, but I would love to know why Safari (or KHTML pre-fork?) added it, and why Opera, Firefox or IE never bothered. Did a site break? Did someone complain about a missing status on a page load? Did this all stem from a typo?

DOMWindow.idl has the following similar-ish comments over the years and probably more, but nothing that points to a bug:

> [This attribute is an alias of defaultStatus and is necessary for legacy uses.][comment2]
> [For compatibility with legacy content.][comment]

It's hard to pin down exactly when it was added. It's in [Safari 0.82's kjs_window.cpp][82]. And in this ["old" kde source tree][old] as well. It is in [current KHTML sources][current], so that suggests it was inherited by Safari after all.

Curious to see some code in the wild, I did some bigquerying with [BigQuery on the HTTPArchive dataset][bq] and got a [list of ~3000 sites][httpar] that have a lowercase `defaultstatus`. Very exciting stuff.

There's at least 4 kinds of results: 

1) False-positive results like `var foo_defaultstatus`. I could re-run the query, but global warming is real and making Google cloud servers compute more things will only hasten our own destruction.

2) User Agent sniffing, but without looking at `navigator.userAgent`. I guess you could call it User Agent inference, if you really cared to make a distinction.

Here's an example from some webmail script:

```js 
O.L3 = function(n) {
    switch (n) {
        case 'ie':
            p = 'execScript';
            break;
        case 'ff':
            p = 'Components';
            break;
        case 'op':
            p = 'opera';
            break;
        case 'sf':
        case 'gc':
        case 'wk':
            p = 'defaultstatus';
            break;
    }
    return p && window[p] !== undefined;
}
```

And another from some kind of design firm's site:

```js
browser = (function() {
    return {
        [snip]
        'firefox': window.sidebar,
        'opera': window.opera,
        'webkit': undefined !== window.defaultstatus,
        'safari': undefined !== window.defaultstatus && typeof CharacterData != 'function',
        'chrome': typeof window.chrome === 'object',
        [snip]
    }
})();
```


3a) Enumerating over global built-ins. I don't know why people do this. I see some references to Babel, Ember, and JSHint. Are we making sure the scripts aren't leaking globals? Or trying to overwrite built-ins? Who knows.

3b) Actual usage, on old sites. Here's a few examples:

```html
<body background="images/bvs_green_bkg.gif" bgcolor="#598580" text="#A2FF00" onload="window.defaultstatus=document.title;return true;">
```

```html
<body onload="window.defaultstatus='Индийский гороскоп - ведическая астрология, джйотиш онлайн.'">
```

This one is my favorite, and not just because the site never calls it:

```js
function rem() {
  window.defaultstatus="ok"
}
```

OK, so what have we learned? I'm not sure we've learned much of anything, to be honest.

If Chrome were to remove `defaultstatus` the code using it as intended wouldn't break&mdash;a new global would be set, but that's not a huge deal. I guess the big risk is breaking UA sniffing and ended up in an unanticipated code-path, or worse, opting users into some kind of "your undetected browser isn't supported, download Netscape 2" scenario.

Anyways, `window.defaultstatus`, or `window.defaultStatus` for that matter, isn't as cool or interesting as Caravaggio would have you believe. Thanks for reading.

[ss]: https://docs.google.com/spreadsheets/d/1zxE-jEaYsyCkYTi99HuatJ4WEnAcD2GOrudXEIp9qbs/edit#gid=1464363467
[82]: https://svn.webkit.org/repository/webkit/branches/old/Safari-0-8-2-branch/WebCore/khtml/ecma/kjs_window.cpp
[old]: https://svn.webkit.org/repository/webkit/branches/old/kde/WebCore/khtml/ecma/kjs_window.cpp
[comment]: https://github.com/WebKit/webkit/blob/4153be964bb532249fce479b355dea0623a62f4e/Source/WebCore/page/DOMWindow.idl#L156
[comment2]: https://github.com/WebKit/webkit/blob/f17d774bb6a8dffda7311a731e090628c1bd350a/Source/WebCore/page/DOMWindow.idl#L124
[current]: https://github.com/KDE/khtml/blob/7d3ed6b96e82c248abd27e669b81362f2fcb3c4b/src/ecma/kjs_window.cpp#L400
[httpar]: https://docs.google.com/spreadsheets/d/1zxE-jEaYsyCkYTi99HuatJ4WEnAcD2GOrudXEIp9qbs/edit#gid=1464363467
[bq]: https://github.com/HTTPArchive/httparchive.org/blob/master/docs/gettingstarted_bigquery.md