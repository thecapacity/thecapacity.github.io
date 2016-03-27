---
id: 346
title: couchdb coming back for more
date: 2009-10-30T22:34:24+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=346
permalink: /2009/10/30/couchdb-coming-back-for-more/
categories:
  - couchdb
---
Not that long ago, JChris [pointed out](http://blog.thecapacity.org/2009/03/19/couchdb-performance-or-use-a-file/#comment-1473) that not only was there a new version of couchdb out but that [Janl](http://twitter.com/Janl) had released a new version of his OSX package, [CouchDBX](http://janl.github.com/couchdbx/)!!

So I knew I needed to find a time to try both new versions out.

&#8216;Thankfully&#8217;, I can&#8217;t really get to sleep right now so I thought I&#8217;d try to be productive and give them both a go again with my small [performance test](http://blog.thecapacity.org/2009/05/31/couchdb-performance-too-much-tcp/).

And here&#8217;s the latest results.

Here&#8217;s a baseline, which if you recall loads the file from disk.

<pre>$time ./finding_keywords.py
[('Hacker', 249160.0), ('Techcrunch', 249160.0)]

real    0m0.259s
user    0m0.216s
sys    0m0.041s</pre>

Now for couchdb&#8217;s results. Here&#8217;s the portion of time required for the database load:

<pre>$ time ./couchdb_finding_keywords.py
real	16m53.912s
user	2m57.409s
sys	1m35.209s</pre>

This is down quite substantially from the 28 minutes the last version tested took to load.

Rather then run the timing for the loading stage again (since it&#8217;s clearly way beyond the time required to analyze the text file), I thought I&#8217;d jump to an actual query.

Unfortunately in the process of running the real test I realized I hadn&#8217;t created the necessary views for the new database.

Then, in doing so, I made a typo in my map() function and had to wait through many, many error messages like:

<pre>OS Process :: function raised exception (ReferenceError: worse is not defined) with doc._id ############</pre>

This was certainly my fault, but it would be nice if couch could take a break from spitting out error messages and not bake my processor any further running a bad map()!

I finally was able to click off the temporary view page and found the &#8220;Stop&#8221; button.

I managed to get most of my view function squared away but then missed the quotes around the dictionary key &#8220;word&#8221;, so while it should have read:

<pre>"map": function(doc) {
    emit(doc[\"word\"], 1);
}

"reduce": function(key, value, rereduce) {
    if (rereduce) {
        return sum(value);
    }
    else {
      return value.length;
    }
}</pre>

It didn&#8217;t and the bad line came out as:

<pre>emit(doc[word], 1);</pre>

So as you can imagine, I had to do the dance all over again. This time, after I was able to stop it I went directly to the document for the design itself and edited the code there.

I know I hit the green arrow to save, but when I went back to the design view to see the results it still had the same mistake. So I corrected it there, and quickly hit &#8216;Save&#8217; and then couchdbx promptly crashed on me.

After I told OSX to restart it I got:

<pre>"The application beam.smp quit unexpectedly after it was relaunched"</pre>

So yes&#8230; sometimes software and I don&#8217;t get along. What can I say, but that it makes me a great tester!

I was able to restart couchdbx though, and it seemed to load fine, and eventually got data from a browser after the view was built.

But I also got an interesting tidbit from the DBX console too:

<pre>1&gt; [info] [&lt;0.66.0&gt;] 127.0.0.1 - - 'GET' /_config/native_query_servers/ 200
1&gt; [info] [&lt;0.86.0&gt;] checkpointing view update at seq 92542 for keywords _design/finding
1&gt; [error] [&lt;0.69.0&gt;] Uncaught error in HTTP request: {exit,normal}
1&gt; [info] [&lt;0.69.0&gt;] Stacktrace: [{mochiweb_request,send,2},
             {couch_httpd,send_chunk,2},
             {couch_httpd_view,send_json_reduce_row,3},
             {couch_httpd_view,'-make_reduce_fold_funs/5-fun-1-',8},
             {couch_btree,reduce_stream_kv_node2,8},
             {couch_btree,reduce_stream_kp_node2,11},
             {couch_btree,fold_reduce,7},
             {couch_httpd_view,'-output_reduce_view/6-fun-0-',12}]
1&gt; [info] [&lt;0.81.0&gt;] 127.0.0.1 - - 'GET' /keywords/_design/finding/_view/word_count?group=true 200
1&gt;</pre>

Yep, I think I broke it yet again&#8230;

A subsequent query to:

<pre>http://localhost:5984/keywords/_design/finding/_view/word_count?group=true</pre>

Seemws to show all&#8217;s well, so I thought I&#8217;d get fancy:

<pre>wget -O - http://localhost:5984/keywords/_design/finding/_view/word_count?group=true</pre>

But when I hit Control-C to cancel the get (because I realized I hadn&#8217;t redirected output to /dev/null) I got yet another stack trace:

<pre>1&gt; [info] [&lt;0.126.0&gt;] 127.0.0.1 - - 'GET' /keywords/_design/finding/_view/word_count?group=true 304
1&gt; [error] [&lt;0.387.0&gt;] Uncaught error in HTTP request: {exit,normal}
1&gt; [info] [&lt;0.387.0&gt;] Stacktrace: [{mochiweb_request,send,2},
             {couch_httpd,send_chunk,2},
             {couch_httpd_view,send_json_reduce_row,3},
             {couch_httpd_view,'-make_reduce_fold_funs/5-fun-1-',8},
             {couch_btree,reduce_stream_kv_node2,8},
             {couch_btree,reduce_stream_kp_node2,11},
             {couch_btree,fold_reduce,7},
             {couch_httpd_view,'-output_reduce_view/6-fun-0-',12}]</pre>

So let&#8217;s just get on with the performance test I guess&#8230;

After another DBX restart (more to be sure then anything since couchdb seems to almost enjoy dumping stack traces while still merrily marching along).

I changed my URLs:
  
old\_url u = &#8220;http://localhost:5984/%s/\_view/finding/word\_count&#8221; % (db\_name)

new\_url = &#8220;http://localhost:5984/%s/\_design/finding/\_view/word\_count&#8221; % (db_name)

And can now officially tell you (after one more stack traces) that:

<pre>$ time ./couchdb_finding_keywords.py
[('Hacker', 249160.0), ('Techcrunch', 249160.0)]

real	0m31.559s
user	0m0.730s
sys	0m0.429s</pre>

**It&#8217;s still an impressive bit of performance for the functionality, and I think I&#8217;ve clearly shown it&#8217;s fault resistance. I just wish it didn&#8217;t come at more than 100 times the cost of the flat file.**