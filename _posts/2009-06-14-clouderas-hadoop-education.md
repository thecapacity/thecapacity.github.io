---
id: 291
title: 'Cloudera&#8217;s Hadoop Education'
date: 2009-06-14T13:43:50+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=291
permalink: /2009/06/14/clouderas-hadoop-education/
categories:
  - code
  - mapreduce
  - opensource
  - python
---
A while back, after Cloudera released their [lectures](http://www.cloudera.com/hadoop-training) and [VMware image](http://www.cloudera.com/hadoop-training-virtual-machine) for [Hadoop](http://hadoop.apache.org/core/), I watched the training sessions and worked through some of the initial exercises.

I must say I was a little disappointed by the videos but I believe that&#8217;s because I&#8217;d seen <span>Christophe Bisciglia&#8217;s lectures when he was still at Google.<br /> </span>

However, the exercises are definitely something to get you thinking and are worth giving a shot. It&#8217;s sort of like &#8216;[programming golf](http://stackoverflow.com/questions/534608/python-golf-whats-the-most-concise-way-of-turning-this-list-of-lists-into-a-dic)&#8216; and I thought I&#8217;d share my version of the first map function vs. the packaged solution.

Here&#8217;s my map function

> <pre>import sys, re
WORDS = re.compile(r'(\w+)')
PARSER = re.compile('(.+?)\t(.+?)\n')

for input in sys.stdin.readlines():
m = PARSER.match(input)
if m:
    key = m.groups()[0]
    for word in WORDS.findall(m.groups()[1]):
        print "%s\t%s" % (word, key)</pre>

Cloudera&#8217;s version is:

> <pre>import re
import sys
NONALPHA = re.compile("\W")

for input in sys.stdin.readlines():
    keyline = input.split("\t", 1)
    if (len(keyline) == 2):
        (key, line) = keyline
        for w in NONALPHA.split(line):
            if w:
                print w + "\t" + key</pre>

By definition they should produce the same output, i.e. the mappings should be identical, and barring buggy corner cases mine certainly passed the test.

What I found interesting was my instinctual desire to let regexps do the work, whereas their version relies on a simple &#8220;split()&#8221; to sort the input. It&#8217;s likely a faster solution and given the massive amounts of data for large data passes, it&#8217;s worth benchmarking.

However, although I&#8217;m clearly biased, I must admit I found mine easier to grok and should be more flexible, e.g. perhaps the input pattern could become a parameter rather then hard-coded into the flow.

There&#8217;s certainly not a &#8220;right&#8221; way to do it, other then one that works. The advantage of the MapReduce model is that the necessary code is often really really short and easy to modify but I thought others might find it interesting to realize that perl doesn&#8217;t have an exclusive license on &#8216;[TMTOWTDI](http://catb.org/~esr/jargon/html/T/TMTOWTDI.html)&#8216;