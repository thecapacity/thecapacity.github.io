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

My situation was this; I bought my wife one of the new macbooks… and how does that lead to rsync you might wonder? Well it’s complicate but it’s all about iTunes…

Our first experience with Apple and iTunes was when I bought a nano and a Nike+ system for her running. She has (soon that will be past tense) the only Windows system in the house (I run Linux) and she happily and blissfully (which means I wasn’t involved) loaded up all our music, which was a collection of years of random MP3’s.

Eventually, after I saw how neat the Nike+ system was, got my own nano and transmitter and since I didn’t know how iTunes would react to multiple devices I created a second user on her system and imported the same set of songs.

Now, give me a bit of credit… what I tried to do was point my iTunes to the same directory where her files were and hope that just the *.xml files would be different.

However, either I got this wrong or iTunes is silly (I’m leaning towards the latter) but it wasn’t the stellar setup I expected. Even worse, was that I had cleaned up all the files (Genre’s and so forth) and it didn’t reflect in her iTunes….

Now there we were, happy iPod users, and we eventually expanded our experience to include two new iPhones! When we activated them we used her iTunes (I wasn’t sure if our “Family Plan” would work with multiple iTunes). That’s when I learned iTunes is mostly ok with multiple users and multiple devices (except when it comes to podcasts).

So why rsync, and what does this have to do with her macbook? Well I decided to import “my” version of all the mp3’s since the tags and duplicates were mostly cleaned up, however since then she’d purchased songs and imported more music.

Thus, even after I’d imported a “clean” set of base files I still had to find a way to import her library, just minus the ones that had already been imported.

Thus my cry of pain…

The magic? I won’t bore you with mounting directories over the network, etc… but the magic line is this;

rsync -avhmP –ignore-existing –compare-dest=/home/itunes/iTunes Music/ /mnt/chaos/Documents and Settings/Administrator/My Documents/My Music/iTunes/iTunes Music /dump/remaining_iTunes/

Where “~iTunes” is the subset of “clean” files, “/mnt/chaos/….” is a cifs mount of her “dirty” files, and “remaining_iTunes” is where the difference b/t the two directories should go.

It’s not 100% completed yet but I think it’s working perfectly!

Thanks friends on the intertubes!

