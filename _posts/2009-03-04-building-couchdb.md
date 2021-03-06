---
id: 232
title: Building CouchDB
date: 2009-03-04T11:01:20+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=232
permalink: /2009/03/04/building-couchdb/
categories:
  - code
  - couchdb
---
This is a quick technical post for posterity, if you’re not interested I won’t be offended if you leave now.

I’m working to try to get couchdb on my hosting provider, Dreamhost. They’ve been a great service for me so far, but understandably this isn’t something they’re yet ready to support. So rather then a few apt-get’s I’m building it from source, which means erlang and spidermonkey.

I’ve got those two dependencies build (I hope) but I kept hitting an error with couchdb’s configure script complaining that it couldn’t find the libraries, despite me setting;

> <pre>./configure --prefix=$RUN --with-js-lib=/home/wjhuie/run/lib/ --with-js-include=/home/wjhuie/run/include/js</pre>

Note, since I don’t have root or sudo access that I use a ~/run directory;

> <pre>export RUN=/home/wjhuie/run</pre>

This is so I can isolate my installed software from the system stuff. In configuring couchdb it was able to find libjs.so but complained about not being able to find jsapi.h. In the end the error isn’t that it couldn’t find it but that the header wasn’t able to compile successfully.

I searched far and wide with out much luck (and having seen others with similar problems) but in the end I realized I was missing some required files. Since the instructions from the couchdb wiki weren’t very useful;

http://wiki.apache.org/couchdb/Installing_SpiderMonkey

I had followed this, more helpful, advice;

http://avidemux.org/admWiki/index.php?title=Compile_SpiderMonkey

However, the commands listed needs to be modified slightly to include two files generated during the build, jsproto.tbl and jsautocfg.h;

So for the record (in case it helps someone else someday) I built my spidermonkey like this;

> <pre>make -f Makefile.ref BUILD_OPT=1 JS_DIST=$RUN</pre>

Then installed it with;

> <pre>cp *.h /usr/include/js
cd Linux_All_OPT.OPJ
cp jsproto.tbl jsautocfg.h $RUN/include/js
cp libjs.so $RUN/lib</pre>

A good way to get a hint on the problem is to create a sample “program” and try to compile it. To do this, create a file called “test.c” with the contents;

> <pre>#include </home/wjhuie/run/include/jsapi.h>
void main()
{ }</pre>

Then try to compile it with;

> <pre>make test 2>&1 | less</pre>

That should allow you to see what’s going on and once that works then couchdb’s configure test should also! Now I have to sort out the unicode library requirements!

