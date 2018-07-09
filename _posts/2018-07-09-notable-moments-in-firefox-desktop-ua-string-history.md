---
layout: post
title: Notable moments in Firefox desktop pre-release UA string history
date: 2018-07-09
---

I'm sure everyone remembers this [super great blog post from 2010 about changes in the Firefox 4 user agent string][uablog]. In terms of "blog posts about UA string changes", it's, well, one of them.

The final post-v4 release desktop UA string is as follows:

Mozilla/5.0 (&lt;platform>; rv:&lt;geckoversion>) Gecko/20100101 Firefox/&lt;firefoxversion>

As sort of a companion piece (that nobody asked for), I wrote up the following table of minor changes to the pre-release desktop UA string between then and now. 

(You know, in case you or I find ourselves needing to know them one day&mdash;apologies in advance for whatever crappy future scenario we ended up in where we need to re-visit this blog post.)

<style>table, th, td {
   border: 1px solid black;
   padding: 5px;
}table {padding: 1px;}</style>
<table>
<tr>
  <th>Version</th>
  <th>Sample Windows 10 pre-release UA string</th>
</tr>
<tr>
  <td>5</td>
  <td>Mozilla/5.0 (Windows NT 6.2; rv:2.2a1pre) Gecko/20110412 Firefox/4.2a1pre</td>
</tr>
<tr>
  <td>6</td>
  <td>Mozilla/5.0 (Windows NT 6.2; rv:6.0a1) Gecko/20110524 Firefox/6.0a1</td>
</tr>
<tr>
  <td>13</td>
  <td>Mozilla/5.0 (Windows NT 6.2; rv:13.0<sup><a href="#fn1">1</a></sup>) Gecko/20120313 Firefox/13.0a1</td>
</tr>
<tr>
  <td>15</td>
  <td>Mozilla/5.0 (Windows NT 6.2; rv:16.0) Gecko/16.0<sup><a href="#fn2">2</a></sup> Firefox/16.0a1</td>
</tr>
<tr>
  <td>16</td>
  <td>Mozilla/5.0 (Windows NT 6.2; rv:16.0) Gecko/16.0 Firefox/16.0<sup><a href="#fn3">3</a></sup></td>
</tr>
<tr>
  <td>20</td>
  <td>Mozilla/5.0 (Windows NT 6.2; rv:20.0) Gecko/20130107<sup><a href="#fn4">4</a></sup> Firefox/20.0
</td>
</tr>
<tr>
  <td>25</td>
  <td>Mozilla/5.0 (Windows NT 6.3; rv:25.0) Gecko/20100101<sup><a href="#fn5">5</a></sup> Firefox/25.0</td>
</tr>
</table>


Footnotes:
<sup id="fn1">1. In [572659][] we dropped pre-release indicators and patch level version numbers from <i>geckoversion</i></sup>
<sup id="fn2">2. In [588909][] we replaced Gecko/&lt;<i>builddate</i>&gt; with Gecko/&lt;<i>geckoversion</i>&gt;</sup>
<sup id="fn3">3. In [572659][] we dropped pre-release indicators and patch level version numbers from <i>firefoxversion</i></sup>
<sup id="fn4">4. In [815743][] we reverted the changes from 588909 (because it broke too many sites, mostly banks)</sup>
<sup id="fn5">5. In [728773][] we began using the frozen 20100101<sup><a href="#afn-1">1</a></sup> builddate for all releases
</sup>

Apocryphal Footnotes to the Footnotes:
<sup id="afn-1">1. It's widely believed Jan 1, 2010 was chosen as the frozen build date because it marked the day that [North Korea pledged lasting peace and a nuclear free Korean peninsula][nk]. That only happens once in a lifetime. Probably.</sup>

[uablog]: https://hacks.mozilla.org/2010/09/final-user-agent-string-for-firefox-4/
[572659]: https://bugzilla.mozilla.org/show_bug.cgi?id=572659
[588909]: https://bugzilla.mozilla.org/show_bug.cgi?id=588909
[572659]: https://bugzilla.mozilla.org/show_bug.cgi?id=572659
[815743]: https://bugzilla.mozilla.org/show_bug.cgi?id=815743
[728773]: https://bugzilla.mozilla.org/show_bug.cgi?id=728773
[nk]: http://edition.cnn.com/2010/WORLD/asiapcf/01/01/nkorea.nuclear/
