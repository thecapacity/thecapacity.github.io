---
id: 288
title: Hadoop Benchmark and CouchDB Implications
date: 2009-04-23T14:01:17+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=288
permalink: /2009/04/23/hadoop-benchmark-and-couchdb-implications/
categories:
  - couchdb
  - mapreduce
---
**Although I don’t write much about it directly I’m a big fan of the [MapReduce](http://labs.google.com/papers/mapreduce.html)** approach to computing and data mining. I love the fluid manner with which it encourages partitioning and how little code is required to focus directly on problem solving vs. setup and communication Also, [as Paco admits](http://ceteri.blogspot.com/2008/02/hadoop-part-1-approach.html) it’s familiar to anyone who’s had an enterprise mainframe experience.

In the microcosm I’ve found myself using more of the functional programming methods that python provides, and I’m always interested in big announcements like Amazon’s new [Elastic MapReduce](http://aws.amazon.com/elasticmapreduce/). However, I think [Hadoop](http://hadoop.apache.org/core/) has been an exciting development and [I’ve been tracking it for a while](http://www.slideshare.net/thecapacity/hadoop-overview-presentation) though unfortunately I lack a large cluster, and more importantly a large problem to leverage it on.

**Thus, you can imagine my skepticism when [a recent paper](http://database.cs.brown.edu/sigmod09/), co-authored by Microsoft, was released stating how poor Hadoop performance was compared to some RDBMS solutions.**

Having worked with many product and marketing teams I’m aware that benchmarks don’t always translate into real world behavior. However, I’ve also worked with a number of benchmark teams and I know that these technical experts have an honest integrity to their work.

So I took a [book reading](http://blog.thecapacity.org/category/books) break and sat down with the paper, aptly titled “[A Comparison of Approaches to Large-Scale Data Analysis](http://database.cs.brown.edu/sigmod09/benchmarks-sigmod09.pdf)” to see what I could discern. **It’s a well written pape**r, is actually straightforward at **only 14 pages**, and **often frank about some of the pros, cons and issues they encountered for all of the products during their tests.**

Not being a hadoop expert or having a 100 node cluster **I can’t really argue or recreate the details** (go read the paper) but I will mention that **there are some interesting acts of  <span id=":nn" dir="ltr">naivety</span>**. For example they _“cat”_ the output of the SQL command to a file but instead of aggregating the individual output logs with;

> <pre>hadoop fs -cat output/* > combined_file</pre>

**They force a final map/reduce phase even though earlier in the paper they admit the JAVA overhead for short tasks is significant.**

**However, I think regardless of the outcome** (and I don’t know of anyone who’s tried to refute the tests) **there are many lessons here for CouchDB.** 

The team split most of the tests into two types of environments, one where the data per node was constant (533MB / node) and the other where the data per cluster was constant (1TB overall). Specially in the analytical benchmark example (Section 4.3) they mining websever logs and data.

**CouchDB has a fantastic ability to replicate data between instances but unfortunately the only way you could aggregate knowledge across all your data would be (a) run multiple queries against the view on each server or (b) aggregate all this data to a central CouchDB isntance.**

I can easilly imagine a scenario where this fragementation could occur (say multiple sharded databases) and picture that as you scale (b) will quickly become impossible. Imagine trying to aggregate every tweet back to a main store, this would quickly require emmense diskspace and likely commercial storage solutions (which defeats the point of commodity hardware).

**I don’t think CouchDB needs to morph into a parallelized database but I believe this distributed & aggergated query environment needs to develop for it to function at the envisioned scale.**

Finally, let me say that I’m glad couchdb didn’t end up written in JAVA and I’m looking forward to seeing if implementations like [Disco](http://discoproject.org/) can further accelerate the performance of mapreduce systems. I also think Amazon’s offering will make it even easier to afford 1000’s of hadoop nodes instead of a 100 node license for a commercial RDBMS environment (which is certainly not something they mention in their evaluation).
