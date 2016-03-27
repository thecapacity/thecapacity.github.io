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
I&#8217;ve been building my [personal site](http://wjhuie.com) and one of the things I&#8217;m excited about is the chance to interconnect my work with the larger social networks out there.

I believe every site should do what it&#8217;s best at and my intent isn&#8217;t to manage comments, wall posts or user signups and security. So once I got a basic life aggregator put together my next step was to integrate Google Friend Connect and see what it was all about.

The idea of Friend Connect is to let Google proxy most of the &#8220;social interactions&#8221; for you so you can simply deal with what you do best (which is likely developing useful content and interacting with other humans&#8230; not swearing over spam and comment plugins).

It&#8217;s incredibility easy to get up and running, you simply copy two HTML files to your site and let Google give it a once over. After that things are &#8220;configured&#8221; but there&#8217;s still no interface for interacting &#8220;socially&#8221; with the site.

In order do that you add a members gadget (or a smaller signon module) and &#8216;viola &#8211; People are now able to &#8220;join&#8221; your site! To get one of these plugins installed you use Google&#8217;s site to generate the necessary Javascript and HTML. I simply copied these codes into a &#8220;friend.html&#8221; file which is loaded via a corresponding menu item.

It&#8217;s not very difficult but you need to have a fairly straightforward &#8220;color scheme&#8221; defined and I don&#8217;t understand why &#8220;Links&#8221; and &#8220;Secondary Links&#8221; are two separate categories. I also don&#8217;t like that the CSS Style information is coded inline but I understand this makes it a single step to setup.

Nontheless it&#8217;s pretty straightforward, but it annoys me that Google makes you re-enter your color settings each and every time, i.e. not only if you regenerate one of the plugins but a new plugin is similarly &#8220;blind&#8221; as to your preferences. You also have to pick a general &#8220;size&#8221; for your widget which isn&#8217;t difficult thanks to the Web Developer&#8217;s Dislpay Ruler (under Miscellaneous).

So once it&#8217;s up and running, what are my impressions?

Well it&#8217;s certainly a neat idea but I was a bit underwhelmed. Currently, there&#8217;s only a &#8220;Wall&#8221; a &#8220;Rate and Review&#8221; widgets available and neither of my two test posts &#8220;left&#8221; the walled garden of my site. What I&#8217;d like is the ability to control the publishing of these &#8220;events&#8221; so that my twitter friends currently know if I&#8217;m active and engadged on a site!

It also wasn&#8217;t clear with the widgets how I&#8217;d solve typical &#8220;use cases&#8221;. For example currently my site&#8217;s &#8220;Wall&#8221; is only on a single page, but if you wanted to use this plugin for post comments how would that be done? How could I connect it to Akismet so I didn&#8217;t have to worry about spam filtering? How about other common features like emailing people when someone posts a folow up, or what about an RSS feed for this widget?

There doesn&#8217;t seem to be the typical wealth of developer documentation either. I haven&#8217;t yet investigated the &#8220;Lame Game Demonstration&#8221; or how to build a custom gadget. But given that it&#8217;s Google I think API&#8217;s and sample code is likely forthecoming.

Bottom line, it&#8217;s a start but given all the noise this has been making for so long I was expecting things to  be much farther along. Still get started and you&#8217;ll find yourself on the forefront of yet another Beta product from Google!