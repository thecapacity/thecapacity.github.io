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
I&#8217;ve been using a lot of python, jQuery and web services recently and thought it was time to pull together those skills into a public app.

Like most developers, I often &#8220;scratch my own itch&#8221; and write code to solve a problem or learn something. I try to [post](http://blog.thecapacity.org/category/code/) what I can but some solutions are hosted internally and I know there are numerous code fragments scattered across my hard drives which haven&#8217;t made it into posts.

Many of these projects are &#8220;evolutionary dead ends&#8221; but I think **it&#8217;s important to engage in &#8220;purposeful play&#8221;** without anticipating success or failure. You really have to take time to nurture your childlike creativity and **it&#8217;s often in these limitless exercises that we develop the foundation for real breakthroughs for more &#8220;respected&#8221; works.**

I was reminded of this recently when I watched a [presentation](http://vimeo.com/3199933) by [Aaron Koblin](http://www.aaronkoblin.com/) on some of his creative works. His [compositions](http://www.aaronkoblin.com/work.html) are stunning and while I recall noticing many of those projects independently over time, it was seeing the evolution of his portfolio that really inspired me.

**If you watch the video you can see how his work went from a type of deliberate play to having a full &#8220;application&#8221;.** It&#8217;s a lesson I try to perpetually embody with a &#8220;just do it&#8221; attitude, and it&#8217;s rewarding to see someone having applied it with such success.

So in that vein, I decided to clean up one of my sites and pull together a lot of these components into something &#8220;useful&#8221;. **I call it &#8220;twitterline&#8221;**, because &#8220;twitterbar&#8221; may be more descriptive but doesn&#8217;t roll off the tongue as well. You can see an [example](http://twitterline.shelv.us/twitterline/wjhuie/7) and get a pretty good idea of what it&#8217;s used for.

The API is &#8220;RESTful&#8221; and is simply &#8220;http://twitterline.shelv.us/twitterline&#8221; followed by your twitter ID, e.g. &#8220;/wjhuie&#8221; and the number of days that you&#8217;d like to graph, e.g. &#8220;/4&#8221;. I&#8217;ve limited the number between [1, 14] and if you don&#8217;t supply a number the default is 7, which all make for reasonable defaults.**
  
** 

**However, beyond just looking at the bar graph on my site, you should be able to embed it wherever you wish!** You can check the source on my [example](http://twitterline.shelv.us/trial), mostly you&#8217;ll need to make sure [jQuery](http://jquery.com/) and [jQuery.Flot](http://code.google.com/p/flot/) are embedded first and you&#8217;ll likely want to tweak the CSS. Just let me know if you need help to it up and running or if you&#8217;d like some different defaults.

**It&#8217;s intended to be a simple culmination of a more complex process (which I&#8217;ll blog more on later) but I hope it inspires you to dust off a project of your own or start a new one!**