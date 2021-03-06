---
id: 398
title: Trying to run Processing on Mac
date: 2012-07-08T23:22:33+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=398
permalink: /2012/07/08/trying-to-run-processing-on-mac/
categories:
  - frustration
---
So this is just a quick post as a breadcrumb for anyone with similar problems.

I’ve been trying to run Processing (after a long hiatus) on my MacBook, and was having no luck.

It would throw an error message like the following:

<pre>Exception in thread "Thread-7" java.lang.Error: Unable to launch target VM: 

java.io.IOException: Cannot run program "java": error=2, No such file or directory
	at processing.mode.java.runner.Runner.launchVirtualMachine(Runner.java:317)
	at processing.mode.java.runner.Runner.launch(Runner.java:118)
	at processing.mode.java.JavaMode$1.run(JavaMode.java:181)
	at java.lang.Thread.run(Thread.java:680)</pre>

I’d remembered a java problem before on Mac / OSX, and thought it was Processing, but a friend tipped me off that it was actually an Arduino issue.

For better or worse, they use the same “ide” environment, so that lead me in the right direction towards; <http://arduino.cc/en/Guide/Troubleshooting#toc2>

What’s strange, is that that doesn’t seem to be an option any longer for the latest Arduino install.

However, so it is an option on the latest Processing… but it didn’t help…

But it clued me in on some ideas and ultimately this trick; `"arch -i386 /Applications/Processing.app/Contents/MacOS/JavaApplicationStub"`

Allows it to run, although with errors like:

<pre>Jul  9 00:18:28 loki java[1933] : CGContextGetCTM: invalid context 0x0
Jul  9 00:18:28 loki java[1933] : CGContextSetBaseCTM: invalid context 0x0
Jul  9 00:18:28 loki java[1933] : CGContextGetCTM: invalid context 0x0
Jul  9 00:18:28 loki java[1933] : CGContextSetBaseCTM: invalid context 0x0</pre>

It’s still better than nothing!

I suspect it’s related to using Apple’s version of Java. I had allowed Apple to remove whatever version I had installed, since I hadn’t using Processing/Arduino, so when it needed java again I let it install the default.

It pains me to say it, but I may have to try Oracle’s version tomorrow….

