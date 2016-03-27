---
id: 26
title: Code of Conduct
date: 2007-12-02T12:45:53+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=26
permalink: /2007/12/02/code-of-conduct/
categories:
  - opensource
  - social
  - technology
---
[Brad](http://stickmanindc.blogspot.com/ "Stickan's blog") has [some great comments](http://stickmanindc.blogspot.com/2007/12/thoughts-on-integration-convergence.html "Brad's comments on data locality") on [my post](http://blog.thecapacity.org/?p=24 "Software and Data Locality") about **how software needs to extend beyond a single locality**. I&#8217;ve yet to be happy with the &#8220;comment&#8221; structure of the blog&#8217;o&#8217;sphere, so I&#8217;ll follow [Brad&#8217;s example](http://blog.thecapacity.org/?p=24#comments "Posting vs. Commenting") and put my thoughts here, especially because Brad&#8217;s post and mine evolve the topic(s) rather then being follow-ups.

n particular, **I like Brad&#8217;s insight that it&#8217;s not really an application, but the overall data that matters. It&#8217;s also liberating to realize that all data you&#8217;re interested in (and allowed to see) is &#8220;your data&#8221;.**

Privacy might dictate differently but **if I&#8217;m allowed to keep it in my brain then I believe Brad&#8217;s right that I&#8217; allowed to keep it &#8220;on disk&#8221;**. For example, when a friend updates status on Facebook, that change is their data, but the alert (with the same data) which I receive is my data and I should be able to store it and do with it what I will.

In the enterprise world, we talk about **being &#8220;data centric&#8221;** as opposed to the &#8220;infrastructure centric&#8221; approach of the past. Sometimes there are more pejorative terms like &#8220;enterprise data bus&#8221; used but they all refer to **the goal of making the right data accessible to the right end-points (people or applications) at the right time, while ensuring requirements like consistency, audit-ability and performance are met.**

Satisfying these demands usually represents two perspectives (1) centralize everything or (2) accept the fractional nature of data and try to embrace an appropriate level of policy. For example you may state &#8220;ok, for this application you can use whatever data you want but you&#8217;ll probably want to try to update every 30 min&#8221;

In reality it&#8217;s really always a mixture of the two, especially given compliance requirements. Whatever the ultimate solutions, even beginning the conversation is usually instructive enough to help business better define, and ultimately solve, these access issues.

So can we use Brad&#8217;s realization to inspire an &#8220;end consumer, SaaS using, RESTful application&#8221; bill of rights? We need a better name, but just as there are rules of behavior for participating in forums or etiquette for blogging and linking, **I believe we should require applications to live up to a required set of expectations,** perhaps we can get one of the opensource entities to help with defining, auditing and certifying such applications.

An initial standard must require &#8220;import/export&#8221;. However, I don&#8217;t think that captures what we&#8217;re after. **There needs to be unmodified event reporting**, and [not the Facebook styile of &#8220;something&#8217;s changed come visit to check it out&#8221;](http://informationweek.com/shared/printableArticle.jhtml?articleID=204203573 "Cory Doctorow on Facebook"). If we&#8217;re going to expect an application to report on state changes (presumably reporting these to another application which can act on that state) then **we must also require an external mechanism for inducing state changes**. Continuing to use Facebook as a model I believe it&#8217;s reasonable to expect an API for &#8220;friending&#8221; or changing status.

**What other requirements should services we use be encouraged to provided?**