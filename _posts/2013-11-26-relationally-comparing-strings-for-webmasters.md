---
layout: post
title:  (Relationally) Comparing Strings in JavaScript, WebMaster edition
date:   2013-11-26
---

Alternate title, _Learning The Abstract Relational Comparison Algorithm in 24 minutes or less or your money back guaranteed (with proof of purchase)_.

Last night I spent a little time going through this [meta bug][mbug] which collects sites that broke during the first wave of browsers going from single to double digit version numbers.

(A moment of silence for those WebMasters who had to endure through those trying times, lest we forget).

This particular snippet of code from [Bug 695901][bug695901] was especially memorable:

``` html
<!-- REDIRECT - OLDER BROWSERS to ORIGINAL WEBSITE -->
<script type="text/javascript">
    var url = "http://www.smartusa.com/home/overview.aspx";
    if (($.browser.msie) && ($.browser.version < "7")) {
        window.location.replace(url);
    }
    if (($.browser.mozilla) && ($.browser.version < "3.4.0")) {
        window.location.replace(url);
    }
    if (($.browser.safari) && ($.browser.version < "4")) {
        window.location.replace(url);
    }
</script>
```

For the sake of the up and coming WebMaster generation, I'm going to outline how relational string comparison works, so when your favorite browser hits version 100, instead of wondering why all your `"$.browser.version" < "72.4"` scripts aren't behaving as you would expect, you can be out hoverboarding the streets of futures-ville.

The trick to understanding this is defined in the [Abstract Relational Comparison Algorithm][spec] (referred to as the ARCA, by nobody). You can skip all those scary words until Step 4.

There's two parts in the ARCA to understanding how the stringy browser version comparison in [Bug 695901][bug695901] fails. The first is the spec notion of "prefix-hood" and the second is lexicographic ordering.

The spec says (in the context of x < y):

"If py is a prefix of px, return false."
"If px is a prefix of py, return true."

And it defines a prefix like so: "A String value p is a prefix of String value q if q can be the result of concatenating p and some other String r. Note that any String is a prefix of itself, because r may be the empty String."

I had to write that down on a napkin at Whataburger before that made sense to me. Let's look at some examples.

If we're comparing `"10" < "100"`, the result is `true`, because you can concatenate the string "0" with "10" and get the result "100". Amazing! Hence, "10" is a prefix of "100".

What's `"100" < "10"` return?

`false`&mdash;because "100" is not a prefix of "10".

For extra credit now, what's `"10" < "10"`?

`false`&mdash;because "any String is a prefix of itself".

As a final exercise for the reader, see if you can figure out what `"pee" < "peepoo"` evals to.

OK, so you're halfway there with this "prefix-y" knowledge.

The fancy-pants term "lexicographic ordering" is something you've been doing since second grade. It just means alphabetically ordering strings, one character at a time from left to right (at the code unit value, just like in second grade). Steps c. through f. of the ARCA have all the gorey details, but the tl;dr of why `"10" < "3.4.0"` evals to `true` is because `"1" < "3"` evals to `true`.

Hoverboard on, future WebMasters.

[spec]: http://es5.github.io/#x11.8.5
[mbug]: https://bugzilla.mozilla.org/show_bug.cgi?id=690287
[bug695901]: https://bugzilla.mozilla.org/show_bug.cgi?id=695901#c3