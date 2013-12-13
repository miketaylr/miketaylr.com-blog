---
layout: post
title:  Anonymous table box creations and Google Plus Mobile
date:   2013-12-13
---

In [Bug 928074][bug] I thought Gecko had a bug with `display: table-cell` elements wrapped by `<a>` elements. In a nutshell, you couldn't click or tap anywhere in the full width of Google Plus Mobile's menu&mdash;it had to be exactly on top of the menu item text for anything to happen.

But I was wrong, <a href="https://miketaylr.com/posts/assets/crickets.gif">turns out</a>.

[Daniel Holbert][dan] gets credit for finding the actual bug and kindly explaining it to me&mdash;I'm just writing it up so I can commit some of this to memory.

Per the [CSS 2.1 spec][spec], implementations should generate anonymous table elements around elements with CSS table model display values (if needed). In the case of the Google Plus menu, `display:table-cell` elements parented by an `<a>` like so:

&lt;li&gt;
  &lt;a&gt;
    &lt;div&gt;Home&lt;/div&gt;
  &lt;/a&gt;
&lt;/li&gt;

The `<div>` requires an anonymous table-row box ("anon-tr") parent, and finally an anonymous table box to wrap that. There's two kinds of boxes you can end up with: table and inline-table ("anon-it"), determined by the parent of "Home". In this case, "Home"'s parent is an &lt;a&gt;, which is by default an inline box. So you should end up with something like this:

&lt;li&gt;
  &lt;a&gt;
 ╔═══════anon-it═══════╗
 ║ ┌─────anon-tr─────┐ ║
 ║ │ &lt;div&gt;Home&lt;/div&gt; │ ║
 ║ └─────────────────┘ ║
 ╚═════════════════════╝
  &lt;a&gt;
&lt;/li&gt;


In Gecko, Presto, and IE, this is the (correct) result:

┌────────────────viewport─────────────────┐
│&lt;li&gt;                                     │
│┌─────────────┐                          │
││&lt;a&gt;clkbl area│                          │
││  &lt;div&gt;&lt;/div&gt;│                          │
││&lt;/a&gt;         │                          │
│└─────────────┘                          │
│&lt;li&gt;                                     │
└─────────────────────────────────────────┘

Not very usable. In fact, the bug reporter assumed that it was broken becuase they failed to click exactly on the width of the link text.

WebKit and Blink browsers [have][webkit] [bugs][blink], it turns out, where they generate a table box, rather than an inline-table box so the `<a>` expands to the entire width of its parent `<li>` and is clickable.

┌────────────────viewport─────────────────┐
│&lt;li&gt;                                     │
│┌───────────────────────────────────────┐│
││&lt;a&gt;           clickable area           ││
││  &lt;div&gt;&lt;/div&gt;                          ││
││&lt;/a&gt;                                   ││
│└───────────────────────────────────────┘│
│&lt;li&gt;                                     │
└─────────────────────────────────────────┘

If you find yourself in a similar situation, the simplest fix is to add an explicit `display: block` to the wrapping `<a>` element and it will work as expected in all browsers.

[webkit]: https://bugs.webkit.org/show_bug.cgi?id=125640
[blink]: http://code.google.com/p/chromium/issues/detail?id=327832
[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=928074#c30
[dan]: https://twitter.com/CodingExon
[spec]: http://www.w3.org/TR/CSS21/tables.html#anonymous-boxes
