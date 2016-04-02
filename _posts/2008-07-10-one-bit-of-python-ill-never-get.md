---
id: 94
title: "One bit of Python I'll never get"
date: 2008-07-10T16:44:58+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=94
permalink: /2008/07/10/one-bit-of-python-ill-never-get/
categories:
  - code
  - frustration
  - python
---

```python
>>> "hello"[2:4]
  
> "ll"
  
>>> "hello"[2:]
  
> "llo"
  
>>> "hello"[:4]
  
> "hell"
```

<span style="text-decoration: underline;">Do any of those not make sense to you?</span> **Here’s how I see it, and my confusion;**

“hello” represents a 5 character string… and indexing starts at 0, something most programmers are all familiar with. The “[” “]” characters are used to subscript (or index) the string (which is actually a list of characters).

So;

```python
>>> "hello"[2:4]

> "ll"
  
>>> "hello"[0]
  
"h"
  
>>> "hello"[2]
  
> "l"
  
>>> "hello"
  
> "o"
  
>>> "hello"[-1]
  
> "o"
```

Get it? The 0th character is "h", 2nd is "l" (the 1st one) and the 4th (or -1 i.e. 1 from the end) character is "o". The ":" in between the "[]" is used to separate out the various indexing parameters.

Holistically, it's actually referred to as “[slicing](http://www.diveintopython.org/native_data_types/lists.html)“. So [1:2] says slice using indexes 1 and 2 (sorta… wait for the punchline below), and you can also say [0:20:2] which will slice in ‘steps’ of 2!!

It's a handy technique, but let’s return to our example for the rub… Notice how “hello”[2:4] gave us "ll" and not characters 2,3 & 4!!! **That’s because the slice <span style="text-decoration: underline;">includes</span> the 1st index but <span style="text-decoration: underline;">excludes</span> the second!**

Man I hate that…

