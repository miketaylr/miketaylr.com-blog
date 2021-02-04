---
layout: post
title:  The Mike Taylor method™ of naming git branches
date:   2021-01-22
---

I thought I would document how I do branching in git because it's clearly <s>the best</s> a perfectly acceptable way to do it, especially if you use GitHub.

Step 1: `git checkout -b <bug-number>/<rev>`

(That's it.)

`<bug-number>` maps to the GitHub (or Bugzilla, or Chromium, or Roblox customer support, etc.) issue number.

`<rev>` is an integer, and maybe 99% of the time it's just `1` for me. But if, for whatever reason, you want to get wild and pivot off into a totally different direction, or set a new [`<start-point>`][docs], you just increment that integer and still have some kind of transparent relationship with the bug task at hand.

For example:

`git checkout -b parsing_html_with_regex_in_the_year_2021/2`

(edit: It's been out to me that this example does not follow my own advice because there's no `<bug-number>`, but instead `<mike_trying_to_make_a_joke>`. Oops.)

As a side benefit, when you go to write a commit message you don't have to go hunting for the bug number in your N² + 1 open tabs (where N is the number of hours you've been working on the patches), because it's in the branch name.

<img alt="screenshot of a git commit prompt which also shows the branch name by default" src="https://miketaylr.com/posts/assets/git-branch.png" style="border:1px solid #ccc">

(For no good reason, for GitHub issues I like to prefix it with `issues/`, I guess I just like the symmetry with the GitHub URL.)

I started doing this about 10 years ago when I worked at Opera. I don't know if it was a widely used convention, or I just copied it off someone, but it's pretty good, IMHO.

[docs]: https://git-scm.com/docs/git-branch

