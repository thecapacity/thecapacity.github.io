---
id: 197
title: Open heart surgery!
date: 2008-12-17T10:26:44+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=197
permalink: /2008/12/17/open-heart-surgery/
categories:
  - uncategorized
---
It&#8217;s not quite a detailed tutorial but I hope any budding ioBridge cardiologists will find [my latest foray](http://www.vimeo.com/2553791) into web2.0 useful.

I&#8217;ve recorded a video of [my ioBridge project](http://blog.thecapacity.org/2008/12/15/bridge-to-my-heart/) and posted it on [Vimeo](http://www.vimeo.com/) in the hope that it will help anyone looking to do something similar.

Unfortunately, pasting HTML into a blog post usually isn&#8217;t successful but I believe [my source](http://www.wjhuie.com/heart.html) and previous writeup should be simple enough for others to follow. The trick is to include your widget script in your page and wrap it with a &#8220;div&#8221; using a specific, ID. In my case I used &#8220;#content&#8221; and was then able to use jQuery to mark this as hidden, CSS &#8220;display: hidden&#8221; would also work equally well.

I could then use jQuery to parse on this div ID (rather then having to find out the ioBridge widget number), which isn&#8217;t hard either and convert the html content to a floating point value. It&#8217;s a straightforward query though you may have to play with the &#8220;split()&#8221; function depending on your layout and sensor.

If you&#8217;ve got any questions feel free to email me, jay, at thecapacity.org or find me via http://wjhuie.com