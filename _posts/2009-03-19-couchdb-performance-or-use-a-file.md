---
id: 247
title: CouchDB Performance or Use a File
date: 2009-03-19T21:49:20+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=247
permalink: /2009/03/19/couchdb-performance-or-use-a-file/
categories:
  - code
  - couchdb
  - python
---
If this is the first post you&#8217;ve read from my blog you should probably go check some others and assert for yourself that I&#8217;m a big fan of [couchDB](http://couchdb.apache.org/).

Even if it wasn&#8217;t easy to be [impressed](http://www.infoq.com/presentations/katz-couchdb-and-me) by Damon Katz&#8217;s, it would be hard to overlook the [interest](http://www.google.com/trends?q=couchdb) his code has created. If even those miracles weren&#8217;t enough for you, then just look to [what](http://lethain.com/entry/2008/dec/08/full-text-search-in-couchdb-using-couchdb/) [other](http://jchrisa.net/drl/_design/sofa/_list/index/recent-posts?descending=true&limit=5) [amazing](http://jan.prima.de/plok/) [minds](http://www.davispj.com/) [have](http://www.cmlenz.net/archives/2008/08/view-source) [done](http://vmx.cx/cgi-bin/blog/index.cgi/geocouch-geospatial-queries-with-couchdb:2008-10-26:en,CouchDB,Python,geo). Finally, for the truly skeptical there&#8217;s now a [business](http://couch.io/) you can contact.

There are quite a few things that make it an amazing piece of engineering, which includes its simplicity of purpose, something you don&#8217;t often get a chance to appreciate these days. I&#8217;m a big fan of pipes, lots of individual pieces doing their dedicated task, and to me the MapReduce model epitomizes that behavior.

Still, there&#8217;s a lot I&#8217;ve taken on faith, since I haven&#8217;t been able to dedicate weeks to it&#8217;s internals instead trying to leverage it for projects. One of those traits has been the assumption of [performance](http://damienkatz.net/2007/12/couchdb_perform_1.html).

Truly, it&#8217;s [more then just an assumption](http://damienkatz.net/2007/12/couchdb_roundup.html). [Reports](http://iamseanmurphy.com/2008/09/08/couchdb-view-generation/) are that couchDB&#8217;s performance is already [quite decent](http://userprimary.net/user/2007/12/16/a-quick-look-at-couchdb-performance/) and it&#8217;s not even been tuned, so I&#8217;ve never attempted to benchmark it&#8217;s behavior.

Instead I&#8217;ve been working on some language processing code for my wife. Learning about NLP has aligned with my A.I. background, although it&#8217;s reminded me about all the math I&#8217;ve forgotten!

And reading [samples](http://digitalhistoryhacks.blogspot.com/2006/08/easy-pieces-in-python-word-frequencies.html) and feeling like I&#8217;d gotten my legs underneath me I decided to &#8220;port&#8221; [a nice little example](http://uswaretech.com/blog/2009/03/finding-keywords-using-python/) over to couchDB. If you want to play along at home then you&#8217;ll want to check out the article and grab his code (and [keywords2.txt](http://svn.wjhuie.com/public_sandbox/trunk/couchdb/finding_keywords/keywords2.txt) file).

Although [the code](http://svn.wjhuie.com/public_sandbox/trunk/couchdb/finding_keywords/finding_keywords.py) is geared more for education then performance it still runs fairly snappy on my laptop, running with some pretty consistent times;

> <pre>time ./finding_keywords.py
[('Hacker', 249160.0), ('Techcrunch', 249160.0)]

real	0m0.286s
user	0m0.228s
sys	0m0.047s</pre>

I&#8217;ve also run it with some [performance sampling](http://amix.dk/blog/viewEntry/19407) but let&#8217;s stick with simple timing for now.

There&#8217;s about 124,580 &#8220;words&#8221; in the text file;

> <pre>&gt;&gt;&gt; key_file = open("keywords2.txt")
&gt;&gt;&gt; data = key_file.read()
&gt;&gt;&gt; words = data.split()
&gt;&gt;&gt; len(words)
124580</pre>

This data is then used to create a word frequency count and is generated each time the program is run, not a bad 0.2 seconds worth of work!

Naturally, having static text data and supplementing this original data with some derived &#8220;data structures&#8221; (like total count, or a view showing each word and the number of times it appears), is a perfect case for couchDB.

So I decided to simply load this data right into couchDB. Here&#8217;s how I did this, skipping details like creating the database itself;

> <pre>def load_db():
    key_file = open('keywords2.txt')
    data = key_file.read()
    words = data.split()
    for word in words:
        node = db.create( { "word": word } )</pre>

You&#8217;d expect this to take some time, databases provide valuable services but of course can only do so at the expense of some cycles. However, I was surprised to find out this took almost _30 minutes!_

> <pre>time ./couchdb_finding_keywords.py 

real	27m2.356s
user	2m35.921s
sys	1m14.478s</pre>

Based on the low user and sys times you can guess most of the delay is due to transport overhead, i.e. network communication. This is all going to a couchdb running on localhost, a MacBook with 4G RAM and an Intel 2.0 GHz Core 2 Duo, so it&#8217;s a bit surprising but not really critical.

I didn&#8217;t bother running this three times and taking an average. The keywords2.txt file should already be in memory having been read by the file backed example. Nor is upfront cost a big consideration for me, I&#8217;m willing to spend the time once especially if it can save me work on the backend!

So naturally I was pretty excited port things over to a more couchDB / pythonic example and here&#8217;s what I came up with. After you load your data you then need a view, which you can get from [my previous post](http://blog.thecapacity.org/2009/03/13/can-a-couchdb-guru-please-explain-this/), along with jChris&#8217; helpful comment. Note, if this is your first time with this stuff (or even if it isn&#8217;t) you may want to practice on a smaller database first!!

Next we&#8217;ll need some code to get this data, and while I highly recommend the [fantastic couchdb-python library](http://code.google.com/p/couchdb-python/) for the rest of my examples I&#8217;ll use JSON & urllib to remove a layer of indirection.

Here&#8217;s how we can get the overall word count (used to calculate relative frequencies);

> <pre>def total_word_count(word):
    try:
        u = "http://localhost:5984/%s/_view/finding/word_count" % (db_name)
        j = simplejson.loads(urllib.urlopen(u).read())
        # Sample Output: {"rows":[{"key":null,"value":19}]}
        return j['rows'][0]['value']
    except:
        return 0</pre>

We can do the same thing with &#8220;?group=true&#8221; in our URL to get the individual words each with their respective count. Here&#8217;s some code and a contrived bit of output to serve as our sample;

> <pre>def all_word_count():
    try:
        u = "http://localhost:5984/%s/_view/finding/word_count?group=true" % (db_name)
        ### Example Output: {"rows":[{"key":"be","value":1},{"key":"do","value":4},{"key":"to","value":1},{"key":"we","value":2}]}
        j = json.loads(urllib.urlopen(u).read())
        return j['rows']
    except:
        return [{}]</pre>

Now what is a bit problematic from this (vs the original example) is that we&#8217;re actually getting a long list of dictionaries instead of one dictionary, but we can convert this to a full word frequency dictionary and end up on equal footing again all at the same time.

> <pre>def build_prob_dict(word_list, total_words):
    num = float(total_words)
    try:
        return dict([ (r['key'], r['value'] / num ) for r in word_list])
    except:
        return {}</pre>

So that should get us the rest of the way. Here&#8217;s the relevant excerpt [from the new script](http://svn.wjhuie.com/public_sandbox/trunk/couchdb/finding_keywords/couchdb_finding_keywords.py) vs the [original](http://svn.wjhuie.com/public_sandbox/trunk/couchdb/finding_keywords/finding_keywords.py):

> <pre>def find_keyword(test_string = None):
    if not test_string:
        test_string = 'Hacker news is a good site while Techcrunch not so much'
    word_prob_dict = build_prob_dict(all_word_count(), total_word_count())
    non_exist_prob = min(word_prob_dict.values()) / 2.0
    #... everything blow should function unchanged</pre>

OK, so how does this fair? Well let&#8217;s give it a try;

> <pre>time ./couchdb_finding_keywords.py
[('Hacker', 249160.0), ('Techcrunch', 249160.0)]
real    0m33.878s
user    0m0.692s
sys    0m0.408s</pre>

**Ouch&#8230; this is after the view had been generated**, by multiple calls (and thus cached), by couchDB. If you look at some more detailed numbers you can see that the bulk of the delay is again spend in socket calls. **Even downloading the view results via wget is painful at ~ 11.8 KB/s vs. ~163 MB/s when serving a static file with the results via apache.**

Here&#8217;s an interesting tidbit from a more detailed profiling;

> <pre>ncalls  tottime  percall  cumtime  percall      filename:lineno(function)
      2     32.437   16.218   32.437  16.218        socket.py:278(read)</pre>

I know the team has not focused on tuning couchDB, and I&#8217;ve read lots of anecdotal evidence that erlang is fast for computation especially on multicore systems, but **my hope is they can get the transport layer working quickly as well**!

As a final curiosity **I&#8217;d love it if couchDB supported queries from STDIN**!! Think about the piping fun you could have you could insert couchDB as part of your bash pipe! I also wouldn&#8217;t have to worry about adding another network server to my hosted service!

**Did I mess up here? Can someone try this and tell me if they get similar results?**