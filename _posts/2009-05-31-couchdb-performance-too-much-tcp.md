---
id: 299
title: 'CouchDB Performance – Too much TCP'
date: 2009-05-31T20:20:53+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=299
permalink: /2009/05/31/couchdb-performance-too-much-tcp/
categories:
  - code
  - couchdb
  - python
---
It’s been a while since I ran [my CouchDB performance test](http://blog.thecapacity.org/2009/03/19/couchdb-performance-or-use-a-file/), but many of the comments I received suggested that updating my codebase should yield some significant performance improvements. Unfortunately, at the time I didn’t have spare cycles to invest in building the latest branches of erlang, couchdb and everything else, so I hadn’t previously been able to rerun my tests.

However, I started a new project today and, like most developers, I took some time to sharpen my tools before I felt sufficiently prepared to proceed. Of course since one of my favorite tools is CouchDB itself I checked in to see how [it had been progressing](http://wiki.apache.org/couchdb/Breaking_changes) and I was thrilled to see [Janl](http://twitter.com/JanL), and it looks like [others](http://twitter.com/topfunky) have [contributed](http://twitter.com/jonGretar), had released a new version of the excellent [DBX bundle](http://janl.github.com/couchdbx/)!

So after a round of updating DBX and CouchDB [python library](http://code.google.com/p/couchdb-python/) components, I decided to suffer a small distraction and give the new code a test drive.
  
I wanted to check my baseline, so here’s a rough time sample for the original, file based, keywords code:

> <pre>time ./finding_keywords.py
[('Hacker', 249160.0), ('Techcrunch', 249160.0)]

real    0m0.329s
user    0m0.225s
sys    0m0.046s</pre>

I ran the initial load and it looks much the same as the previous test:

time ./couchdb\_finding\_keywords.py

> <pre>real    28m16.430s
user    2m55.550ssys    1m30.335s</pre>

So perhaps around 20% faster, though on a second test run this actually took more than 39 minutes!

Well, now that the load is out of the way, let’s see how are our queries are looking.

Well after making a view, the results with wget aren’t any more promising then last time (note the view location has changed):

> <pre>wget -O - http://localhost:5984/keywords/_design/finding/_view/word_count?group=true > /dev/null
--20:35:08--  http://localhost:5984/keywords/_design/finding/_view/word_count?group=true
           => `-'
Resolving localhost... 127.0.0.1, ::1, fe80::1
Connecting to localhost|127.0.0.1|:5984... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [text/plain]

    [                     <=>                        ] 422,776       12.54K/s             

20:35:40 (12.94 KB/s) - `-' saved [422776]</pre>

Alas, it doesn’t look like most of the performance improvements have really paid off for this testcase, in fact every run I tried was slower then last version.
  
Here’s a sample run which is fairly indicative of the rest:

> <pre>time ./couchdb_finding_keywords.py
[('Hacker', 249160.0), ('Techcrunch', 249160.0)]

real	0m52.659s
user	0m0.702s
sys	0m0.441s</pre>

And again, with more of the full debugging info:

> <pre>time ./couchdb_finding_keywords.py
[('Hacker', 249160.0), ('Techcrunch', 249160.0)]
>>>---- Begin profiling print
         788 function calls (709 primitive calls) in 51.297 CPU seconds

   Ordered by: internal time, call count
   List reduced from 118 to 20 due to restriction <20>

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        2   51.092   25.546   51.092   25.546 socket.py:278(read)
        2    0.049    0.024    0.049    0.024 decoder.py:320(raw_decode)
        1    0.040    0.040    0.040    0.040 ic.py:182(__contains__)
       14    0.036    0.003    0.036    0.003 socket.py:321(readline)
        1    0.020    0.020    0.020    0.020 couchdb_finding_keywords.py:61(build_prob_dict)
        1    0.011    0.011    0.019    0.019 ic.py:1()
        1    0.011    0.011   51.297   51.297 couchdb_finding_keywords.py:68(find_keyword)
        8    0.008    0.001    0.008    0.001 :1(connect)
        1    0.007    0.007    0.066    0.066 urllib.py:1296(getproxies_internetconfig)
        1    0.005    0.005   51.228   51.228 couchdb_finding_keywords.py:41(all_word_count)
        1    0.003    0.003    0.069    0.069 urllib.py:1329(getproxies)
        1    0.003    0.003    0.003    0.003 Res.py:1()
        1    0.002    0.002    0.002    0.002 File.py:1()
        1    0.001    0.001    0.002    0.002 macostools.py:5()
        2    0.001    0.001    0.001    0.001 socket.py:229(close)
        2    0.001    0.000    0.002    0.001 httplib.py:659(connect)
        2    0.001    0.000    0.005    0.002 httplib.py:224(readheaders)
     11/6    0.001    0.000    0.001    0.000 sre_parse.py:385(_parse)
        1    0.000    0.000    0.000    0.000 ic.py:161(__init__)
        2    0.000    0.000    0.000    0.000 httplib.py:323(__init__)

>>>---- End profiling print

real	0m52.023s
user	0m0.732s
sys	0m0.437s</pre>

I’m no erlang expert but seeing that many socket calls makes me still suspect that some TCP level tuning (window size & buffering) might be helpful.

As a final note, I did a database compaction and reran the query which helped significantly compared to the worse case 0.9 time but at best only matched 0.8.

> <pre>time ./couchdb_finding_keywords.py
[('Hacker', 249160.0), ('Techcrunch', 249160.0)]

real	0m34.605s
user	0m0.687s
sys	0m0.394s</pre>

You can find [the test script](http://svn.wjhuie.com/public_sandbox/trunk/couchdb/finding_keywords/couchdb_finding_keywords.py), with changes to work with the slightly different view URL’s, and if you’d like to recreate the test all you will need to do (beyond setting up couchdb) is:

  1. Swap the comments on the last two lines to run “load_db()”
  2. Create a map / reduce wordcount view
  3. Change the “view_url” parameter on line 25
  4. Invert the comments again to just run “find_keyword()”
