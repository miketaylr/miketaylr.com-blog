---
layout: post
title:  A label editor for webcompat.com issues
date:   2014-08-04
---

Today we finally deployed the ability to edit labels on issues at webcompat, for logged in users.

Here's what it looks like:

<img src="https://miketaylr.com/posts/assets/1.png" style="border: 1px solid #ccc" alt="label editor cog">

Click or tap on the cog to open the editor:

<img src="https://miketaylr.com/posts/assets/2.png" style="border: 1px solid #ccc" alt="open label editor">

It works basically the same way that GitHub's issue label editor-widget-ui-thingy does, with one important difference: on GitHub, only repo-owners or collaborators can set labels on an issue. Through the power of programming, gritty determination, and a pull ourselves up by our own bootstraps attitude [we work around this][api] and let any auth'd user add or remove labels.

So if you've wanted to help out on webcompat.com but aren't quite sure how to get started, adding labels like `mobile`, `firefox`, `needsinfo`, etc. to issues is a great way to help with triage.

Here's an issue with no labels to get you started: [http://webcompat.com/issues/253][253]

As usual, please report any issues at [GitHub][issues].

[api]: https://github.com/webcompat/webcompat.com/blob/748a8a401675986dfa415937d1d9577917943cc9/webcompat/api/endpoints.py#L107-L118
[issues]: https://github.com/webcompat/webcompat.com/issues/
[253]: http://webcompat.com/issues/253