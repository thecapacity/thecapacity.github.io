---
id: 311
title: Mashing up the Dashboard
date: 2009-07-01T11:32:48+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=311
permalink: /2009/07/01/mashing-up-the-dashboard/
categories:
  - business
  - frustration
  - Google
  - hacks
---
This post is for anyone interested in any of the Government Transparency inituatives. If you’ve been [This post is for anyone interested in any of the Government Transparency inituatives. If you’ve been](http://blog.sunlightfoundation.com/2009/03/06/transparency-visualized/) this topic then you’re probably aware that [Vivek Kundra](http://www.whitehouse.gov/the_press_office/President-Obama-Names-Vivek-Kundra-Chief-Information-Officer/) sees [a dashboard](http://bits.blogs.nytimes.com/2009/06/15/the-nations-co-government-needs-a-dashboard/) as a way of accelerating the transparency and transformation of the Government.

After watching groups like the [Sunlight Foundation](http://www.sunlightfoundation.com/) and [Change Congress](http://change-congress.org/) work their magic, I’ve now begun seeing much of this transformation from the inside due to my new job.

However, I still endeavor to participate externally as well and so I wanted to do some analysis on the public data.

In order to start, I wanted to import the [USASpending.gov](http://www.usaspending.gov/) information into Google spreadsheets, since I can kill two buzzwords at once by leveraging Cloud Computing services for Transparency!

**For a while, I fought with the easiest way to import the data and wanted to share what eventually worked for me.**

**First, create an new spreadsheet** and name your tab appropriately. **Then go to the** [**USASpending Feeds page**](http://it.usaspending.gov/?q=content/data-feeds) to select the specific data you want. I suggest starting with the Exhibit 300 information since it’s typically a smaller dataset and my Exhibit 53 tab with more than 1200 rows has proven to be very slow.

**Next pick, and I highly suggest reordering, the data fields** you want **you must then pick which Agency or Agencies you’d like info on**. Again, considering that lots of data will be pretty slow.

**Finally, select the CSV icon** which should open the download prompt for your browser. It’s unfortunate that the implementers didn’t use a dynamic tag here because you can’t simply copy the URL. Instead I had to first download the file itself and then **copy the originating URL into my clipboard** (I was using Chrome so how to do this will depend on your browser).

The URL should look like a much longer version of this:

> http://it.usaspending.gov/customcode/build_feed.php?extype=300&select1=agencyName&columns%5B%5D=bureauName…

Now that we’ve got the URL we can import everything into our spreadsheet by **selecting the A1 cell and entering**:

> <pre>=ImportData("<url>")</pre>

Where <span style="font-family: Consolas; line-height: 18px; font-size: 12px; white-space: pre; ">“<url>” <span style="font-family: Georgia; line-height: 19px; white-space: normal; font-size: 13px; ">is of course the long URL you copied earlier.</span></span>

**After a quick few seconds your data should be automajically imported!**

**For the Exhibit 300, things worked just great but for the Exhibit 53 data I ended up with each cell of Column A holding the full data for each entry. So in B1 I simply entered: =SPLIT(A1, “,”)** (note thre’s a bug with Google where the quotes ave to be double quotes not single) and then things auto populated left to right.

**Unfortunatley, the “SPLIT()” didn’t auto-populate downwards as well and dragging the function down the full B column is very very painful.**

Happy Data Hacking!
