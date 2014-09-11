---
layout: post
title:  Closing issues in webcompat.com
date:   2014-09-04
---

In between checking twitter vanity searches and denying LinkedIn connection requests, we've been adding features little by little to [webcompat.com][wc]. One of the most important, but perhaps not-very-exciting ones is the ability to close an issue from [webcompat.com][wc] itself, rather than the GitHub repo.

If you've used GitHub much, you'll know that only people with repo write access can close an issue. This makes sense most of the time, but for our project we want to give collaborators the ability to get to work right away. So we're proxying all API requests to close or re-open an issue as [closerbot][cb], one of the newest members of our bot proxy staff. This means that any logged in user can open or close an issue. (Non-logged in users won't see the open/close buttons).

So next time you're looking at [http://webcompat.com/#browse-issues][browse] and notice that a bug has been fixed (or not), is a dupe or whatever, feel free to close or re-open it. All that we ask is that you be logged in via GitHub OAuth so we know you're legit.

In the future we'd like to add some more fine-grained classifications such as "WONTFIX", "INVALID", etc. If you'd like to help out with that, drop us a line in the [original issue][issue]. I'll even put a LinkedIn connection request bounty on it.


[wc]: http://webcompat.com
[cb]: https://github.com/closerbot
[browse]: http://webcompat.com/#browse-issues
[issue]: https://github.com/webcompat/webcompat.com/issues/196