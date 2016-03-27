---
id: 71
title: Can your datacenter handle this?
date: 2008-06-25T16:41:32+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=71
permalink: /2008/06/25/can-your-datacenter-handle-this/
categories:
  - datacenter
  - enterprise
  - Google
---
Google recently hosted their [I/O conference](http://news.cnet.com/At-IO%2C-getting-technical-with-Google/2009-1032_3-6240414.html) and, during that, a Google Fellow named [Jeff Dean](http://research.google.com/people/jeff/index.html) illuminated some of their operational measurements;

  * A single search query touches 700 to 1,000 machines in less then 0.25 seconds.
  * They currently have 36 data centers containing over 800,000 servers
  
    with 40 servers/rack.

That&#8217;s about 555 racks per datacenter and if a standard 19&#8243; rack is ~61 sqft that means they&#8217;ve got 33,855 sqft of raised floor space. Which averaged (be careful of averages) over 36 datacenters is about 950 sqft of commutate space each. Which is probably much smaller then the actual sizes.

We know from experience that they use BigTable (their distributed storage service) and MapReduce (cluster computing) a lot.

  * The largest BigTable instance manages about 6 petabytes of data spread across thousands of machines.

I think 6 petabytes actually seems kind of low. Although I realize that&#8217;s about one hundred times the amount of data in the Library of Congress, it seems to me that they likely have a very large number of BigTable clusters.

  * They&#8217;ve had 29,000 MapReduce
  
    jobs in August 2004 and 2.2 million in September 2007 and the average time to complete a job has dropped from about 10 minutes to 6 minutes. .

That seems like an astounding increase and makes a clear statement that it&#8217;s a valid programming paradigm for data processing. One can only imagine how much their infrastructure has compounded (both in size and computing capacity) to accommodate such an increase in volume and still cut the time almost in half.

  * In a typical day will they&#8217;ll run about 100,000 MapReduce jobs each of which occupies about 400 servers.

If you take 400 servers per job times 100,000 jobs that would imply about 40M machines. I know they&#8217;re not all being run at the same time (and we know from earlier that they have about 800,000) servers but combined that suggests they&#8217;re seeing a contribution factor of ~50 ( 40M / 800K ) from each machine.

  * The data output by these MapReduce tasks has risen from 193 terabytes to 14,018 terabytes.

I&#8217;m not sure it&#8217;s valid to try to compare the data out with the data being stored since we don&#8217;t know how many BigTable instances they have running, but they&#8217;ll often recompute data instead of storing a cached copy. Their other big challenge in computing is getting the data
  
shuttled around the network. It also seems typical (especially in the web world) that the data you
  
compute from a source can be much larger then that original data. So it seems likely that Google&#8217;s found a well balanced compute cost vs. data storage tradeoff that works for them.

They also have some interesting insight into the frequency and costs of various failures for a 1 year period;
  
On average, for a typical cluster configuration of 1000 machines you&#8217;ll have;

  * 1000+ HD failures, 20 mini switch failures and 5 full switch failures and 1 PDU failure
  * ~1/2 will overheat, forcing a power down of most machines in <5 mins and taking ~1-2 days to recover.
  * ~1 PDU failure, ~500-1000 machines suddenly disappear and take ~6 hours to come back
  * ~1 rack-move advanced notice but ~500-1000 machines powered down and take ~6 hours to bring back up [Note this seems to contradict the 40 machines per rack statement but it may have to do with intra cluster communication links]
  * ~1 network rewiring, rolling ~5% of machines down over 2-day span
  * ~20 rack failures, 40-80 machines instantly disappear, 1-6 hours to get back
  * ~5 racks go wonky, 40-80 machines see 50% packetloss
  * ~8 network maintenances, 4 might cause ~30-minute random connectivity losses
  * ~12 router reloads, takes out DNS and external VIPs for a couple minutes
  * ~3 router failures, have to immediately pull traffic for an hour
  * ~dozens of minor 30-second blips for DNS