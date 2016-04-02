---
id: 224
title: twitterline
date: 2009-02-20T23:07:50+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=224
permalink: /2009/02/20/twitterline/
categories:
  - code
  - python
  - twitter
  - visualization
---
I’ve been using a lot of python, jQuery and web services recently and thought it was time to pull together those skills into a public app.

Like most developers, I often “scratch my own itch” and write code to solve a problem or learn something. I try to [post](http://blog.thecapacity.org/category/code/) what I can but some solutions are hosted internally and I know there are numerous code fragments scattered across my hard drives which haven’t made it into posts.

Many of these projects are “evolutionary dead ends” but I think **it’s important to engage in “purposeful play”** without anticipating success or failure. You really have to take time to nurture your childlike creativity and **it’s often in these limitless exercises that we develop the foundation for real breakthroughs for more “respected” works.**

I was reminded of this recently when I watched a [presentation](http://vimeo.com/3199933) by [Aaron Koblin](http://www.aaronkoblin.com/) on some of his creative works. His [compositions](http://www.aaronkoblin.com/work.html) are stunning and while I recall noticing many of those projects independently over time, it was seeing the evolution of his portfolio that really inspired me.

**If you watch the video you can see how his work went from a type of deliberate play to having a full “application”.** It’s a lesson I try to perpetually embody with a “just do it” attitude, and it’s rewarding to see someone having applied it with such success.

So in that vein, I decided to clean up one of my sites and pull together a lot of these components into something “useful”. **I call it “twitterline”**, because “twitterbar” may be more descriptive but doesn’t roll off the tongue as well. You can see an [example](http://twitterline.shelv.us/twitterline/wjhuie/7) and get a pretty good idea of what it’s used for.

The API is “RESTful” and is simply “http://twitterline.shelv.us/twitterline” followed by your twitter ID, e.g. “/wjhuie” and the number of days that you’d like to graph, e.g. “/4”. I’ve limited the number between [1, 14] and if you don’t supply a number the default is 7, which all make for reasonable defaults.**
  
** 

**However, beyond just looking at the bar graph on my site, you should be able to embed it wherever you wish!** You can check the source on my [example](http://twitterline.shelv.us/trial), mostly you’ll need to make sure [jQuery](http://jquery.com/) and [jQuery.Flot](http://code.google.com/p/flot/) are embedded first and you’ll likely want to tweak the CSS. Just let me know if you need help to it up and running or if you’d like some different defaults.

**It’s intended to be a simple culmination of a more complex process (which I’ll blog more on later) but I hope it inspires you to dust off a project of your own or start a new one!**
