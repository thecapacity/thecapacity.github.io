---
id: 43
title: 'Even the real geeks at google can&#8217;t get it right all the time&#8230;'
date: 2008-02-10T17:28:52+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/2008/02/10/even-the-real-geeks-at-google-can-get-it-right/
permalink: /2008/02/10/even-the-real-geeks-at-google-can-get-it-right/
categories:
  - code
  - frustration
  - technology
---
Having a &#8220;systems programming&#8221; background I&#8217;m a little green to this web programming stuff. It&#8217;s not that it&#8217;s incomprehensible, I&#8217;ve designed for these applications but I want to ensure I can implement some of the software components as well.

I&#8217;m beginning to really appreciate it, in part because the frameworks are so polished that I don&#8217;t have to worry about drawing boxes and attaching buttons just to make a workable interface. I&#8217;m also a fan of REST and appreciate the human readability, however XML is not &#8220;imparseable&#8221;, it just wraps horribly in a console window.

_As you can tell from [previous posts](http://blog.thecapacity.org/category/code/ "Learning Python"), I&#8217;ve been practicing [Python](http://python.org/ "python") and I&#8217;ve begun exploring [RSS](http://en.wikipedia.org/wiki/RSS_(file_format) "rss") as a communication mechanism and attempting to use [feedparser](http://www.feedparser.org/ "feedparser.org") for working with that data._ A feed library is crucial because of the trickiness of XML, the multiple versions of RSS and the intermix with [Atom](http://en.wikipedia.org/wiki/Atom_%28standard%29 "atom").

_Yet, I seem to be thwarted by my expected allies in this brave new world, [Google](http://www.google.com/ "Google")._

Of all parties, as evidenced by [the plethora of their API](http://googleblog.blogspot.com/ "google blog")&#8216;s, you&#8217;d expect Google to be the most rigorous in ensuring the ease of integration of their products.

Yet, even [with a set of well baked set of enhancements to Google Reader](http://googleblog.blogspot.com/2006/03/google-reader-learns-to-share.html "google reader shares") it appears that [even Google isn&#8217;t perfect](http://feedvalidator.org/check.cgi?url=http%3A%2F%2Fwww.google.com%2Freader%2Fshared%2F14480565058256660224 "google RSS doesn't validate").

_In short; I&#8217;m trying to use a well known feed library for [a language favored by Google](http://www.sauria.com/~twl/conferences/pycon2005/20050325/Python%20at%20Google.html "python at google"), to read [a popular blogger&#8217;s shared feed](http://scobleizer.com/2007/12/25/merry-christmas-2007/ "Scoble likes google reader"), in well known standard from a popular Google RSS service_

_
  
[Yet, it appears to not be valid syntax](http://feedvalidator.org/check.cgi?url=http%3A%2F%2Fwww.google.com%2Freader%2Fshared%2F14480565058256660224 "feedvalidator for google reader")&#8230; how can us mere mortals hope to survive?
  
_