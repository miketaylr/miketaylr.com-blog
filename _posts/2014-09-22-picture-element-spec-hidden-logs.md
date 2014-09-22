---
layout: post
title:  How Wilto got his &lt;picture&gt; element spec writing wings&#58; the hidden logs
date:   2014-09-22
---


Mat Marquis wandered into our IRC channel a few months before, talking responsive images and motorcyles and Mega Man mostly.
At the same time, Bruce Lawson was advocating for something like &lt;picture&gt; with the standards people at Opera ([he wrote about that here][bruce]). The following logs took place on December 6th, 2011.

```
<miketaylr> btw, Wilto: http://miketaylr.com/post/b3c55cf2.png
<miketaylr> chatting with annevk
```

I think the screenshot I pasted was from an Opera internal IRC network called #techno&mdash;where the standards people hung out. I don't remember though. (sup lawyers)

Anne was linking to a demo that Scott Jehl and Mat made that used &lt;canvas&gt;. It's dead today but there's plenty of links on the web describing it.
Mat filed this bug for Firefox: [https://bugzilla.mozilla.org/show_bug.cgi?id=707806][bug] as a result of the demo.

```
<Wilto> miketaylr: I can't get past the no-JS issue.
<Wilto> I'm not comfortable saying that it's the way forward.
<Wilto> On paper, man, it's gorgeous.
<miketaylr> he's just talking about the general push for responsive images
<miketaylr> not this technique
<Wilto> Oh, oh.
<Wilto> miketaylr: Two questions.
<Wilto> miketaylr: 1) Who's this Anne dude?
<vladikoff> lol
<Wilto> 2) Yes yes yes more visibility to responsive images issues yes let's make that happen.
<miketaylr> re 1) anne van kesteren, standards guy, editor of CORS, XHR, FullScreen API, 18 million other things, zelda nerd
<Wilto> Oh ho.
<Wilto> A good dude to have on our side, you're saying.
<miketaylr> he's a good guy for sure
<Wilto> miketaylr: What does he mean by "standardize on it?"
<miketaylr> Wilto: write a spec, get browsers to implement it
<Wilto> Thing is, canvas is PERFECT.
<Wilto> If it followed the spec everywhere.
<miketaylr> well, it's a perfect *hack*
<Wilto> I dunno.
<miketaylr> canvas is not intended to be a generic <img> container
<miketaylr> even though it can do that
<Wilto> No, but it's made for the display of dymanic content, y'know?
<matjas> TIL "dymanic" is a thing.
```

Mathias Bynens, IRC wise guy, before he was at Opera.

```
<Wilto> I, matjas, am dymanic.
<Wilto> The small vs. big image logic would all be be within the canvassy goodness. The img would just be a reliable fallback, as designed.
<miketaylr> i guss that passes muster
<miketaylr> guess even
<Wilto> YOU DYMANIC NOW DAWG
<Wilto> I mean… It's like, that or a new element.
<miketaylr> or better media queries and network control
<Wilto> Should I write a spec? I will write the fresh hell out of a spec.
<miketaylr> or something
<Wilto> Should I get in touch with this dude?
<miketaylr> Wilto: sure, he's parked in #whatwg
```

6 hours later (in theory we did some actual work that day):

```
<Wilto> -Hey, there's that Anne dude in #WHATWG.
<Wilto> What's my plan of attack, miketaylr!?
<miketaylr> right now he's busy talking xhr with sicking
<Wilto> Yeah, I'm kinda tied up now anyway.
<Wilto> I'll pester people tomorrow.
<miketaylr> so when they're done... say, annevk i want to make responsive images happen
<Wilto> That doesn't seem very attack-y, though.
<miketaylr> or just say wowowowowowowowow
<miketaylr> and drop in that gif (http://i.imgur.com/7XsmM.gif)
<Wilto> I like attacking people, places, and things.
<Wilto> HE SAID THE WORD "RESPONSE" NOW IS MY TIME TO STRIKE
<miketaylr> lol
<Wilto> "SPEAKING of things that respond to things!"
<miketaylr> SPEAKING OF RESPONSE text
<Wilto> I'm smooth like that.
<Wilto> "Oh, hey brah-you dig respondin'? That's cool, that's cool. SAY, HAVE YOU HEARD OF RESPONSIVE WEB DESIGN!?"
<Wilto> ?cc
<bot-t> CASE CLOASED >: |
```

And then two days later the conversation continues in #whatwg. Or maybe it was the next day cause Krijn's servers are in the future. I don't remember.

And then a lot of people did a lot of real work for _years_ to make &lt;picture&gt; happen. I just continued making jokes on IRC instead (I regret nothing).

[http://krijnhoetmer.nl/irc-logs/whatwg/20111208][whatwg]

[bug]: https://bugzilla.mozilla.org/show_bug.cgi?id=707806
[whatwg]: http://krijnhoetmer.nl/irc-logs/whatwg/20111208
[ss]: http://miketaylr.com/post/b3c55cf2.png
[bruce]: http://www.brucelawson.co.uk/2011/notes-on-adaptive-images-yet-again/