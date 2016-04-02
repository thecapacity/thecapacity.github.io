---
id: 240
title: Could a couchdb guru explain this, please?
date: 2009-03-13T18:15:29+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=240
permalink: /2009/03/13/can-a-couchdb-guru-please-explain-this/
categories:
  - code
  - couchdb
  - frustration
---
I’m in the process of trying to build (and benchmarking) a couchdb project and I decided to use some word count & frequency samples as data. Since “word count” and “grep” are the quintessential map/reduce examples I thought this would be fairly simple.

However, couchdb doesn’t seem to be following the expected semantics.

Let’s say I’ve got some data, here’s how it looks in python;

> <pre>>>> import couchdb
>>> s = couchdb.Server()
>>> db = s['kw2']
>>> for d in db: print db[d]
...
<Document '133da883092e206d7191f81661beb813'@'3188228489' {'word': 'ho'}>
<Document '2287406943e627278d98a3a2f3d3483b'@'634745217' {'word': 'do'}>
<Document '2717deb4df8ba09601166021fb758126'@'2083376980' {'word': 'mo'}>
<Document '38d48e8e069538a55902dd2d2b7e1771'@'2475366164' {'word': 'ho'}>
<Document '39ef4a9e3eb0eeb02d483ce658d08356'@'2904312995' {'word': 'hi'}>
<Document '4237064ad7a89fa11e9bbbc8ca4ed302'@'722283984' {'word': 'do'}>
<Document '4d0e61dedaf2af93a9d4d261cab696de'@'996995145' {'word': 'we'}>
<Document '55ba96501ed1e9573b2cb6e647c35b47'@'3153984663' {'word': 'my'}>
<Document '5be13ca69c76d202b131d50f5b9c1ecb'@'1584030189' {'word': 'do'}>
<Document '612e4a0d32f4c91f7fb2414e4de47845'@'3488016124' {'word': 'be'}>
<Document '61426c868dc388e6edb2b4ce2078ce06'@'2761346180' {'word': 'me'}>
<Document '908acaf4ad704951dbb08d27ddfbe9a9'@'941727127' {'word': 'mo'}>
<Document '9136e093fda2dda7d5585983299fcbc7'@'4166962206' {'word': 'mo'}>
<Document '9decb25944110c04d040feb31e532c78'@'1016718857' {'word': 'do'}>
<Document 'ad7f4aab329d55c3a2fb97390df5ae0a'@'1660663052' {'word': 'my'}>
<Document 'c4d976a789e37e1c3eb4d57bd50d47aa'@'923287257' {'word': 'my'}>
<Document 'cccf15515077d100498573fe40244130'@'3846996388' {'word': 'hi'}>
<Document 'd747a88eb2cb18776237852aceff96fc'@'3596694550' {'word': 'we'}>
<Document 'dc115f5d42d442f0b5e7d3680aeb62c2'@'3446491946' {'word': 'to'}></pre>

Feel free to add your own but that’s what I’ve got. Each doc has a simple structure, an “_id” (supplied by couchdb when the document is created) and an element called “word” which obviously contains some fabricated two letter structures (which I hesitate to actually call words).

What’s important to note is that the same word may appear in multiple documents.

Now we want to build a view to show each word as well as the sum of how many times it appears in our database.

Again, following the classic paradigm we build our map function (in javascript) as such;

> <pre>function(doc) {
  emit(doc["word"], 1);
}</pre>

So far so good, now reduce;

> <pre>function(key, value, rereduce) {
   if (rereduce) {
      return sum(value);
   }
   else {
      return value.length;
   }
}</pre>

You can pretty much ignore the “rereduce” clause as our dataset’s not big enough right now, nor are we updating it. However, I will mention explain the function’s trick which is that while sum(value) is actually the “mathematically correct” action to take regardless of whether this is our first time through, we’re relying on the fact that since we’re emitting a “1” for each key (i.e. each word instance) that the sum of those values is simply the length of the array we’re passed in. [I learned this from [one of the masters](http://jchris.mfdz.com/posts/106)]

Ok, despite the attempt at “premature optimization” this actually seems to work out, or at least it looks to when shown in the couchdb key/value view. Here’s my screenshot for proof;

[<img class="aligncenter size-medium wp-image-243" title="picture-21" src="http://blog.thecapacity.org/wp-content/uploads/2009/03/picture-21-300x151.png" alt="picture-21" width="450" height="226" srcset="http://blog.thecapacity.org/wp-content/uploads/2009/03/picture-21-300x151.png 300w, http://blog.thecapacity.org/wp-content/uploads/2009/03/picture-21-1024x516.png 1024w" sizes="(max-width: 450px) 100vw, 450px" />](http://blog.thecapacity.org/wp-content/uploads/2009/03/picture-21.png)

However, what I see from a direct URL query to this view is markedly different then the data that’s represented. To test this either use Firefox or a command line client like curl and go to the following url;

> <pre>http://localhost:5984/kw2/_view/finding/word_count</pre>

What I see (and I suspect you will as well) is

> <pre>{"rows":[{"key":null,"value":19}]}</pre>

**_Which seems to break our expected key/value pairing!!!_**

Suspecting my understanding of couchdb’s map/reduce representation has been occluded by all the Google videos I’ve watched, it seems like an intuitive modification might be to change our reduce function to return the key & and the value, like this;

> <pre>return [key, value];</pre>

However, that yields an even more shocking outcome;

> <pre>{"rows":[{"key":null,"value":[[["we","d747a88eb2cb18776237852aceff96fc"],["we","4d0e61dedaf2af93a9d4d261cab696de"],["to","dc115f5d42d442f0b5e7d3680aeb62c2"],["my","c4d976a789e37e1c3eb4d57bd50d47aa"],["my","ad7f4aab329d55c3a2fb97390df5ae0a"],["my","55ba96501ed1e9573b2cb6e647c35b47"],["mo","9136e093fda2dda7d5585983299fcbc7"],["mo","908acaf4ad704951dbb08d27ddfbe9a9"],["mo","2717deb4df8ba09601166021fb758126"],["me","61426c868dc388e6edb2b4ce2078ce06"],["ho","38d48e8e069538a55902dd2d2b7e1771"],["ho","133da883092e206d7191f81661beb813"],["hi","cccf15515077d100498573fe40244130"],["hi","39ef4a9e3eb0eeb02d483ce658d08356"],["do","9decb25944110c04d040feb31e532c78"],["do","5be13ca69c76d202b131d50f5b9c1ecb"],["do","4237064ad7a89fa11e9bbbc8ca4ed302"],["do","2287406943e627278d98a3a2f3d3483b"],["be","612e4a0d32f4c91f7fb2414e4de47845"]],19]}]}</pre>

Of course I’m still baffled as to why we seem to have no entry set for key and all our rows as values.

However, my larger concern is beyond even that perplexing situation_;_

**_What’s most surprising here is that the key we’re being passed_ _includes the doc id even though it was not emitted as part of our map phase!_** 

Let’s give it one last go here, thinking perhaps we need to be more explicit;

> <pre>function(key, value, rereduce) {
   if (rereduce) {
      return sum(value);
   }
   else {
      return {"key": key[0],"value": value.length};
   }
}</pre>

Unfortunately, this seems to still not yield the organized rows we expected and returns;

> <pre>{"rows":[{"key":null,"value":{"key":["we","d747a88eb2cb18776237852aceff96fc"],"value":19}}]}</pre>

Which stands in high contrast to what couchdb continues to show us;

[<img class="aligncenter size-medium wp-image-244" title="picture-11" src="http://blog.thecapacity.org/wp-content/uploads/2009/03/picture-11-300x149.png" alt="picture-11" width="464" height="230" />](http://blog.thecapacity.org/wp-content/uploads/2009/03/picture-11.png)

So whatever we emit from reduce ends up as the value part of the reply (as indexed by “value”). Which matches our original expectation (that couchdb will handles setting this based) but doesn’t explain why it’s “null”.

In short I’m left with three questions;

1) Why does couchdb pass our reduce function the doc ID, when it’s not emitted in the map phase!

2) Why is “key” null in our output?

3) How do we get our JSON output to match the same pretty key/value representation that couchdb shows?

I wish I could promise that if you tune in next time I’ll have the answers but we’ll have to rely on the good nature of our experts out there to help us out.
