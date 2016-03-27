---
id: 238
title: How to build Couchdb on Dreamhost
date: 2009-03-04T12:19:14+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=238
permalink: /2009/03/04/how-to-build-couchdb-on-dreamhost/
categories:
  - code
  - couchdb
---
As you know from many of my entries I&#8217;m a big fan of [couchdb](http://couchdb.apache.org/), and if you&#8217;re interested you should really be following [janl](http://jan.prima.de/plok/), [jchris](http://jchris.mfdz.com/) and [lethain](http://lethain.com/) as they push this technology forward.

As you might also guess from my earlier post I&#8217;m working to build and install it on [Dreamhost](http://dreamhost.com/), another thing I support enthusiastically.

Unfortunately, being on the outer fringe of technology meant I wasn&#8217;t able to get them to install it for me, but that&#8217;s completely understandable. Given that the current package release has no Auth support (I believe the repository builds do but that would have required more software installs) if I were supporting a multi-user production environment it might make me a little nervous too.

However, to in order to continue my interests it&#8217;s a major component so I wanted to give it a shot. I don&#8217;t have it up and running 100% right now (it appears to have run though I can&#8217;t connect) but I wanted to document to build side of things before I forgot 😀

So here&#8217;s the rundown;

I was fortunate to follow some [excellent](http://wiki.dreamhost.com/index.php/Django) [advice](http://jeffcroft.com/blog/2006/may/11/django-dreamhost/) about getting Django up and running on Dreamhost. It advised that you setup a &#8220;~/run&#8221; directory to install all your add-on software too and these steps below will build on that existing environment.

First you need to download some software, I needed to get; [Erlang](http://erlang.org/download/otp_src_R12B-5.tar.gz), [SpiderMonkey](http://ftp.mozilla.org/pub/mozilla.org/js/js-1.7.0.tar.gz), [ICU](http://download.icu-project.org/files/icu4c/4.0.1/icu4c-4_0_1-src.tgz) & [CouchDB](http://mirrors.kahuki.com/apache/incubator/couchdb/0.8.1-incubating/apache-couchdb-0.8.1-incubating.tar.gz).

I downloaded all my files into &#8220;~/repo&#8221; but wherever you like to store them will be fine (&#8220;~/software for example). Now create a temp directory and unpack everything, replacing the paths (and potentially filenames if you picked different versions) as appropriate.

> <pre>mkdir ~/tmp && cd ~/tmp
tar zxf ~/repo/otp_src*
tar zxf ~/repo/js-1.7.0*
tar zxf ~/repo/icu4c*
tar zxf ~/repo/apache*</pre>

I will show the build commands in the order I did them but as long as you save CouchDB for the final step (naturally) then I think you should be fine. Though it&#8217;s important to realize I didn&#8217;t get this right first time through so I did have some partial installs at times.

> <pre>cd js/src
make -f Makefile.ref BUILD_OPT=1 JS_DIST=$RUN
cp *.h $RUN/include/js
cd Linux_All_OPT.OPJ
cp jsproto.tbl jsautocfg.h $RUN/include/js
cp libjs.so $RUN/lib</pre>

Now for Erlang:

> <pre>cd ~/tmp/otp_src*
./configure --prefix=$RUN --enable-smp-support --enable-threads --enable-hipe
make && make install</pre>

Next, Unicode Support (ICU):

> <pre>cd ~/tmp/icu/source
./configure --prefix=$RUN
make && make install</pre>

And finally, CouchDB!

> <pre>./configure --prefix=$RUN --with-js-lib=$RUN/lib --with-js-include=$RUN/include/js --with-erlang=$RUN/lib/erlang/usr/include
make && make install</pre>

**Once that&#8217;s completed I was able to run &#8220;couchdb&#8221; and see the famous &#8220;Apache CouchDB has started. Time to relax.&#8221; !!!**

**Unfortunately, running &#8220;couchdb -s&#8221; in another window tells me that &#8220;Apache CouchDB is not running.&#8221; <img src="http://blog.thecapacity.org/wp-includes/images/smilies/frownie.png" alt=":(" class="wp-smiley" style="height: 1em; max-height: 1em;" />**

However, I suspect that&#8217;s an easier issue for Dreamhost to help me with then building everything from source!