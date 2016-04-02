---
id: 76
title: 'python & couchdb sample'
date: 2008-07-04T17:15:05+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=76
permalink: /2008/07/04/python-couchdb-sample/
categories:
  - code
  - couchdb
  - python
---
Lacking any good examples of how to use python’s couchdb module, I’ve managed to make pretty impressive progress (for me) on a 4th of July holiday.

I’ll try to recreated it here for others although I know it’ll be incomplete.

Consider it a syntactical example;

> import couchdb
  
> s = couchdb.Server(‘http://localhost:5984/’) ##why can’t it default to this?
  
> db = s[‘stock_values’]
  
> ids = []
  
> stock_values = {}
  
> for doc in db:
> 
> > ids.append(doc)
  
> > d = db[doc]
  
> > stock\_values[d[‘symbol’]] = d[‘historical\_data’]

More good examples are in the code;

http://code.google.com/p/couchdb-python/source/browse/trunk/couchdb/client.py?r=61

