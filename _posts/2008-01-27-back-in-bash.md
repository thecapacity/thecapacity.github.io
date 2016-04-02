---
id: 40
title: Back in Bash
date: 2008-01-27T15:08:25+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/2008/01/27/back-in-bash/
permalink: /2008/01/27/back-in-bash/
categories:
  - code
  - opensource
---
Quick little post today,

_Using my previous two scripts you’ve (1) been able to separate a massive photo directory into 3rds (2) been able to take one of those directories and identify matching photos and move them into directories of their own, to make it easier to compare the results._

_Now you’ve got a bunch of directories, most of which have 1 photo in them… what do you do._

I decided to take a break from Python for this one, it just didn’t seem the appropriate hammer for this nail.

So here’s a bit of bashing for you, I have an itch somewhere that makes me believe you can stat a directory to determine how many files are in it, but in this case find is the cheap date I was looking for.

_After this script, which I call consolidate.sh, you’ll be left with only the directories which have duplicates, Enjoy!_

> ``<br />
#!/bin/bash</p>
<p>for e in `ls`; do<br />
if [ -d $e ]; then<br />
total_files=$(find $e -type f | wc -l)<br />
if [ $total_files -le 1 ]; then<br />
find $e -type f -exec mv {} !/uniques \;<br />
rmdir $e<br />
fi<br />
fi<br />
done``

