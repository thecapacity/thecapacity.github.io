---
id: 364
title: I love Chrome Cookies
date: 2010-08-24T20:58:58+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=364
permalink: /2010/08/24/i-love-chrome-cookies/
categories:
  - frustration
  - hacks
---
OK,

I know it’s been way too long since I’ve posted and this isn’t intended as a heartfelt explanation, merely a reference for those in need.

I’ve been hacking up a storm and shoehorning my way around problems and this is no different.

If you need to use wget with a site that requires authentication, then you need to dump some cookies from Chrome (or Firefox) because they’ve moved from [cookies.txt](http://blog.omnux.com/index.php/2008/03/25/cookiestxt-file-format/) to sqlite3.

Here’s how (when typing this from the cmdline hit ^V then <Tab> for the separator, i.e. after the 1st ‘:

<pre>sqlite3 -separator '       ' Cookies 'select host_key, httponly, path, secure, expires_utc, name, value from cookies' > ~/Sites/uservice/chrome_cookies.txt
</pre>

Unfortunately that didn’t work for me (not sure why wget wasn’t correctly reading the file) so I ended up using Firebug to look at the HTTP headers, and then using wget with:

<pre>wget --no-cookies --header= "Cookie: name1=v1; name2=v2; name3=v3"
</pre>

It’s actually way easier than it looks to get your header correctly, just copy and paste what you see from Firebug (look at the “Net” field and have it show all communications, what you want is the 1st line sent).

I figured this out [with some help](http://www.linuxquestions.org/questions/linux-software-2/using-the-cookies-sqlite-from-firefox-3-in-wget-653227/) but mostly by my lonesome given the differences for Chrome.
