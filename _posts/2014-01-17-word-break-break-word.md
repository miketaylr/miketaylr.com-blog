---
layout: post
title:  More than anyone needs to know about word-break colon break-word.
date:   2014-01-17
---

(Funny how Jekyll barfs on `title: word-break: break-word`.)

ANYWAYS.

I just learned a whole bunch of stuff about CSS3 Text and word-breaks and word-wraps and hyphens (but not run-on sentences?).

[Bug 959735][bug] tells the tale of truncated yet non-hyphenated text on wired.com's mobile site in Firefox browsers. Like so (Firefox left, Chrome right):

<img style="border: 1px solid #ccc;" src="https://miketaylr.com/posts/assets/wired-mobile.png">

The offending CSS is here:

``` css
.x-article #x-test #article-wrap .entry p,
.x-article #x-test #article-wrap .entry li {
    -ms-word-break:break-all;
    word-break:break-all;
    word-break:break-word;
    -webkit-hyphens:auto;
    -moz-hyphens:auto;
    hyphens:auto;
    ...
}
```

So why aren't there any hyphens appearing in Firefox, which supports CSS hyphens? I [binged][ddg] some things and lots of example code on the web looks pretty darn similar to what wired.com has. It turns out `hyphens: auto` here won't actually do what the authors intended.

After reading some specs and stackoverflows I learned that `word-break: break-all` is really intended for predominantly CJK scripts (from what I gather), where breaking up a "word" (grapheme cluster?) isn't such a big deal, and `word-wrap: break-word` really is for non-CJK scripts.

OK, cool. But what the heck is `word-break: break-word`? It's not in the [CSS3 Text spec][spec], the only valid values for `word-break` are `normal`, `keep-all`, and `break-all`.

This comment from WebKit's [RenderStyleConstants.h][render] suggests that it was added by WebKit (which Blink inherited) for IE compatibility.

``` c++
// Word Break Values. Matches WinIE, rather than CSS3

enum EWordBreak {
    NormalWordBreak, BreakAllWordBreak, BreakWordBreak
};
```

So let's go back to the bug.

Even though Chrome doesn't support CSS hyphens, it understands `word-break: break-word` and treats it like `word-wrap: break-word`. Safari does the same, but it actually supports CSS hyphens so it looks a little nicer. Safari also appears to add hyphens to non-CJK text when using `word-break: break-all` (even without `hyphens: auto`). You can check out this [test page I made][test] if you're curious.

From what I can tell, IE7 (and below?) treated `word-break: break-word` as a synonym to `word-wrap: break-word` (or at least it did the same thing). At some point WebKit needed that for compat, so they added it. Note that IE8 and above, if you can trust IE11 in that version's document mode doesn't do anything with `word-wrap: break-word`. See this [screenshot from the bug][ie].

Now the 64 bitcoin question is, "is `word-break: break-word` important for web compatibility today?" It seems like wired.com is relying on that behavior (on accident, I think), but we'll do our best to ask them to remove it (and remove `word-break: break-all` too&mdash;they already have `word-wrap: break-word` on `body`, which is all they need).

[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=959735
[ddg]: https://lmddgtfy.net/?q=bing
[spec]: http://www.w3.org/TR/css3-text/#word-break-property
[render]: https://github.com/WebKit/webkit/blob/e887e1aa1b7be17a8d160e869790c320b08f7ae0/Source/WebCore/rendering/style/RenderStyleConstants.h#L219-L223
[test]: https://bug959735.bugzilla.mozilla.org/attachment.cgi?id=8361943
[ie]: https://bug959735.bugzilla.mozilla.org/attachment.cgi?id=8361967