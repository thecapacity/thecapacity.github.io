---
id: 29
title: My bad old-time-y-ness
date: 2007-12-10T21:45:56+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=29
permalink: /2007/12/10/my-bad-old-time-y-ness/
categories:
  - code
  - hacks
  - technology
---
I wanted to post something relatively quick tonight, so here&#8217;s a bit of poking I&#8217;ve been doing. I&#8217;ve yet to get anything interesting with bluetooth between my laptop and my iPhone. That and **I&#8217;m a pretty paranoid person so I keep the bluetooth turned off. However, I decided to leverage my paranoia to give me a reason to keep bluetooth on.** 

**So I did some poking around and came up with the shell script below&#8230;** yes, I know shell scripts are old-time-y, and I&#8217;m even more aware that my bash skills have slipped with age.

I&#8217;m working on some python code but rather then looking crufty and forgetful that will come off as clueless and juvenile so **we&#8217;re sticking with shell for now, because it works (which is all most shell scripts can claim anyway).**

**A few things that made this hard has been the evolution of dbus and the apps built on them**. There&#8217;s some interesting articles out there but most tell you how to implement a dbus service and not how to make calls to an existing one, i.e. **how in the world can I query dbus methods rather then just &#8220;sniffing&#8221; them out.**

The example in [this post](http://lists.freedesktop.org/archives/xdg/2006-June/006523.html "dbus example") incorrectly lists the locking target as `"org.gnome.ScreenSaver.setActive"` when it is actually ``I wanted to post something relatively quick tonight, so here&#8217;s a bit of poking I&#8217;ve been doing. I&#8217;ve yet to get anything interesting with bluetooth between my laptop and my iPhone. That and **I&#8217;m a pretty paranoid person so I keep the bluetooth turned off. However, I decided to leverage my paranoia to give me a reason to keep bluetooth on.** 

**So I did some poking around and came up with the shell script below&#8230;** yes, I know shell scripts are old-time-y, and I&#8217;m even more aware that my bash skills have slipped with age.

I&#8217;m working on some python code but rather then looking crufty and forgetful that will come off as clueless and juvenile so **we&#8217;re sticking with shell for now, because it works (which is all most shell scripts can claim anyway).**

**A few things that made this hard has been the evolution of dbus and the apps built on them**. There&#8217;s some interesting articles out there but most tell you how to implement a dbus service and not how to make calls to an existing one, i.e. **how in the world can I query dbus methods rather then just &#8220;sniffing&#8221; them out.**

The example in [this post](http://lists.freedesktop.org/archives/xdg/2006-June/006523.html "dbus example") incorrectly lists the locking target as `"org.gnome.ScreenSaver.setActive"` when it is actually`` also it is listed in the method list. The command `dbus-monitor` will help immensely, **and if someone can tell me why `^C` doesn&#8217;t work and I end up having to `^Z` and then `kill %1` that would be great**.

**Also the `Poke` method doesn&#8217;t initiate activity in such as way as to produce an &#8220;unlock&#8221; screen as you&#8217;d expect.**

**Another example of frustration is [this python sample](http://www.redhat.com/magazine/003jan05/features/dbus/ "RedHat on dbus") from RedHat which fails, i.e. the object returned by <code class="computeroutput">dbus.SystemBus() has no member get_service().</code>**

**I grew using KDE and although I run GNOME now I miss many KDE things, but I&#8217;m hopeful that dbus becomes a common communication system for all things on Linux**, although there are times when it feels like reinventing the wheel like why can&#8217;t network manager just use the standard `ifconfig` and `iwconfig` commands.

**However, I think I&#8217;ll save trying to figure that part out until after I get my python in shape.**

My apologies for the code formatting, I&#8217;m not sure how to do this cleanly in WP, however, I hope this helps someone out there!

> `<br />
#!/bin/bash`
> 
> \### You&#8217;ll need to change the 00:FF:FF:FF:FF:FF below to the bdaddr of your phone
> 
> lock_it=0
> 
> while true; do
> 
> jay_found=0
  
> TEST=\`/usr/bin/sudo /usr/bin/l2ping -s 1 -c1 -t 1 00:FF:FF:FF:FF:FF 2>/dev/null\`
  
> echo $TEST | grep &#8220;1 sent, 1 received&#8221; &> /dev/null && jay_found=1
> 
> if [ $jay_found -eq &#8220;1&#8221; ]; then
  
> echo &#8220;I found your phone&#8221;;
  
> sleep 10
  
> dbus-send &#8211;session &#8211;type=method_call &#8211;dest=org.gnome.ScreenSaver /org/gnome/ScreenSaver org.gnome.ScreenSaver.Poke boolean:true
  
> lock_it=0
  
> else
  
> if [ $lock_it -eq &#8220;0&#8221; ]; then
  
> echo &#8220;Not yet!&#8221;;
  
> lock_it=1
  
> sleep 30
  
> elif [ $lock_it -eq &#8220;1&#8221; ]; then
  
> echo &#8220;I think I should lock your screen&#8221;;
  
> \# dbus-send &#8211;session &#8211;type=method_call &#8211;dest=org.gnome.ScreenSaver /org/gnome/ScreenSaver org.gnome.ScreenSaver.Lock boolean:true
  
> lock_it=2
  
> else
  
> \# I don&#8217;t know that there&#8217;s a good alternative here
  
> sleep 60
  
> fi
  
> fi
  
> done