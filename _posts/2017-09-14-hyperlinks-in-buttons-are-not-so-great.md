---
layout: post
title: hyperlinks in buttons are probably not a great idea
date: 2017-09-14
---

Over in [web-bug #9726][webbug], there's an interesting issue reported against [glitch.com][glitch] (which is already fixed because those peeps are classy):

Basically, they had an HTML `<button>` that when clicked would `display:block` a descendent `<dialog>` element that contained some hyperlinks to help you create a new project.

<img src="http://miketaylr.com/posts/assets/glitch.png" style="border: 1px solid #ccc;" alt="screenshot of glitch.com button">

The [simplest test case][demo]:

```html
<button>
  <a href="https://example.com">do cool thing</a>
</button>
```

Problem is, clicking on an anchor with an `href` inside of a button does nothing in Firefox (and Opera Presto, which only 90s kids remember).

What the frig, web browsers.

But it turns out [HTML is explicit on the subject][html], as it often is, stating that a button's content model must not have an [interactive content descendant][int].

(and `<a href>` is totally, like, interactive content, itsho\*)

Soooo, probably not a good idea to follow this pattern. And who knows what it means for accessibility.

The [fix][bugfix] for glitch is simple: just make the `<dialog>` a sibling, and hide and show it the same way.

\* in the spec's humble opinion

[webbug]: https://github.com/webcompat/web-bugs/issues/9726
[bugfix]: https://github.com/jennschiffer/glitch-community-backup/commit/7dacbd655f959696cc92d2551fe5f726777f6765#diff-97f07c36526b064b52160867e14df866R6
[glitch]: https://glitch.com
[html]: https://html.spec.whatwg.org/multipage/form-elements.html#the-button-element
[int]: https://html.spec.whatwg.org/multipage/dom.html#interactive-content-2
[demo]: https://miketaylr.com/bzla/button-link.html