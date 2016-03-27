---
id: 78
title: What if stocks were movies?
date: 2008-07-07T15:27:26+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=78
permalink: /2008/07/07/what-if-stocks-were-movies/
categories:
  - code
  - python
  - visualization
---
As you&#8217;ve seen from my previous posts, I&#8217;ve been playing with [python](http://blog.thecapacity.org/category/python/), [couchdb](http://blog.thecapacity.org/2008/07/04/python-couchdb-sample/) and been [working my way through](http://blog.thecapacity.org/2008/04/17/oreilly-make-me-an-offer/) the &#8220;Programming Collective Intelligence&#8221; book. However, what I haven&#8217;t been talking about much is what I&#8217;ve actually been doing with it!

**When learning something, I&#8217;m at a disadvantage unless I can relate the technique toward an application, even if it&#8217;s just hypothetical.** Since my day job is mostly spent at an IT / Architecture level it can often be difficult to get a chance to move beyond theoretical &#8220;what ifs&#8221;.

**It&#8217;s a constant balance to make progress through a book or tutorial vs. branching off to investigate and actually apply something.** As a rampant &#8220;[consumer](http://www.softwarebyrob.com/2008/05/18/the-single-most-important-career-question-you-can-ask-yourself/)&#8221; I naturally error toward the &#8220;more data&#8221; side of things rather then being able to take time and explore in depth (I still have Google App Engine in my queue to revisit).

**With the potential of [PCI](http://blog.kiwitobes.com/), I&#8217;ve been really focused on working through the examples and trying to explore what a good tangential application might be.** Outside of technology I have a lot of varied interests (e.g. some of my friends call me &#8216;Longbow&#8217;) not the least of which is the idea that business and money is always an interesting area to investigate.

**So I decided to mess around with applying PCI&#8217;s recommendation techniques to the field of stock analytics.** There&#8217;s a theory called the &#8220;[efficient market hypothesis](http://en.wikipedia.org/wiki/Efficient_market_hypothesis)&#8221; which holds that [the crowd is wise](http://en.wikipedia.org/wiki/The_Wisdom_of_Crowds) and that if someone does have some corner on knowledge it (a) won&#8217;t be you and (b) won&#8217;t be legal.

Ok, the last two implications are my own but the theory basically suggests that you shouldn&#8217;t try to (and in fact can&#8217;t) beat the market because it&#8217;s the one making all the rules. Also, consider that if a stock is priced at $90, even if the stock is &#8220;worth&#8221; $100, it&#8217;s actually not worth $100 because the market will only pay $90!

Let&#8217;s leave off arguing the truth (or un-truth) of this hypothesis, we all know fortunes are made by being ahead of the crowd (whether by skill or luck) **and see what sort of insights we can gain from the data itself!**

**Here&#8217;s what I did;**

  1. I took a list of 125 stock symbols and [built a python class](http://media.thecapacity.org/files/get_stock_data_py.html) to help me out ( <span style="text-decoration: underline;">can someone please share a good wordpress plugin for code?</span> ). The class is used to parse [Google Finance](http://finance.google.com) data and extract stock information (currently just the closing price information).
  2. Once I had this data and as an educational aside (and since I was certain Google didn&#8217;t want me hitting up their servers all the time) I built a couchdb database for this information. Now that Google finance [has an API](http://googledataapis.blogspot.com/2008/06/google-data-apis-now-easier-to-use-for.html) it might be unnecessary but I&#8217;ve yet to investigate it.
  3. Now I can query the database and build a python dictionary of stocks and their historical information and apply the PCI techniques for &#8220;recommendations&#8221; on this datastructure.

I don&#8217;t know about you but I think that&#8217;s pretty cool!

**Let me corral my excitement for a second and explain my thought process.** The initial examples in PCI are based on movie recommendations; a **critic** will watch a **movie** and issue a numerical **rating**. In my stock analogy a critic => **stock_symbol** a movie => **date** and a rating => **closing_price**.

That might be a stretch for the relationship mapping but remember **the PCI techniques can rate clusters for nearly everything which has a numerical value (or can be turned into a numerical representation).**

So since they&#8217;ve been nice enough to share the financial data, let&#8217;s take GOOG as an example;

> >>> stock_values[&#8220;GOOG&#8221;]
  
> {u&#8217;26-Mar-08&#8242;: u&#8217;458.19&#8242;, u&#8217;4-Dec-07&#8242;: u&#8217;684.16&#8242; &#8230; }

**In my example GOOG&#8217;s is not actually a critic but being _critiqued_ by the crowd and on March 26th they rated it a 458.19 on the scale (from 0 &#8211; infinite).** Here&#8217;s the rating for a different stock on that same day;

> >>> stock_values\[&#8220;YHOO&#8221;\]\[&#8217;26-Mar-08&#8242;\]
  
> u&#8217;28.49&#8242;

Obviously sim_distance() (PCI&#8217;s Euclidian measure) won&#8217;t do because of the massive discrepancy in the stock prices. **However, the sim_pearson() rating will take these &#8220;rating tendencies&#8221; into effect**, just like it will for critics who always judge movies optimistically, in effect normalizing your data for you. <span style="text-decoration: underline;">Note, you&#8217;ll have to tweak these functions from the original PCI to cast the value to floats before summing.</span>

**Now, for a given stock I can find the one other stock (among my sample of 125 stocks) which matches it most (or least) similarly in price fluctuations based on the closing price per day traded.** 

> >>> topMatches(stock_values, &#8220;GOOG&#8221;, n=1)
  
> [(0.92134859322750906, u&#8217;LOGI&#8217;)]
  
> >>> topMatches(stock_values, &#8220;YHOO&#8221;, n=1)
  
> [(0.45166743704701778, u&#8217;BIIB&#8217;)]

You might also consider finding relationships based on trading volumes but I haven&#8217;t yet learned how to do correlations based on multiple relationships.

**So let&#8217;s look at all the stocks;**

> >>> for s in stock_values.keys():
  
> &#8230; rank, stock = topMatches(stock_values, s, n=1)[0]
  
> &#8230; print &#8220;%s is best matched (%s) by %s&#8221; % (s, rank, stock)
  
> &#8230;
  
> AAPL is best matched (0.903380774431) by BIDU

**That&#8217;s the &#8220;best matches&#8221; for 125 of the NYSE&#8217;s top stocks, and a quick bit of shell scripting;
  
** 

> cat matches | cut -d &#8221; &#8221; -f7 | sort | uniq | wc -l
  
> 68

**Shows us that the prices (up and down) for the 125 stocks I&#8217;d selected are in fact represented by the movement of just 68 overall stocks!**

**That&#8217;s like being able to capture the excitement from a full summer of movies by only going to see 55% of the movies!**