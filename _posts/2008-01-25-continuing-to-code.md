---
id: 39
title: Continuing to Code
date: 2008-01-25T17:37:51+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/2008/01/25/continuing-to-code/
permalink: /2008/01/25/continuing-to-code/
categories:
  - code
  - opensource
  - python
---
_Well, my python’s not exactly getting prettier but I’ve been able to make it more functional, may I present “find\_image\_dups.py”!_

Although I’m learning this 4G language (is it truly?) _I still tend towards a shell scripting approach so I write small bits of code_ rather then trying to write one script that will both subdivide my pictures as well as find duplicates.

You can see I’ve experimented with different ways to determine if files are “equal”. If they’re the exact same file then the MD5’s would match and there are plenty of tools for that, but _I anticipate some instances where a few pixels may be different, so checking a thumbnail may be a more repeatable test of identify._

Although my first script allows me to circumvent the constraints imposed by working with too many files, I’m starting to feel frustrated that the limits aren’t easier to “ignore”. I don’t want to waste my time tuning linux limits or tweaking python, I want it to just work in the most simplistic (even if it’s brute force) manner possible!

_One last comment on a language deficiency I find constraining._ Mike and I were discussing the matter and his view, is that it’s not a problem. However, the solution is more code, which is sometimes a silly metric as both of pieces of code would operate equally “efficiently” _I believe my “style” to be more readable (although it doesn’t work so Mike is clearly the victor in the argument)._

Let’s assume for the purposes of the illustration that you were an old C programmer and didn’t use IDE’s all that much, nor want to learn the python debugger yet. So _during your while loop you may be tempted to do;_

> `print "counter: %s p: %s i:%s" % (counter, p.filename, i.filename)`

_Unfortunately, the first time through `p` is `"None"` and you’ll have problems for this single corner case. In C, when printing pointer references, I would use the `?:` tertiary operator which led me to try this in python;_

> `print "counter: %s p: %s i:%s" % (counter, p ? p.filename : "None", i.filename)`

_I think it’s pretty straightforward to understand what’s going on there and what you’d like done, but unfortunately python’s ternary is_

> [`op1 if condition else op2`](http://en.wikipedia.org/wiki/Ternary_operation "Ternary Operator")

_Thus, in python I feel like I should be able to do;_

> `print "counter: %s p: %s i:%s" % (counter, p.filename if p else "None", i.filename)`

_But that doesn’t work!_
  
_Mike’s solution is;_

>  ``_Well, my python’s not exactly getting prettier but I’ve been able to make it more functional, may I present “find\_image\_dups.py”!_

Although I’m learning this 4G language (is it truly?) _I still tend towards a shell scripting approach so I write small bits of code_ rather then trying to write one script that will both subdivide my pictures as well as find duplicates.

You can see I’ve experimented with different ways to determine if files are “equal”. If they’re the exact same file then the MD5’s would match and there are plenty of tools for that, but _I anticipate some instances where a few pixels may be different, so checking a thumbnail may be a more repeatable test of identify._

Although my first script allows me to circumvent the constraints imposed by working with too many files, I’m starting to feel frustrated that the limits aren’t easier to “ignore”. I don’t want to waste my time tuning linux limits or tweaking python, I want it to just work in the most simplistic (even if it’s brute force) manner possible!

_One last comment on a language deficiency I find constraining._ Mike and I were discussing the matter and his view, is that it’s not a problem. However, the solution is more code, which is sometimes a silly metric as both of pieces of code would operate equally “efficiently” _I believe my “style” to be more readable (although it doesn’t work so Mike is clearly the victor in the argument)._

Let’s assume for the purposes of the illustration that you were an old C programmer and didn’t use IDE’s all that much, nor want to learn the python debugger yet. So _during your while loop you may be tempted to do;_

> `print "counter: %s p: %s i:%s" % (counter, p.filename, i.filename)`

_Unfortunately, the first time through `p` is `"None"` and you’ll have problems for this single corner case. In C, when printing pointer references, I would use the `?:` tertiary operator which led me to try this in python;_

> `print "counter: %s p: %s i:%s" % (counter, p ? p.filename : "None", i.filename)`

_I think it’s pretty straightforward to understand what’s going on there and what you’d like done, but unfortunately python’s ternary is_

> [`op1 if condition else op2`](http://en.wikipedia.org/wiki/Ternary_operation "Ternary Operator")

_Thus, in python I feel like I should be able to do;_

> `print "counter: %s p: %s i:%s" % (counter, p.filename if p else "None", i.filename)`

_But that doesn’t work!_
  
_Mike’s solution is;_

>`` 

_While clearly functional but I dislike the added conditionals since, to me, they don’t feel like the main intent of the code. However, Mike expressed a good point; If the code were permanent, the ternary operator might cause to someone examining the printouts and sees two possible outputs from a single statement._

_Nevertheless it’s great learning and discussing some of the finer points of style in a language I’m only newly familiar with!_ 

> `<br />
#!/usr/bin/python`
> 
> import Image
  
> import ImageStat
> 
> from glob import glob
  
> from os import mkdir
  
> from shutil import move
> 
> def RMS_cmp(x, y):
  
> \# if ImageStat.Stat(x).rms == ImageStat.Stat(y).rms:
  
> \# if x.copy().resize((300,300)).histogram() == y.copy().resize((300,300)).histogram():
  
> if x.histogram() == y.histogram():
  
> return 0
  
> elif ImageStat.Stat(x).rms > ImageStat.Stat(y).rms:
  
> return 1
  
> else: # x<y
  
> return -1
> 
> images = map(Image.open, glob(“*.\[Jj\]\[Pp\][Gg]”))
  
> images.sort(RMS_cmp)
> 
> counter = 1
  
> p = None
  
> for i in images:
  
> if p and ( RMS_cmp(p, i) == 0 ):
  
> move(i.filename, str(counter))
  
> else: #no match start a new directory
  
> if p:
  
> counter += 1
  
> try:
  
> mkdir(str(counter))
  
> except:
  
> pass
  
> move(i.filename, str(counter))
  
> p = i

