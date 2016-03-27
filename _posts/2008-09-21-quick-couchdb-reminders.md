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

Looking at a view for a specific database with your browser. In this case the database is &#8220;stock\_values&#8221; and the view is &#8220;all\_stocks&#8221; (yes you need the &#8220;_view&#8221; and the &#8220;all&#8221; part of it);

http://192.168.1.99:5984/stock\_values/\_view/all_stocks/all

I was really getting tripped up by figuring out the corresponding python version which would be;

for ro in db.view(&#8216;all_stocks/all&#8217;): print ro.id