---
id: 199
title: Taking (and keeping) your temperature!
date: 2009-01-02T16:29:22+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=199
permalink: /2009/01/02/taking-and-keeping-your-temperature/
categories:
  - code
  - Google
  - hardware
  - iobridge
  - python
  - visualization
---
I swear I don’t have a penchant for medical terminology but [this ioBridge stuff](http://iobridge.com/) is making me feel like that time I stayed at a Holiday Inn… so refreshing, I think I could perform surgery!

After my heart hacks (see my previous posts) I had questions from some friends about how we could graph the data from an ioWidget’s (my term). Initially, I wanted to push the data into a Google Spreadsheet but unfortunately there doesn’t seem to be a higher level javascript library supporting Google Docs and sorting through the feed URL’s was just too complicated.

At the same time I was thinking through this request, I received a resounding response from the ioBridge team! Although I’d worked around the need for a simple API they quickly responded to the desire (apparently I wasn’t the only one with interesting ideas) and now there’s [a full JSON API](http://www.iobridge.net/projects/2008/12/data-feed-api/)!

I won’t bore you with my initial graphing solution as the sample made it into [their official API demo](http://www.iobridge.com/ServerTempChart.html) (along with some much needed code enhancements). However, there were a few pitfalls with that approach that I still didn’t like, most specifically that the data is “lost” every time you reload the page.

It took me until after the holiday break (along with a welcome return to python) but I’ve solved my initial frustration with great results.

Here’s [a script](http://svn.wjhuie.com/public_sandbox/trunk/iobridge/gMonitor/update_gdata.py) which will poll my ioBridge module and then [store the results](http://spreadsheets.google.com/pub?key=p_9k09whFucDqlxl3LwEuTQ) of my tempreature sensor in a Google Spreadsheet that I created! Once the data’s there you can use Google’s visualization widgets to make some fun graphs!

Aside from some setup and “ease of use” code, the real work is done by two very brief classes. I deliberately didn’t add some error checking nor make the widget class generic (it’s actually proxying the full ioBridge module) so I think it should be straightforward enough to modify for your own uses!

All you need to do is create a spreadsheet and open it in your browser. Copy the key from that URL and paste it, along with your ioBridge feed URL, into the appropriate places in the script (the locations are commented).

I simply run this from a cron script every few minutes (once I get more data I’ll reduce the time) and although there’s not a lot of variation in the data (I deliberately introduced some to make the graph more interesting) it’s a spectacular way to record, visualization and act on the sensor’s findings!

Good luck with your own modifications and let me know if I can help!

