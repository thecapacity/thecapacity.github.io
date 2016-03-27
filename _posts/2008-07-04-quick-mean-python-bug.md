---
id: 73
title: 'Quick mean python bug&#8230;'
date: 2008-07-04T10:59:43+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=73
permalink: /2008/07/04/quick-mean-python-bug/
categories:
  - python
---
So I&#8217;ve been learning some python and love it! It&#8217;s highly functional and most of the libraries are &#8220;good enough&#8221;.

This morning I recently discovered a bug I introduced when testing&#8230; not a bug in the class, but a bug in the way Python works against expectations (even if they are just my own).

Let&#8217;s say I have a list of stock symbols;

> stocks = (&#8216;GOOG&#8217;, &#8216;IBM&#8217;, &#8216;JDSU&#8217;, &#8216;HPQ&#8217;, &#8216;GE&#8217;, &#8216;MSFT&#8217;, &#8216;BUD&#8217;, &#8216;DIS&#8217;)

It&#8217;s quite a collection I know and ideally I want to do some complicated operations on each;

> for sym in stocks:

If we call things the way they are now;

> >>> stocks = (&#8216;GOOG&#8217;, &#8216;IBM&#8217;, &#8216;JDSU&#8217;, &#8216;HPQ&#8217;, &#8216;GE&#8217;, &#8216;MSFT&#8217;, &#8216;BUD&#8217;, &#8216;DIS&#8217;)
  
> >>> for sym in stocks:
  
> &#8230;     print sym
  
> &#8230;
  
> GOOG
  
> IBM
  
> JDSU
  
> HPQ
  
> GE
  
> MSFT
  
> BUD
  
> DIS
  
> >>>

That code&#8217;s working so far, so let&#8217;s go on to debug our function; do\_stock\_stuff()

It&#8217;s wasteful to worry about looping through all the stocks so we&#8217;ll simply make a single pass;

> >>> stocks = (&#8220;GOOG&#8221;)
  
> >>> for sym in stocks:
  
> &#8230;     do\_stock\_stuff(sym)

**So do you see the bug?**

I didn&#8217;t and my &#8220;do\_stock\_stuff&#8221; procedure actually worked. However it got called 4 times&#8230; with &#8220;G&#8221;, &#8220;O&#8221;, &#8220;O&#8221;, &#8220;G&#8221;.

**The counter intuitive fix?**

> >>> stocks = (&#8220;GOOG&#8221;**,**)
  
> >>> for sym in stocks:
  
> &#8230;     do\_stock\_stuff(sym)

Can you tell the difference? **It&#8217;s simply adding a &#8216;,&#8217; at the end!**

This ocurred because I&#8217;m using a &#8220;touple&#8221; type when I should just use a list;

> >>> stocks = [&#8220;GOOG&#8221;]
  
> >>> for s in stocks:
  
> &#8230;     print s
  
> &#8230;
  
> GOOG

This ability to test expectations interactively is a great aspect of programming python. But for anyone who says it matches exactly how they think is thinking about the wrong things i.e. syntax over function (don&#8217;t get me started on the &#8216;:&#8217;).