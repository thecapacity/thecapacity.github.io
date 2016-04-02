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
Here’s a free thought for you.

**How much of people choice in programming languages is really syntax dependent?**

For example, I dislike Java (I hate it for other reasons) simply because of the verbosity of ‘System.out.println’ and don’t really understand why Scala would chose ‘println’ instead of Python’s terse use of ‘print’.

And I’m pretty sure despite overt rationalizations like ‘saving myself keystrokes’ that’s just a petty reason.

**However, what I learned in compiler construction is that the parser or tokenizer is really separate from the language itself.** 

So, for example, there’s no reason there couldn’t be a plugin for Java that allowed me to write with python’s syntax, or vice versa. Such a technique might require a little bit of library support, but I suspect adding pythons ‘map()’ even to C/C++ would be fairly trivial.

**We should be able to ‘skin’ our languages with our syntax of choice regardless of the underlying compiler, JVM or bytecode.**

If this were possible, then ‘language wars’ could be less about syntax and interface (a la emacs vs. vi) and more about the underlying value of the language itself.

**If we can theme operating systems and user interfaces, then why not programming languages?**

