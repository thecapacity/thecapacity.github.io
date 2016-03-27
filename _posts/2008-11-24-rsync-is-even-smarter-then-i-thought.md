---
id: 166
title: rsync is even smarter then I thought!
date: 2008-11-24T21:02:07+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=166
permalink: /2008/11/24/rsync-is-even-smarter-then-i-thought/
categories:
  - sysprog
---
I love rsync and feel like I use it a lot but it took a friend of mine to teach me another lesson in the power of this tool! I think one of the reasons social networks have taken off is that they enable you to quickly poll a pool of active brain trust prior to hitting up google.

Even better this was a case of me expressing my frustration (via a facebook status message) and a friend proactively reacting to my discomfort!

My situation was this; I bought my wife one of the new macbooks&#8230; and how does that lead to rsync you might wonder? Well it&#8217;s complicate but it&#8217;s all about iTunes&#8230;

Our first experience with Apple and iTunes was when I bought a nano and a Nike+ system for her running. She has (soon that will be past tense) the only Windows system in the house (I run Linux) and she happily and blissfully (which means I wasn&#8217;t involved) loaded up all our music, which was a collection of years of random MP3&#8217;s.

Eventually, after I saw how neat the Nike+ system was, got my own nano and transmitter and since I didn&#8217;t know how iTunes would react to multiple devices I created a second user on her system and imported the same set of songs.

Now, give me a bit of credit&#8230; what I tried to do was point my iTunes to the same directory where her files were and hope that just the *.xml files would be different.

However, either I got this wrong or iTunes is silly (I&#8217;m leaning towards the latter) but it wasn&#8217;t the stellar setup I expected. Even worse, was that I had cleaned up all the files (Genre&#8217;s and so forth) and it didn&#8217;t reflect in her iTunes&#8230;.

Now there we were, happy iPod users, and we eventually expanded our experience to include two new iPhones! When we activated them we used her iTunes (I wasn&#8217;t sure if our &#8220;Family Plan&#8221; would work with multiple iTunes). That&#8217;s when I learned iTunes is mostly ok with multiple users and multiple devices (except when it comes to podcasts).

So why rsync, and what does this have to do with her macbook? Well I decided to import &#8220;my&#8221; version of all the mp3&#8217;s since the tags and duplicates were mostly cleaned up, however since then she&#8217;d purchased songs and imported more music.

Thus, even after I&#8217;d imported a &#8220;clean&#8221; set of base files I still had to find a way to import her library, just minus the ones that had already been imported.

Thus my cry of pain&#8230;

The magic? I won&#8217;t bore you with mounting directories over the network, etc&#8230; but the magic line is this;

rsync -avhmP &#8211;ignore-existing &#8211;compare-dest=/home/itunes/iTunes Music/ /mnt/chaos/Documents and Settings/Administrator/My Documents/My Music/iTunes/iTunes Music /dump/remaining_iTunes/

Where &#8220;~iTunes&#8221; is the subset of &#8220;clean&#8221; files, &#8220;/mnt/chaos/&#8230;.&#8221; is a cifs mount of her &#8220;dirty&#8221; files, and &#8220;remaining_iTunes&#8221; is where the difference b/t the two directories should go.

It&#8217;s not 100% completed yet but I think it&#8217;s working perfectly!

Thanks friends on the intertubes!