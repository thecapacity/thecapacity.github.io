---
id: 189
title: ioBridge setup
date: 2008-12-14T22:16:43+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=189
permalink: /2008/12/14/iobridge-setup/
categories:
  - hardware
  - iobridge
---
Although I&#8217;m often working with &#8220;web stuff&#8221; or Enterprise Architectures I&#8217;ve always had a soft spot for what used to be called &#8220;ubiquitious computing&#8221;. I once had an interview at IBM and their interpretation of the term was &#8220;the ability to buy something online wherever you are&#8221; (say wha!!?) so if you&#8217;re not familiar with the phrase I doubt it was ever popular enough to have an agreed upon meaning.

I much prefer the more current term, &#8220;augmented reality&#8221;, although that often has connotations of games or enhanced social interactions. If you ever get a chance to read Donnerjack I think it&#8217;s got some great illustrations of what life could be like once we unlock &#8220;computational intelligence&#8221; from the machine.

What&#8217;s always frustrated me about the computer is that for decades our most popular input device has been the mouse (although I&#8217;m a keyboard user myself). Why must I get RSI from paging through my Google Reader simply because the &#8216;j&#8217; key (or repeated clicking) was the &#8220;least worse alternative&#8221; for &#8220;next post&#8221;?

One of the reasons I&#8217;ve loved my iPhone is that it&#8217;s given me another chance at decoupling my &#8220;interface&#8221; from the computer. Even if it&#8217;s via simple web pages it&#8217;s nice to have an alternate. I have a friend who has setup a page to control his streaming DVR, audio and much more. It&#8217;s a simple advancement but being able take the convenience of a remote and the scripting power of code has been really powerful. If we can get excited about &#8220;always on&#8221; video cameras for life streaming, well I&#8217;m even more so about the time when my computer ( or cloud companion ) knows I&#8217;m on the move (and how urgently) because it can read my Nike+ transmitter!

If you&#8217;ve seen any impact of the &#8220;Makers Movement&#8221; you&#8217;ll likely agree that we&#8217;re entering a time in history where we have the ability to shape the way we want to interact with our devices. From on demand fabrication and open interfaces there&#8217;s likely a good chance that you can find a way to interface with what you want, how you want!

However, even with the multitude of arduino tutorials I&#8217;ve found it hard to get started with some of the embedded programming. I have some friends who are interested in using embedded systems to remove their need for the computer. My desires are a little different in that I want to use embedded systems to even more strongly couple myself to my system but via better interface patterns.

In essence I want the reverse of augmenting reality with technology, I want to augment software with more realistic interfaces! Let&#8217;s call it &#8220;realistic augmentation&#8221; until O&#8217;Reilly coins a more acceptable term.

So it was with much excitement that I saw the announcement of iobridge coming to market! From the writeups it looked like iobridge was a painless way achive the interlocking I&#8217;ve been looking forward to for so long!

I managed to buy one of the last modules (and a temperature sensor for starters) before they sold out and when it came I was setup in less then 5 minutes!

Here&#8217;s a quick record of what I did to get my test setup going for anyone else who&#8217;s interested in it. I already have a &#8220;usage case&#8221; in mind but I&#8217;ve been messing with jQuery for my new site (wjhuie.com) and hadn&#8217;t gotten around to it yet.

When my order came there were all of 4 things to deal with, one of which was supplied by me (the network cable). I took the   module (part 1) connected it to my router via a self-supplied Cat5 (part 2) and then plugged in the power cable (part 3). The power pack is a very nicely build module which has very low profile.

Once the module&#8217;s plugged in you&#8217;ll see a numeric LED display light up (along with two onboard LED&#8217;s, one red and one green). I plugged my module in and then went to the iobridge site and signed up. It asks you for your confirmation code (which you get via email after your order) and you&#8217;ll also have to be prepared to read off the serial number (stamped on the top of your module). Once you&#8217;ve entered those you hold down the button on the side of the module till it presents a &#8216;-&#8216; sign and click &#8220;next&#8221; on the page.

ioBridge will instruct your module to display 3 random numbers (at least I assume they&#8217;re random) and you simply read them off the digital display and enter them into the site. Once that&#8217;s been done, you are done. Literally, it&#8217;s that simple!

I then plugged my temperature module into the port (which is keyed to only go in one way so you can&#8217;t mess it up) and was instantly reading analog input values taken by my sensor off their website! It was a bit unintuative but I realize you can click on the &#8220;Raw&#8221; specification and convert it to a different unit (in my case Fahrenheit).

From there you can create a widget if you&#8217;d like by clicking on the widget menu then selecting &#8220;Create Widget -> I/O Module -> Analog Input -> Module Select -> Channel 1&#8243;&#8230; If that seems complex to you, trust me it&#8217;s not. You simply hit &#8220;Next&#8221; 5 times and &#8216;viola!

You can change the &#8220;Analog Input Scale&#8221; here as well and select &#8220;Auto refresh&#8221; so the data&#8217;s automatically updated (or you can click on the number to refresh). From this page you can also easily copy some javascript to embed on your page and you&#8217;re done!

If you&#8217;ve noticed I&#8217;m not much for screen shots but it&#8217;s as easy as can be to get running and I&#8217;m excited about what&#8217;s in store from the company and for me! Until next time, get real&#8230; go augment your life!

Update: I forgot to include the widget!
  
Check this out and see how hot my office gets, even in the winter!