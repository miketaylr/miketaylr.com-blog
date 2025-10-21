---
layout: post
title:  The origins of the WHATWG Compat Standard logo
date:   2024-09-27
---

Note from the future (Oct 20, 2025): I just opened my editor and found this half-written draft. Might as well publish it.

It's a bit awkward that I haven't blogged in some 19 months so, as owner and author of the best blog about broken websites on the internet. I guess I've been busy, or lacking motivation to write, or perhaps both. Who can really say.

But this morning I was just sitting at a table on the 5th floor of the extremely under construction Hilton Anaheim (location of the 2024 W3C TPAC) telling [Dom Farolino](https://domfarolino.com/) about the origin of the WHATWG Compat Standard logo.

Today, it lives at [https://resources.whatwg.org/logo-compat.svg](https://resources.whatwg.org/logo-compat.svg). If you check out the source, you should see something like:

```css
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
  <rect width="90"
        height="90"
        fill="#fff"
        x="5"
        y="5"
        stroke="#3c790a"
        stroke-width="10"
        style="-webkit-border-radius: 10px"/>
  <path d="(omitted)" fill="#3c790a"/>
</svg>
```

There's clearly a hilarious joke in the inline style. Please clap.

If you dig into the history of the spec, though, you'll notice Karl Dubost's [first stab](https://github.com/whatwg/compat/commit/c0dc9fd3b4fef90f915149cb4e404547606143c0) at the logo as the repo's first commit. Which is certainly <em>one way</em> to begin the commit history of a nascent standard. Thankfully that version [only lasted a day](https://github.com/whatwg/compat/commit/345b2d1ee3a26344e08b49a2345e67cc66063dad), and a little more than a week later that was replaced by the current square logo.

<img src="https://miketaylr.com/posts/assets/compat.png" style="width: 100%">