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
[Brad](http://stickmanindc.blogspot.com/ "Stickan's blog") has [some great comments](http://stickmanindc.blogspot.com/2007/12/thoughts-on-integration-convergence.html "Brad's comments on data locality") on [my post](http://blog.thecapacity.org/?p=24 "Software and Data Locality") about **how software needs to extend beyond a single locality**. I’ve yet to be happy with the “comment” structure of the blog’o’sphere, so I’ll follow [Brad’s example](http://blog.thecapacity.org/?p=24#comments "Posting vs. Commenting") and put my thoughts here, especially because Brad’s post and mine evolve the topic(s) rather then being follow-ups.

n particular, **I like Brad’s insight that it’s not really an application, but the overall data that matters. It’s also liberating to realize that all data you’re interested in (and allowed to see) is “your data”.**

Privacy might dictate differently but **if I’m allowed to keep it in my brain then I believe Brad’s right that I’ allowed to keep it “on disk”**. For example, when a friend updates status on Facebook, that change is their data, but the alert (with the same data) which I receive is my data and I should be able to store it and do with it what I will.

In the enterprise world, we talk about **being “data centric”** as opposed to the “infrastructure centric” approach of the past. Sometimes there are more pejorative terms like “enterprise data bus” used but they all refer to **the goal of making the right data accessible to the right end-points (people or applications) at the right time, while ensuring requirements like consistency, audit-ability and performance are met.**

Satisfying these demands usually represents two perspectives (1) centralize everything or (2) accept the fractional nature of data and try to embrace an appropriate level of policy. For example you may state “ok, for this application you can use whatever data you want but you’ll probably want to try to update every 30 min”

In reality it’s really always a mixture of the two, especially given compliance requirements. Whatever the ultimate solutions, even beginning the conversation is usually instructive enough to help business better define, and ultimately solve, these access issues.

So can we use Brad’s realization to inspire an “end consumer, SaaS using, RESTful application” bill of rights? We need a better name, but just as there are rules of behavior for participating in forums or etiquette for blogging and linking, **I believe we should require applications to live up to a required set of expectations,** perhaps we can get one of the opensource entities to help with defining, auditing and certifying such applications.

An initial standard must require “import/export”. However, I don’t think that captures what we’re after. **There needs to be unmodified event reporting**, and [not the Facebook styile of “something’s changed come visit to check it out”](http://informationweek.com/shared/printableArticle.jhtml?articleID=204203573 "Cory Doctorow on Facebook"). If we’re going to expect an application to report on state changes (presumably reporting these to another application which can act on that state) then **we must also require an external mechanism for inducing state changes**. Continuing to use Facebook as a model I believe it’s reasonable to expect an API for “friending” or changing status.

**What other requirements should services we use be encouraged to provided?**

