---
id: 139
title: Quick couchdb reminders
date: 2008-09-21T21:02:03+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=139
permalink: /2008/09/21/quick-couchdb-reminders/
categories:
  - code
  - couchdb
  - python
---
Using your browser to access the admin interface;

http://192.168.1.99:5984/_utils/

Looking at a view for a specific database with your browser. In this case the database is “stock\_values” and the view is “all\_stocks” (yes you need the “_view” and the “all” part of it);

http://192.168.1.99:5984/stock\_values/\_view/all_stocks/all

I was really getting tripped up by figuring out the corresponding python version which would be;

for ro in db.view(‘all_stocks/all’): print ro.id
