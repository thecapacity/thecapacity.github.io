---
id: 76
title: 'python &#038; couchdb sample'
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
Lacking any good examples of how to use python&#8217;s couchdb module, I&#8217;ve managed to make pretty impressive progress (for me) on a 4th of July holiday.

I&#8217;ll try to recreated it here for others although I know it&#8217;ll be incomplete.

Consider it a syntactical example;

> import couchdb
  
> s = couchdb.Server(&#8216;http://localhost:5984/&#8217;) ##why can&#8217;t it default to this?
  
> db = s[&#8216;stock_values&#8217;]
  
> ids = []
  
> stock_values = {}
  
> for doc in db:
> 
> > ids.append(doc)
  
> > d = db[doc]
  
> > stock\_values[d[&#8216;symbol&#8217;]] = d[&#8216;historical\_data&#8217;]

More good examples are in the code;

http://code.google.com/p/couchdb-python/source/browse/trunk/couchdb/client.py?r=61