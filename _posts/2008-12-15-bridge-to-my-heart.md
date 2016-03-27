---
id: 195
title: 'Bridge to my heart&#8230;'
date: 2008-12-15T16:19:39+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=195
permalink: /2008/12/15/bridge-to-my-heart/
categories:
  - code
  - hardware
  - iobridge
  - technology
  - twitter
---
Yesterday I [talked quickly](http://blog.thecapacity.org/2008/12/14/iobridge-setup/) about my new ioBridge module and how excited I was to get started with it!

As I mentioned, my real goal is to bring physical interactions into the digital world and I had a heartbreakingly simple desire to get started with! Like most of you I&#8217;m sure, I have a special someone who&#8217;s stuck on the other side of the tubes most of the day.

I&#8217;ve known several coworkers to spend quite some time on their cellphones with family members exchanging tidbits throughout the day. I&#8217;m lucky that my spouse has some significant technology savvy (who says they can&#8217;t be trained&#8230; j/k hon!) so we get a lot of our &#8220;chit chat&#8221; done without breaking the context of our mental modes by using gtalk.

I&#8217;m sure many of you use some form of instant messaging for work, and have developed an enjoyable bit of asynchronicity to your conversations. It&#8217;s rewarding to be able to stay in the zone and then pick up a conversation where you left off once you&#8217;ve got a moment&#8217;s break.

However, how many of you have started a conversation with &#8220;Hey, are you there?&#8221;. It&#8217;s a personal pet peeve of mine (i.e. tell me what you need so I can deal w/ it when I have a chance) but often there&#8217;s no need beyond wanting to know the other person&#8217;s there and still has a pulse.

So that&#8217;s where [iobridge\_project\_v0.01](http://www.wjhuie.com/heart.html) comes in!

If you&#8217;ve checked out the link and are a bit confused here&#8217;s the explanation!

1) I wanted to build a digital representation of my physical presence and I decided that a &#8220;heartbeat&#8221; was a pretty obvious analogy.

2) So I found [a wonderful tutorial](http://www.webdesign.org/web/photoshop/imageready-animation/drawing-animated-heart-exclusive-tutorial.9282.html) on how to draw animated gifs but stopped with [a single heart jpg](http://www.wjhuie.com/heart.jpg) and then needed to animate this beating in correspondence with my presence.

3) Using the very simple iobridge widget script I&#8217;m able to [display the temp](http://www.wjhuie.com/iotemp.html) from my sensor wherever I want. However,  I wanted to actually \_act\_ on this data!

I&#8217;ve already talked with some of the ioBridge guys about them providing an XML / JSON interface and they were really receptive but there&#8217;s so much excitement over their device that I knew it would take a little bit of time before they could put something together.

First I tried to use jQuery to make a JSONP call to their server, but that didn&#8217;t work. Although I&#8217;m not smart enough to understand exactly how JSONP works, but I know the server has to cooperate. Despite their enthusiasm I knew ioBridge had more important things to work on and couldn&#8217;t turn around an API in 10 minutes (though I&#8217;m sure they&#8217;d try if you asked)!

I consider myself a persistent fellow, so I broke out firebug and started tracing through their script! It&#8217;s not rocket science, but the session variables and timestamps were a bit daunting. I must say they&#8217;ve put some thought and effort in securing your data (they even use HTTPS) and I started to feel myself deflate.

However, I decided to have a bit of lunch before I gave up entirely and **that&#8217;s when I had my brightest idea all day!**

4) I realized I didn&#8217;t need to worry about &#8220;spoofing&#8221; the request I could simply let ioBridge&#8217;s code do what it did best, polling the data! In turn, I did what I could do best, which was utilize the power of jQuery to get to that data!

5) **So I simply hid the content div that I&#8217;d wrapped the ioBridge Widget in and proceeded to parse out the data!** It&#8217;s as simple as;

> `$("#content").hide();`

Then to get the data you just need to do this;

> `var t = $("#content").text().split(";")[1];<br />
new_temp = parseFloat(t);`

6) After that you just need a little UI logic and you&#8217;re good to go! Here&#8217;s a description of what I settled on but check out the page for the full on code;

I build an &#8220;animate heart&#8221; function which is triggered to fade in and fade out the heart. This is a simple function and could get more complicated if you&#8217;re a better animator then I am. Then I build a control function which would query the current temperature and compare that to the previous temperature. If things had changed (up or down) it simply tweaked the timing interval and reset the trigger event on the heart icon to have it beat faster or slower!

So there you have it! I can tuck the tempreture gague under my laptop so you can see how hard I&#8217;m working, or I can put it the back of my chair so you can get visual feedback as my body heats up the seat!

I know you&#8217;ll have to take [the pulsing example](http://www.wjhuie.com/heart.html) with a bit of faith, but compare that to [the temerature results](http://www.wjhuie.com/iotemp.html) and see the correlation! Also you can [twitter me](http://twitter.com/wjhuie) if you want me to go hold the sensor and make sure I&#8217;m moslty human!