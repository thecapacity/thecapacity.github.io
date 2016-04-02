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
Welcome to the New Year! A lot has happened already, between Google changing their page rank, Apple’s numerous announcements and tons of other cool stuff. _I’ve been getting back into the swing of things and mostly trying to figure out how to start off the new year right_, i.e. if there was some great “first post” for the year I could conceive.

_As can be the case, my desire to make an amazing impression has resulted in me making none, until now._

I graduated from college with a B.S. in Computer Engineering and for anyone not familiar with a Comp.E. degree _I tell people I’m a cross between an Electrical Engineer and a Computer Scientist._

Truly, I’m more of a Scientist then an Engineer and probably should have gone the C.S. route based on my job history through the years. However, _I like being able to call myself an Engineer and it fits with who I am and how I approach problems._

_I tend to act as a “translation layer” and like to approach problems with tangible exploration._ I’m more of a system programmer (i.e. C and perl) then a “pomp and circumstance” application architect (e.g. java and relational DB’s).

_However, as the “active complexity” (or perceived complexity) of development and IT moves “up the stack” I’ve felt my programming skills start to slip. It’s not theory and design failing but that the barrier of taking ideas to fruition has becomes steeper._

Intending to rectify that situation, and loving the compartmentalized development approach that web services brings, _I’ve begun learning python and intend to practice the application and not just theory of programming._

My first “new year need” has been to “spring clean” my data files, I have around 20,000 pictures with many dups (the result of recovering a HD). _So coupling intent with need I’ve written my first real python program._

It’s not fancy, and only serves to break out a large directory of files into a smaller (3rds) subset. However, it was a great exercise for me so far. _Transitioning to Python isn’t really a syntactical problem but an exercise in realizing that things which were once hard are now easy!_

_It’s an exercise in mind-shift that we should all try to initiate this year, try it early and start your year out right!_

_So in case it helps anyone here’s my “code”. I’m sure there are some more “python-esque” ways to do it so if you’ve got any insights please leave a comment!_

> #!/usr/bin/python
> 
> from glob import glob
  
> from os import stat,mkdir
  
> from shutil import move
> 
> mkdir(“1”)
  
> mkdir(“2”)
  
> mkdir(“3”)
> 
> def size_cmp(x, y):
  
> if stat(x).st\_size > stat(y).st\_size:
  
> return 1
  
> elif stat(x).st\_size == stat(y).st\_size:
  
> return 0
  
> else: # x<y
  
> return -1
> 
> files = glob(“*.JPG”)
  
> files.sort(size_cmp)
  
> num_files = len(files)
> 
> for f in files[0: num_files/3]: #slice 1
  
> move(f, “1”)
  
> for f in files[num\_files/3+1 : num\_files*2/3]: #slice 2
  
> move(f, “2”)
  
> for f in files[num\_files*2/3+1 : num\_files]: #slice 3
  
> move(f, “3”)

