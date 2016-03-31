---
id: 94
title: 'One bit of Python I&#8217;ll never get&#8230;'
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
&#8220;

```python
>>> "hello"[2:4]
  
> "ll"
  
>>> "hello"[2:]
  
> "llo"
  
>>> "hello"[:4]
  
> "hell"
```

<span style="text-decoration: underline;">Do any of those not make sense to you?</span> **Here&#8217;s how I see it, and my confusion;**

&#8220;hello&#8221; represents a 5 character string&#8230; and indexing starts at 0, something most programmers are all familiar with. The &#8220;[&#8221; &#8220;]&#8221; characters are used to subscript (or index) the string (which is actually a list of characters).

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

Get it? The 0th character is &#8216;h&#8217;, 2nd is &#8216;l&#8217; (the 1st one) and the 4th (or -1 i.e. 1 from the end) character is &#8216;o&#8217;. The &#8216;:&#8217; in between the &#8220;[]&#8221; is used to separate out the various indexing parameters.

Holistically, it's actually referred to as &#8220;[slicing](http://www.diveintopython.org/native_data_types/lists.html)&#8220;. So [1:2] says slice using indexes 1 and 2 (sorta&#8230; wait for the punchline below), and you can also say [0:20:2] which will slice in &#8216;steps&#8217; of 2!!

It's a handy technique, but let&#8217;s return to our example for the rub&#8230; Notice how &#8220;hello&#8221;[2:4] gave us &#8216;ll&#8217; and not characters 2,3 & 4!!! **That&#8217;s because the slice <span style="text-decoration: underline;">includes</span> the 1st index but <span style="text-decoration: underline;">excludes</span> the second!**

Man I hate that&#8230;
