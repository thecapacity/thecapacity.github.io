---
id: 339
title: Skinned Programming Paradigms
date: 2009-10-19T10:57:27+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=339
permalink: /2009/10/19/skinned-programming-paradigms/
categories:
  - code
  - frustration
  - inspiration
---
Here&#8217;s a free thought for you.

**How much of people choice in programming languages is really syntax dependent?**

For example, I dislike Java (I hate it for other reasons) simply because of the verbosity of &#8216;System.out.println&#8217; and don&#8217;t really understand why Scala would chose &#8216;println&#8217; instead of Python&#8217;s terse use of &#8216;print&#8217;.

And I&#8217;m pretty sure despite overt rationalizations like &#8216;saving myself keystrokes&#8217; that&#8217;s just a petty reason.

**However, what I learned in compiler construction is that the parser or tokenizer is really separate from the language itself.** 

So, for example, there&#8217;s no reason there couldn&#8217;t be a plugin for Java that allowed me to write with python&#8217;s syntax, or vice versa. Such a technique might require a little bit of library support, but I suspect adding pythons &#8216;map()&#8217; even to C/C++ would be fairly trivial.

**We should be able to &#8216;skin&#8217; our languages with our syntax of choice regardless of the underlying compiler, JVM or bytecode.**

If this were possible, then &#8216;language wars&#8217; could be less about syntax and interface (a la emacs vs. vi) and more about the underlying value of the language itself.

**If we can theme operating systems and user interfaces, then why not programming languages?**