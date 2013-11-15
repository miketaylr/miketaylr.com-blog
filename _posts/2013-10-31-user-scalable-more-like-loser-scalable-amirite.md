---
layout: post
title:  user-scalable=like, whatever
date:   2013-10-22
---
[estimated reading time =no;]

If you want to prevent zooming on a mobile device, you can use the so-called "viewport meta tag" and the special `user-scalable` property to ruin your user's day.

If you'd like to actually learn how to use it properly, there's some [good][devo] [articles][mdn] out there. But between you and me, dear reader, we both know you came here for the same reason that traffic slows down when there's someone pulled over on the other side of the freeway. That's right, this is web forensics rubbernecking.

The best part about shipping web features without writing specs or bringing them to standards bodies is watching them mutate from some [mildly descriptive documentation][docs] to what can only be described as flying by the seat of one's pants.

Let me show you how people try to disable viewport zooming on the web. Some of these even work.

[`<meta name="viewport" content="user-scalable=no">`][no]
[`<meta name="viewport" content="user-scalable=0">`][0]
[`<meta name="viewport" content="user-scalable=false;">`][false]
[`<meta name="viewport" content="width=device-width, user-scalable=-1;" />`][-1]
[`<meta name="viewport" user-scalable="0;" />`][0;]
[`<meta name="viewport" content="maximum-scale=1.0, user-scalable=2.0;" />`][2.0]
[`<meta name="viewport" content="maximum-scale=3.0, user-scalable=3;"/>`][3]

(Who knows what's going on in the last two examples.)

[no]: https://github.com/blackberry/WebKit-BB10/blob/687325e884d989aae3a0b5b8e97c0ff7f58d1e9d/Source/WebKit/chromium/tests/data/no_scale_for_you.html#L1
[0]: https://github.com/dstockwell/blink/blob/63745d1c0e81655a2e624a08c41116eeb4b614b4/Source/web/tests/data/viewport/viewport-legacy-merge-quirk-2.html#L11
[false]: https://github.com/yoka/ios7css/blob/8ef18b0136e1f2bdf24ad034818e6acaf9657b4a/input_checkbox.html#L4
[0;]: https://github.com/wingamekits/super-paper-monster-smasher-starter-kit/blob/a2870446c0a571de10f94064b0b601da0c92a123/Projects/SuperPaperMonsterSmasherStarterKitWin8/index.html#L7
[2.0]: https://github.com/ryanmichael/yomurph/blob/3c55a83fe8f5935405bded783b460ce60963e8bf/index.html#L10
[3]: https://github.com/finalljx/hp_finalljx/blob/1a2a969f8a5996ecce6e0c2389beba77919e1f21/jsp/file.jsp#L6
[devo]: http://dev.opera.com/articles/view/an-introduction-to-meta-viewport-and-viewport/
[mdn]: https://developer.mozilla.org/en-US/docs/Mozilla/Mobile/Viewport_meta_tag
[docs]: https://developer.apple.com/library/safari/documentation/AppleApplications/Reference/SafariWebContent/UsingtheViewport/UsingtheViewport.html#//apple_ref/doc/uid/TP40006509-SW26
[-1]: https://ww70.itau.com.br/M/Institucional/IncentivoAplicativo.aspx