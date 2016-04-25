---
layout: post
title: String.prototype.contains, use your judgement
date:   2016-04-25
---

I was lurking on the darkweb (stackoverflow) looking for old bugs when I ran into this gem: ["How can I check if one string contains another substring?"][so].

Pretty normal question for people new to programming (like myself), and the [#3 answer][two] contains the following suggestion:

```js
String.prototype.contains = function(it) { return this.indexOf(it) != -1; };
```

Totally does what the person was asking for. Good stuff.

(And as a result, the person who gave the answer is swimming in stackoverflow points&mdash;which is how you buy illegal things on the darkweb.)

The spooky part is back in 2011, in his response to this answer, [zachleat][zl] linked to a classic Zakas post ["Don’t modify objects you don’t own"][z].

From the article,

> Maintainable code is code that you don’t need to modify when the browser changes. You don’t know how browser developers will evolve existing browsers and the rate at which those evolutions will take place.

You might remember that ES6 tried to add `String.prototype.contains`, but it [broke a number of sites (especially those using MooTools because the two implementations had different semantics)][mt] and [had to be renamed][inc] to `String.prototype.includes`.

To the OP's credit, they came back with an edit:

> Note: see the comments below for a valid argument for not using this. My advice: use your own judgement.

The morals to this story are obvious: the darkweb is as scary as they say. And Zach Leatherman might be a witch.



[so]: http://stackoverflow.com/questions/1789945/how-can-i-check-if-one-string-contains-another-substring
[two]: http://stackoverflow.com/a/1978419
[zl]: http://stackoverflow.com/users/16711/zachleat
[z]: https://www.nczonline.net/blog/2010/03/02/maintainable-javascript-dont-modify-objects-you-down-own/
[inc]: https://bugzilla.mozilla.org/show_bug.cgi?id=1102219
[mt]: https://github.com/mootools/mootools-core/issues/2402