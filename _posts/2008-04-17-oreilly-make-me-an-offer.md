---
id: 61
title: 'O&#8217;Reilly Make me an Offer!'
date: 2008-04-17T07:49:52+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=61
permalink: /2008/04/17/oreilly-make-me-an-offer/
categories:
  - books
  - code
  - frustration
  - technology
---
_I&#8217;m a big fan of O&#8217;Reilly books, as I&#8217;m sure most of you are._ They&#8217;re great technical resources for me and have cute animals my wife can really enjoy!

A [friend](http://blog.stealingfirst.com/ "stealingfirst.com") of mine got _[Programming Collective Intelligence](http://www.amazon.com/Programming-Collective-Intelligence-Building-Applications/dp/0596529325/ref=pd_bbs_sr_1?ie=UTF8&s=books&qid=1208435009&sr=8-1 "Programming Collective Intelligence")_ and recomended it to me, so my mother-in-law gave it to me for my birthday (yay, I&#8217;m old!). _I&#8217;m stoked to see O&#8217;Reilly focused on moving &#8220;up the stack&#8221; of technology in such an approachable way._

I finally got a chance to start last night and reading the preface it was immediately apparent this was going to challenge my newly developed python skills.

> e.g.
> 
> {xvii} //That&#8217;s the page #
> 
> string_list = [&#8216;a&#8217;, &#8216;b&#8217;, &#8216;c&#8217;, &#8216;d&#8217;]
> 
> string_list[2] # returns <span style="text-decoration: line-through;">&#8216;b&#8217;</span> #wrong it should be &#8216;c&#8217;

_You know when they&#8217;re teaching you incorrect python that it&#8217;s going to be a fun way to learn._ I worked my way up to page 11 lastnight and found about ~8+ errata. This is the first time I&#8217;ve felt completely comfortable marking up a book (oh the sacrilege!) but I do focus better when I can&#8217;t simply skim&#8230;

_I expressed my recent activities on twitter, and another friend asked if I was keeping a list._ So, [FJ](http://digitalanalog.net/ "FJ"), this post&#8217;s for you and for everyone else who doesn&#8217;t want to scratch the same grove in their head that I did.

_O&#8217;Reilly&#8217;s great about leveraging the collective intelligence [pun intended] and you can [_I&#8217;m a big fan of O&#8217;Reilly books, as I&#8217;m sure most of you are._ They&#8217;re great technical resources for me and have cute animals my wife can really enjoy!

A [friend](http://blog.stealingfirst.com/ "stealingfirst.com") of mine got _[Programming Collective Intelligence](http://www.amazon.com/Programming-Collective-Intelligence-Building-Applications/dp/0596529325/ref=pd_bbs_sr_1?ie=UTF8&s=books&qid=1208435009&sr=8-1 "Programming Collective Intelligence")_ and recomended it to me, so my mother-in-law gave it to me for my birthday (yay, I&#8217;m old!). _I&#8217;m stoked to see O&#8217;Reilly focused on moving &#8220;up the stack&#8221; of technology in such an approachable way._

I finally got a chance to start last night and reading the preface it was immediately apparent this was going to challenge my newly developed python skills.

> e.g.
> 
> {xvii} //That&#8217;s the page #
> 
> string_list = [&#8216;a&#8217;, &#8216;b&#8217;, &#8216;c&#8217;, &#8216;d&#8217;]
> 
> string_list[2] # returns <span style="text-decoration: line-through;">&#8216;b&#8217;</span> #wrong it should be &#8216;c&#8217;

_You know when they&#8217;re teaching you incorrect python that it&#8217;s going to be a fun way to learn._ I worked my way up to page 11 lastnight and found about ~8+ errata. This is the first time I&#8217;ve felt completely comfortable marking up a book (oh the sacrilege!) but I do focus better when I can&#8217;t simply skim&#8230;

_I expressed my recent activities on twitter, and another friend asked if I was keeping a list._ So, [FJ](http://digitalanalog.net/ "FJ"), this post&#8217;s for you and for everyone else who doesn&#8217;t want to scratch the same grove in their head that I did.

_O&#8217;Reilly&#8217;s great about leveraging the collective intelligence [pun intended] and you can](http://www.oreilly.com/catalog/9780596529321/errata/ "PCI Errata") (perhaps I should order by frequency and say &#8220;Find and Submit&#8221;) a O&#8217;Reilly&#8217;s [website for the book](http://www.oreilly.com/catalog/9780596529321/ "PCI")._

Unfortunately, the official list only has two and hasn&#8217;t been updated since the 18th of Feb!!!

I submitted mine there and there&#8217;s a ton more (but the user format is a little hard to scroll through).

_So here&#8217;s my quick list till now (p11) [I&#8217;ll try to add new ones as comments so you can track this post]_ and if anyone from O&#8217;Reilly&#8217;s reading I think I&#8217;d make a great editor, if only to actually update the official list with the good community feedback and help others out!

{xvii} string_list[2] = &#8216;c&#8217;

{xviii} /\* first list compression should change v1>4 to v>4 \*/

{xix} // Chapter 2, 2nd to last line &#8220;move&#8221; should be &#8220;movie&#8221;

{9} critics[&#8216;Toby&#8217;] #output is missing &#8216;Superman Returns&#8217;: 4.0

{10} //The results of both math functions are wrong as they use the wrong datapoints (5,4) & (4,1) which should be (1,4.5) and (2,4)

{11} //sim\_distance() &#8211; the return function should be; return 1/(1+sqrt(sum\_of_squares))

{11} from recommendations import critics, sim_distance #reload(recommendations) didn&#8217;t work for me. You&#8217;ll have to change the subsequent function call as well and because of the previous errata the returned # should be 0.2942 (approximately) and not 0.1481

{11} This wasn&#8217;t my find, I learned it from the user submitted errata, but someone mentioned using &#8220;si = set()&#8221; and then &#8220;si.add(item)&#8221; instead of &#8220;si[item]=1&#8221; &#8230; Both make sense, but the set seems cleaner and was a new semantic for me.