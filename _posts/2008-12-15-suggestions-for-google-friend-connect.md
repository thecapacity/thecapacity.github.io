---
id: 193
title: Suggestions for Google Friend Connect
date: 2008-12-15T09:48:00+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=193
permalink: /2008/12/15/suggestions-for-google-friend-connect/
categories:
  - Google
  - social
---
I’ve been building my [personal site](http://wjhuie.com) and one of the things I’m excited about is the chance to interconnect my work with the larger social networks out there.

I believe every site should do what it’s best at and my intent isn’t to manage comments, wall posts or user signups and security. So once I got a basic life aggregator put together my next step was to integrate Google Friend Connect and see what it was all about.

The idea of Friend Connect is to let Google proxy most of the “social interactions” for you so you can simply deal with what you do best (which is likely developing useful content and interacting with other humans… not swearing over spam and comment plugins).

It’s incredibility easy to get up and running, you simply copy two HTML files to your site and let Google give it a once over. After that things are “configured” but there’s still no interface for interacting “socially” with the site.

In order do that you add a members gadget (or a smaller signon module) and ‘viola – People are now able to “join” your site! To get one of these plugins installed you use Google’s site to generate the necessary Javascript and HTML. I simply copied these codes into a “friend.html” file which is loaded via a corresponding menu item.

It’s not very difficult but you need to have a fairly straightforward “color scheme” defined and I don’t understand why “Links” and “Secondary Links” are two separate categories. I also don’t like that the CSS Style information is coded inline but I understand this makes it a single step to setup.

Nontheless it’s pretty straightforward, but it annoys me that Google makes you re-enter your color settings each and every time, i.e. not only if you regenerate one of the plugins but a new plugin is similarly “blind” as to your preferences. You also have to pick a general “size” for your widget which isn’t difficult thanks to the Web Developer’s Dislpay Ruler (under Miscellaneous).

So once it’s up and running, what are my impressions?

Well it’s certainly a neat idea but I was a bit underwhelmed. Currently, there’s only a “Wall” a “Rate and Review” widgets available and neither of my two test posts “left” the walled garden of my site. What I’d like is the ability to control the publishing of these “events” so that my twitter friends currently know if I’m active and engadged on a site!

It also wasn’t clear with the widgets how I’d solve typical “use cases”. For example currently my site’s “Wall” is only on a single page, but if you wanted to use this plugin for post comments how would that be done? How could I connect it to Akismet so I didn’t have to worry about spam filtering? How about other common features like emailing people when someone posts a folow up, or what about an RSS feed for this widget?

There doesn’t seem to be the typical wealth of developer documentation either. I haven’t yet investigated the “Lame Game Demonstration” or how to build a custom gadget. But given that it’s Google I think API’s and sample code is likely forthecoming.

Bottom line, it’s a start but given all the noise this has been making for so long I was expecting things to  be much farther along. Still get started and you’ll find yourself on the forefront of yet another Beta product from Google!
