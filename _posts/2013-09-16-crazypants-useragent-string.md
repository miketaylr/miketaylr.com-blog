---
layout: post
title:  CrazyPants Useragent String, Paperback Edition.
date:   2013-09-16
---

My colleague [Karl][karl] got ahold of a ton of interesting log data from a big site that I'm sure he'll write about soon enough. He shared this beauty of a useragent string with me today on IRC:

```
CETRIX-CS1280 Linux/3.0.13 Android/4.0.4 Release/↩
\xD9\xA1\xD9\xA2.\xD9\xA1\xD9\xA1.\xD9\xA2\xD9\xA0\xD9\xA1\xD9\xA2 ↩
Browser/AppleWebKit534.30 Profile/MIDP-2.0 ↩
Configuration/CLDC-1.1 Mobile Safari/534.30 Android 4.0.1;
```

I've added `↩` symbols to indicate where I've forced a line break (because I'm a designer and don't want to degrade the experience of this sweet blog theme, obviously).

So like, what is going on here? It looks like unicode in the UA string, which is... novel. If you paste `\xD9\xA1\xD9\xA2.\xD9\xA1\xD9\xA1.\xD9\xA2\xD9\xA0\xD9\xA1\xD9\xA2` into [Hixie's utf-8 decoder][decoder] and select "Hexadecimal" as the input type you'll get the following result:

As character names:

```
U+0661 ARABIC-INDIC DIGIT ONE character (&#x0661;)
U+0662 ARABIC-INDIC DIGIT TWO character (&#x0662;)
U+0661 ARABIC-INDIC DIGIT ONE character (&#x0661;)
U+0661 ARABIC-INDIC DIGIT ONE character (&#x0661;)
U+0662 ARABIC-INDIC DIGIT TWO character (&#x0662;)
U+0660 ARABIC-INDIC DIGIT ZERO character (&#x0660;)
U+0661 ARABIC-INDIC DIGIT ONE character (&#x0661;)
U+0662 ARABIC-INDIC DIGIT TWO character (&#x0662;)
```

As raw characters:

```
١٢١١٢٠١٢
```

OK, at this point you're probably feeling like that guy in the Da Vinci code books (Tom Selleck maybe?), so you go and put that in [Google Translate][gtranslate] (selecting Arabic and English because Roman Numerals doesn't seem to be an option, thanks Google).

...and you get 12112012, which just so happened to be 49 minutes after the Winter solstice ([Illuminati much?][illum]). Or just a version number. In Arabic. In Unicode. The key to that secret is probably figuring out why the UA string contains both `Android/4.0.4` and `Android 4.0.1`. Current scholarship suggests that the 49 minute difference is derived from the 7 characters in the word "Android" times 7 (hence it being doubled up) and, well you get the picture.

[gtranslate]: http://translate.google.com/#ar/en/%D9%A1%D9%A2%D9%A1%D9%A1%D9%A2%D9%A0%D9%A1%D9%A2
[decoder]: http://software.hixie.ch/utilities/cgi/unicode-decoder/utf8-decoder
[illum]: http://tmgnow.blogspot.com/2008/03/december-11th-1111-pm-2012-pack-your.html
[karl]: http://www.la-grange.net/karl/