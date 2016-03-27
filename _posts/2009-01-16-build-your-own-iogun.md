---
id: 219
title: Build your own ioGun!
date: 2009-01-16T09:23:46+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=219
permalink: /2009/01/16/build-your-own-iogun/
categories:
  - code
  - hardware
  - inspiration
  - iobridge
  - opensource
  - python
---
I apologize for what will effectively be a brain dump post, but a new friend of mine from [the hackaday forums](http://hackaday.com/2009/01/14/wiimote-controlled-coil-gun/) is [getting started](http://blog.thecapacity.org/2009/01/13/wiimote-controlled-coil-gun/#comment-895) on his own accelerometer controlled system and I wanted to see if I could save him some time and frustration.

I think standing on the shoulders of Giants is a fantastic aspect of human nature, now if only someone could show me to some friendly Cyclopi (is that really the right pluralization?), and frankly I could not exist, let along thrive in the technological world if it were for the good graces of a great many people.

Perhaps I will write a \_very long\_ blog thank-you to them (if I can remember them all). So consider this my little chance at saving some of you a few hours of your precious time (and brain strain). Because of the very nature of hacking (i.e. everything&#8217;s just a little different and throw together) this can&#8217;t quite be the same quality as an instructable but it should get some of you started!

I&#8217;ve put up my code [here](http://svn.wjhuie.com/public_sandbox/trunk/iobridge/ioGun/) so that&#8217;s quite obviously the place to start. You&#8217;ll find a [python script](http://svn.wjhuie.com/public_sandbox/trunk/iobridge/ioGun/gen_wiid.py) and an [HTML page](http://svn.wjhuie.com/public_sandbox/trunk/iobridge/ioGun/j.html). The script connects to the [MoteDaemon](sourceforge.net/projects/motedaemon/) port I referenced earlier and will write these data points out to a JSON file which can be served via your webserver, and is then picked up by the HTML page.

If you look through the code you&#8217;ll see there&#8217;s clearly some tuning that can be done. As people on many forums pointed out there&#8217;s a &#8220;lag&#8221; which is really just because of my polling rates (and less to do with the network traffic).

Two important things here, first you&#8217;ll see that I created a separate thread for the monitoring and one for the output, that&#8217;s because the wiimote data is fast and furious and you don&#8217;t want to block and miss any!

Second, I know writing to an JSON file isn&#8217;t ideal so the output part actually buffers 10 events and writes those, luckily jQuery is smart enough to only pull the file if it&#8217;s changed so it&#8217;s not as bad as it sounds.

Once you&#8217;ve got the accelerometer data into python (and then into JSON) it&#8217;s &#8216;just&#8217; a matter of writing the webpage you want! jQuery makes some stuff really easy so I suggest [giving it a look](http://docs.jquery.com/) if you haven&#8217;t yet!

Of course what makes it really easy for me was the ioBridge module. I just plugged in a servo there and defined a widget on their page and &#8216;viola I had a webservice I could send commands to!

I hope that helps give some of you (or at least one of you) a boost, and if I can help out at all please let me know!