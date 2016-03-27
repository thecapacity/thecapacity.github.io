---
id: 38
title: Progress in the New Year
date: 2008-01-18T12:11:55+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/2008/01/18/progress-in-the-new-year/
permalink: /2008/01/18/progress-in-the-new-year/
categories:
  - code
  - opensource
  - python
---
Welcome to the New Year! A lot has happened already, between Google changing their page rank, Apple&#8217;s numerous announcements and tons of other cool stuff. _I&#8217;ve been getting back into the swing of things and mostly trying to figure out how to start off the new year right_, i.e. if there was some great &#8220;first post&#8221; for the year I could conceive.

_As can be the case, my desire to make an amazing impression has resulted in me making none, until now._

I graduated from college with a B.S. in Computer Engineering and for anyone not familiar with a Comp.E. degree _I tell people I&#8217;m a cross between an Electrical Engineer and a Computer Scientist._

Truly, I&#8217;m more of a Scientist then an Engineer and probably should have gone the C.S. route based on my job history through the years. However, _I like being able to call myself an Engineer and it fits with who I am and how I approach problems._

_I tend to act as a &#8220;translation layer&#8221; and like to approach problems with tangible exploration._ I&#8217;m more of a system programmer (i.e. C and perl) then a &#8220;pomp and circumstance&#8221; application architect (e.g. java and relational DB&#8217;s).

_However, as the &#8220;active complexity&#8221; (or perceived complexity) of development and IT moves &#8220;up the stack&#8221; I&#8217;ve felt my programming skills start to slip. It&#8217;s not theory and design failing but that the barrier of taking ideas to fruition has becomes steeper._

Intending to rectify that situation, and loving the compartmentalized development approach that web services brings, _I&#8217;ve begun learning python and intend to practice the application and not just theory of programming._

My first &#8220;new year need&#8221; has been to &#8220;spring clean&#8221; my data files, I have around 20,000 pictures with many dups (the result of recovering a HD). _So coupling intent with need I&#8217;ve written my first real python program._

It&#8217;s not fancy, and only serves to break out a large directory of files into a smaller (3rds) subset. However, it was a great exercise for me so far. _Transitioning to Python isn&#8217;t really a syntactical problem but an exercise in realizing that things which were once hard are now easy!_

_It&#8217;s an exercise in mind-shift that we should all try to initiate this year, try it early and start your year out right!_

_So in case it helps anyone here&#8217;s my &#8220;code&#8221;. I&#8217;m sure there are some more &#8220;python-esque&#8221; ways to do it so if you&#8217;ve got any insights please leave a comment!_

> #!/usr/bin/python
> 
> from glob import glob
  
> from os import stat,mkdir
  
> from shutil import move
> 
> mkdir(&#8220;1&#8221;)
  
> mkdir(&#8220;2&#8221;)
  
> mkdir(&#8220;3&#8221;)
> 
> def size_cmp(x, y):
  
> if stat(x).st\_size > stat(y).st\_size:
  
> return 1
  
> elif stat(x).st\_size == stat(y).st\_size:
  
> return 0
  
> else: # x<y
  
> return -1
> 
> files = glob(&#8220;*.JPG&#8221;)
  
> files.sort(size_cmp)
  
> num_files = len(files)
> 
> for f in files[0: num_files/3]: #slice 1
  
> move(f, &#8220;1&#8221;)
  
> for f in files[num\_files/3+1 : num\_files*2/3]: #slice 2
  
> move(f, &#8220;2&#8221;)
  
> for f in files[num\_files*2/3+1 : num\_files]: #slice 3
  
> move(f, &#8220;3&#8221;)