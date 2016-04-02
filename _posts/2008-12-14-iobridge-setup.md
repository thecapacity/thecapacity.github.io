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
Although I’m often working with “web stuff” or Enterprise Architectures I’ve always had a soft spot for what used to be called “ubiquitious computing”. I once had an interview at IBM and their interpretation of the term was “the ability to buy something online wherever you are” (say wha!!?) so if you’re not familiar with the phrase I doubt it was ever popular enough to have an agreed upon meaning.

I much prefer the more current term, “augmented reality”, although that often has connotations of games or enhanced social interactions. If you ever get a chance to read Donnerjack I think it’s got some great illustrations of what life could be like once we unlock “computational intelligence” from the machine.

What’s always frustrated me about the computer is that for decades our most popular input device has been the mouse (although I’m a keyboard user myself). Why must I get RSI from paging through my Google Reader simply because the ‘j’ key (or repeated clicking) was the “least worse alternative” for “next post”?

One of the reasons I’ve loved my iPhone is that it’s given me another chance at decoupling my “interface” from the computer. Even if it’s via simple web pages it’s nice to have an alternate. I have a friend who has setup a page to control his streaming DVR, audio and much more. It’s a simple advancement but being able take the convenience of a remote and the scripting power of code has been really powerful. If we can get excited about “always on” video cameras for life streaming, well I’m even more so about the time when my computer ( or cloud companion ) knows I’m on the move (and how urgently) because it can read my Nike+ transmitter!

If you’ve seen any impact of the “Makers Movement” you’ll likely agree that we’re entering a time in history where we have the ability to shape the way we want to interact with our devices. From on demand fabrication and open interfaces there’s likely a good chance that you can find a way to interface with what you want, how you want!

However, even with the multitude of arduino tutorials I’ve found it hard to get started with some of the embedded programming. I have some friends who are interested in using embedded systems to remove their need for the computer. My desires are a little different in that I want to use embedded systems to even more strongly couple myself to my system but via better interface patterns.

In essence I want the reverse of augmenting reality with technology, I want to augment software with more realistic interfaces! Let’s call it “realistic augmentation” until O’Reilly coins a more acceptable term.

So it was with much excitement that I saw the announcement of iobridge coming to market! From the writeups it looked like iobridge was a painless way achive the interlocking I’ve been looking forward to for so long!

I managed to buy one of the last modules (and a temperature sensor for starters) before they sold out and when it came I was setup in less then 5 minutes!

Here’s a quick record of what I did to get my test setup going for anyone else who’s interested in it. I already have a “usage case” in mind but I’ve been messing with jQuery for my new site (wjhuie.com) and hadn’t gotten around to it yet.

When my order came there were all of 4 things to deal with, one of which was supplied by me (the network cable). I took the   module (part 1) connected it to my router via a self-supplied Cat5 (part 2) and then plugged in the power cable (part 3). The power pack is a very nicely build module which has very low profile.

Once the module’s plugged in you’ll see a numeric LED display light up (along with two onboard LED’s, one red and one green). I plugged my module in and then went to the iobridge site and signed up. It asks you for your confirmation code (which you get via email after your order) and you’ll also have to be prepared to read off the serial number (stamped on the top of your module). Once you’ve entered those you hold down the button on the side of the module till it presents a ‘-‘ sign and click “next” on the page.

ioBridge will instruct your module to display 3 random numbers (at least I assume they’re random) and you simply read them off the digital display and enter them into the site. Once that’s been done, you are done. Literally, it’s that simple!

I then plugged my temperature module into the port (which is keyed to only go in one way so you can’t mess it up) and was instantly reading analog input values taken by my sensor off their website! It was a bit unintuative but I realize you can click on the “Raw” specification and convert it to a different unit (in my case Fahrenheit).

From there you can create a widget if you’d like by clicking on the widget menu then selecting “Create Widget -> I/O Module -> Analog Input -> Module Select -> Channel 1″… If that seems complex to you, trust me it’s not. You simply hit “Next” 5 times and ‘viola!

You can change the “Analog Input Scale” here as well and select “Auto refresh” so the data’s automatically updated (or you can click on the number to refresh). From this page you can also easily copy some javascript to embed on your page and you’re done!

If you’ve noticed I’m not much for screen shots but it’s as easy as can be to get running and I’m excited about what’s in store from the company and for me! Until next time, get real… go augment your life!

Update: I forgot to include the widget!
  
Check this out and see how hot my office gets, even in the winter!
