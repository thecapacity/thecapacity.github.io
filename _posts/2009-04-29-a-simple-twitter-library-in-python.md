---
id: 294
title: A simple twitter library in python
date: 2009-04-29T09:34:37+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=294
permalink: /2009/04/29/a-simple-twitter-library-in-python/
categories:
  - code
  - python
---
I’ve been working on a project built on [Google App Engine](http://code.google.com/appengine/) and I’m relying on [twitter](http://twitter.com/) to mediate some of the interaction with my end users.

What I find great about the growing prevalence of social interfaces is that I don’t have to focus predominately on coding an interface and with so many clients my users can interact with in whatever way is most appropriate, i.e. from a mobile phone or a desktop client.

Unfortunately, the standard [python-twitter library](http://code.google.com/p/python-twitter/) doesn’t readily run under GAE because of some library [issues](http://code.google.com/p/python-twitter/issues/detail?id=59). Originally, I was looking at providing some code changes for it but it’s a spiderweb of more abstractions then I think the problem deserves.

In the process of building my own library I found out that [Avinash](http://avinashv.net/) had [figured out](http://avinashv.net/2008/04/python-and-twitter/) how to setup the authentication properly so I built upon his work and added a few other functions I needed.

We all know about twitter’s growing popularity so I thought I’d share [my version](http://svn.wjhuie.com/public_sandbox/trunk/python/jtwitter.py) as well in case it proved helpful to anyone. Twitter provides a great mechanism to decouple your interface from your backend code and I hope to see many more smart systems to come!
