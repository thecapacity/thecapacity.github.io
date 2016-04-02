---
id: 61
title: 'O’Reilly Make me an Offer!'
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
_I’m a big fan of O’Reilly books, as I’m sure most of you are._ They’re great technical resources for me and have cute animals my wife can really enjoy!

A [friend](http://blog.stealingfirst.com/ "stealingfirst.com") of mine got _[Programming Collective Intelligence](http://www.amazon.com/Programming-Collective-Intelligence-Building-Applications/dp/0596529325/ref=pd_bbs_sr_1?ie=UTF8&s=books&qid=1208435009&sr=8-1 "Programming Collective Intelligence")_ and recomended it to me, so my mother-in-law gave it to me for my birthday (yay, I’m old!). _I’m stoked to see O’Reilly focused on moving “up the stack” of technology in such an approachable way._

I finally got a chance to start last night and reading the preface it was immediately apparent this was going to challenge my newly developed python skills.

> e.g.
> 
> {xvii} //That’s the page #
> 
> string_list = [‘a’, ‘b’, ‘c’, ‘d’]
> 
> string_list[2] # returns <span style="text-decoration: line-through;">‘b’</span> #wrong it should be ‘c’

_You know when they’re teaching you incorrect python that it’s going to be a fun way to learn._ I worked my way up to page 11 lastnight and found about ~8+ errata. This is the first time I’ve felt completely comfortable marking up a book (oh the sacrilege!) but I do focus better when I can’t simply skim…

_I expressed my recent activities on twitter, and another friend asked if I was keeping a list._ So, [FJ](http://digitalanalog.net/ "FJ"), this post’s for you and for everyone else who doesn’t want to scratch the same grove in their head that I did.

_O’Reilly’s great about leveraging the collective intelligence [pun intended] and you can [_I’m a big fan of O’Reilly books, as I’m sure most of you are._ They’re great technical resources for me and have cute animals my wife can really enjoy!

A [friend](http://blog.stealingfirst.com/ "stealingfirst.com") of mine got _[Programming Collective Intelligence](http://www.amazon.com/Programming-Collective-Intelligence-Building-Applications/dp/0596529325/ref=pd_bbs_sr_1?ie=UTF8&s=books&qid=1208435009&sr=8-1 "Programming Collective Intelligence")_ and recomended it to me, so my mother-in-law gave it to me for my birthday (yay, I’m old!). _I’m stoked to see O’Reilly focused on moving “up the stack” of technology in such an approachable way._

I finally got a chance to start last night and reading the preface it was immediately apparent this was going to challenge my newly developed python skills.

> e.g.
> 
> {xvii} //That’s the page #
> 
> string_list = [‘a’, ‘b’, ‘c’, ‘d’]
> 
> string_list[2] # returns <span style="text-decoration: line-through;">‘b’</span> #wrong it should be ‘c’

_You know when they’re teaching you incorrect python that it’s going to be a fun way to learn._ I worked my way up to page 11 lastnight and found about ~8+ errata. This is the first time I’ve felt completely comfortable marking up a book (oh the sacrilege!) but I do focus better when I can’t simply skim…

_I expressed my recent activities on twitter, and another friend asked if I was keeping a list._ So, [FJ](http://digitalanalog.net/ "FJ"), this post’s for you and for everyone else who doesn’t want to scratch the same grove in their head that I did.

_O’Reilly’s great about leveraging the collective intelligence [pun intended] and you can](http://www.oreilly.com/catalog/9780596529321/errata/ "PCI Errata") (perhaps I should order by frequency and say “Find and Submit”) a O’Reilly’s [website for the book](http://www.oreilly.com/catalog/9780596529321/ "PCI")._

Unfortunately, the official list only has two and hasn’t been updated since the 18th of Feb!!!

I submitted mine there and there’s a ton more (but the user format is a little hard to scroll through).

_So here’s my quick list till now (p11) [I’ll try to add new ones as comments so you can track this post]_ and if anyone from O’Reilly’s reading I think I’d make a great editor, if only to actually update the official list with the good community feedback and help others out!

{xvii} string_list[2] = ‘c’

{xviii} /\* first list compression should change v1>4 to v>4 \*/

{xix} // Chapter 2, 2nd to last line “move” should be “movie”

{9} critics[‘Toby’] #output is missing ‘Superman Returns’: 4.0

{10} //The results of both math functions are wrong as they use the wrong datapoints (5,4) & (4,1) which should be (1,4.5) and (2,4)

{11} //sim\_distance() – the return function should be; return 1/(1+sqrt(sum\_of_squares))

{11} from recommendations import critics, sim_distance #reload(recommendations) didn’t work for me. You’ll have to change the subsequent function call as well and because of the previous errata the returned # should be 0.2942 (approximately) and not 0.1481

{11} This wasn’t my find, I learned it from the user submitted errata, but someone mentioned using “si = set()” and then “si.add(item)” instead of “si[item]=1” … Both make sense, but the set seems cleaner and was a new semantic for me.

