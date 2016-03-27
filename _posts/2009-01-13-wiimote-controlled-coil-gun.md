---
id: 207
title: Wiimote Controlled Coil Gun
date: 2009-01-13T12:01:50+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=207
permalink: /2009/01/13/wiimote-controlled-coil-gun/
categories:
  - hacks
  - hardware
  - iobridge
---
I know it seems like a while since I last posted, but since my Christmas enlistment into the [Joint Strike Force](http://en.wikipedia.org/wiki/Tom_Clancy%27s_EndWar) I&#8217;ve been busy winning WWIII for America! Actually it&#8217;s not been all video games, I&#8217;ve been working on a Django project that I hope to release in a few weeks, and inspired by [ServoBeer](http://www.polymythic.com/2008/12/serv-obeer/) I managed to find time for some ioBridge hacking.

**However, as you can tell my recent militaristic tendencies have probably gotten the better of me!**

<div id="attachment_212" style="width: 235px" class="wp-caption alignleft">
  <a href="http://blog.thecapacity.org/wp-content/uploads/2009/01/img_3096.jpg"><img class="size-medium wp-image-212" title="ioBridge Coil Gun" src="http://blog.thecapacity.org/wp-content/uploads/2009/01/img_3096-225x300.jpg" alt="Weapons of War" width="225" height="300" srcset="http://blog.thecapacity.org/wp-content/uploads/2009/01/img_3096-225x300.jpg 225w, http://blog.thecapacity.org/wp-content/uploads/2009/01/img_3096-768x1024.jpg 768w" sizes="(max-width: 225px) 100vw, 225px" /></a>
  
  <p class="wp-caption-text">
    Weapons of War
  </p>
</div>

As you know from my previous posts **I&#8217;m interested bringing a higher level of physical awareness and capability to my typical computing environment.** Embedded systems are usually used to bypass the need for sophisticated systems, and the [ioBridge](http://iobridge.com/) is great for those situations.

Yet, I&#8217;m perfectly comfortable with the idea of using a laptop for a command and control system. It might be a more &#8220;tethered&#8221; solution then some people would like but I figure in a few years my iPhone will be able to fulfill the role my laptop currently has!

**I wanted to move beyond some of the work I&#8217;ve been doing with [passive](http://blog.thecapacity.org/2008/12/17/open-heart-surgery/) [sensors](http://blog.thecapacity.org/2009/01/02/taking-and-keeping-your-temperature/)** and in thinking about some ideas for a more active interfaces I realized that the wiimote certainly fits the description of a powerful controller! There are already [enough](http://blog.makezine.com/archive/2008/11/hacking_the_wiimote_ir_ca.html?CMP=OTC-0D6B48984890) [great](http://www.cs.cmu.edu/~johnny/projects/wii/) wii [control](http://blog.makezine.com/archive/2009/01/labyrinth_game_controlled_by_an_ard.html?CMP=OTC-0D6B48984890) [hacks](http://google-latlong.blogspot.com/2009/01/flying-through-google-earth-at-macworld.html) out there and some [programs](http://sourceforge.net/projects/darwiin-remote/) let you use the wiimote as a mouse!

But I wanted more then to just replace my mouse, and in most of these cases the flow control goes from the wiimote to a computer, or the [wiimote to an arduino](http://www.instructables.com/id/How_to_Control_Your_Robot_Using_a_Wii_Nunchuck_an/) with lots of wiring. I wanted to both avoid the electrical requirements as well as **extend the influence and &#8220;reach&#8221; of my wiimote beyond my immediate vicinity.**

**There are certainly times when you wouldn&#8217;t want to put your expensive new Macbook in harm&#8217;s way.** Whether on the front lines or in the basement laptops are sometimes the wrong systems to dedicate to a task, so ioBridge to the rescue!

The ioBridge already has a number of features which make it idea for &#8220;remote&#8221; situations and so **all I needed create was a mechanism to coordinate wiimote events sent to my computer with ioBridge events sent to the module**. Luckily the team at ioBridge has [created](http://www.iobridge.net/forum/index.php/topic,81.0.html) just such an API!

In this setup I use a python script and [MoteDaemeon](http://screenfashion.org/releases/motedaemon/) to bring the events into my laptop&#8217;s domain. Of course I could script up a number of local events but what I wanted to do was act on that data and sending commands to an ioBridge module**.**

 **So I build a website which monitors the position from one axis of the wiimote and extrapolates that position to a servo output on a remote ioBridge module (which happens to be in my office, but doesn&#8217;t have to be).** I&#8217;m also tracking some of the button inputs and can expand easilly to include other [axes](http://www.answers.com/topic/axis) as well!

**Now I just needed something fun to control on the other end!** I happened to have an office golf putter lying around and that uses an electromagnetic induction coil to generate the force needed for the ball return when you sink a putt. **If you take that apart and replace the metal cylinder with something a tad bit smaller then you&#8217;re left with a coil gun capable of some pretty powerful shooting!**

I&#8217;m not sure it&#8217;s up to [DoD](http://www.defenselink.mil/) standards yet but remember the only required link between the wiimote and the ioBridge module is the Internet so if you can sneak one of these into your friend&#8217;s house, or a colleagues&#8217; office, then **you could be anywhere on the planet with Internet access and stick it to them!** 

**How&#8217;s that for &#8220;Can you hear me now?&#8221;.** Check out the video of it in action on [YouTube](http://www.youtube.com/watch?v=aBKoaXtHyfs) or [Vimeo](http://www.vimeo.com/2815315)!