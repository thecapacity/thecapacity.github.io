---
id: 73
title: 'Quick mean python bug…'
date: 2008-07-04T10:59:43+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=73
permalink: /2008/07/04/quick-mean-python-bug/
categories:
  - python
---
So I’ve been learning some python and love it! It’s highly functional and most of the libraries are “good enough”.

This morning I recently discovered a bug I introduced when testing… not a bug in the class, but a bug in the way Python works against expectations (even if they are just my own).

Let’s say I have a list of stock symbols;

> stocks = (‘GOOG’, ‘IBM’, ‘JDSU’, ‘HPQ’, ‘GE’, ‘MSFT’, ‘BUD’, ‘DIS’)

It’s quite a collection I know and ideally I want to do some complicated operations on each;

> for sym in stocks:

If we call things the way they are now;

> >>> stocks = (‘GOOG’, ‘IBM’, ‘JDSU’, ‘HPQ’, ‘GE’, ‘MSFT’, ‘BUD’, ‘DIS’)
  
> >>> for sym in stocks:
  
> …     print sym
  
> …
  
> GOOG
  
> IBM
  
> JDSU
  
> HPQ
  
> GE
  
> MSFT
  
> BUD
  
> DIS
  
> >>>

That code’s working so far, so let’s go on to debug our function; do\_stock\_stuff()

It’s wasteful to worry about looping through all the stocks so we’ll simply make a single pass;

> >>> stocks = (“GOOG”)
  
> >>> for sym in stocks:
  
> …     do\_stock\_stuff(sym)

**So do you see the bug?**

I didn’t and my “do\_stock\_stuff” procedure actually worked. However it got called 4 times… with “G”, “O”, “O”, “G”.

**The counter intuitive fix?**

> >>> stocks = (“GOOG”**,**)
  
> >>> for sym in stocks:
  
> …     do\_stock\_stuff(sym)

Can you tell the difference? **It’s simply adding a ‘,’ at the end!**

This ocurred because I’m using a “touple” type when I should just use a list;

> >>> stocks = [“GOOG”]
  
> >>> for s in stocks:
  
> …     print s
  
> …
  
> GOOG

This ability to test expectations interactively is a great aspect of programming python. But for anyone who says it matches exactly how they think is thinking about the wrong things i.e. syntax over function (don’t get me started on the ‘:’).
