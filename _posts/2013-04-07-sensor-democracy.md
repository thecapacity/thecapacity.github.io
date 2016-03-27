---
id: 414
title: Sensor Democracy
date: 2013-04-07T12:49:08+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=414
permalink: /2013/04/07/sensor-democracy/
categories:
  - uncategorized
---
Hot on the heels of talking with my friend Kevin about [Hot on the heels of talking with my friend Kevin about](https://www.tambascio.org/kevin/health/exercising-for-data-with-fitbit/ "training for data") is a revelation by my new favorite fitness company [Wahoo](http://www.wahoofitness.com/) makers of fitness sensors, devices and apps.

One of the challenges for any geek who wants to go beyond commercial products for their fitness regimen is the need to understand all the complicated protocols of sensors, namely Bluetooth and ANT+.

I recently acquired a [biking cadence and speed sensor](http://store.apple.com/us/product/H9752VC/A/wahoo-blue-sc-speed-and-cadence-sensor-iphone) to match my heart rate monitor and realized that the [app](https://itunes.apple.com/us/app/fisica-fitness/id391599899?mt=8) has a hidden gem of a feature!

Under the _User Profile_ settings is a featured called _Air Broadcaster_, which states it _Broadcast[s] your data live over Wi-Fi_!!!

It must not be well known within the company, because their support email had no idea what I was talking about, but I was lucky enough to stumble upon the developer [via twitter](https://twitter.com/wahoofitness) who gave me some code, enough to figure out how it works!

There are two quick and easy ways to get access to this data, the first using our trusty friend [netcat](http://netcat.sourceforge.net/):

<pre>nc -w0 -dulk 51530</pre>

You should see some data such as:

<pre>{"HeartrateInstant":0,"LapRunningTimeString":"01:36","CadenceInstantString":"0","HeartbeartBitmap":"GBg8PH5+////////f/4//B/4D/AH4APAAYAAAAAAAAA=","HeartrateInstantString":"0","SpeedInstant":0,"DeviceName":"&lt;YOURPHONE_NAME&gt;","Email":"","DistanceWorkout":0,"BikePowerInstant":0,"WorkoutRunningTimeString":"02:35","CadenceInstant":0,"WorkoutState":"Running"}</pre>

 _Note, this was me testing it from my couch and not engaged in any physical activity, so the only sensor it had was GPS._ 

The second using some [simple python](https://gist.github.com/thecapacity/5331464):

<pre>#!/usr/bin/python

import socket
import json

UDP_IP = "0.0.0.0"
UDP_PORT = 51530

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM) # UDP
sock.bind((UDP_IP, UDP_PORT))

while True:
    data_str, addr = sock.recvfrom(1024) # buffer size is 1024 bytes
    data = json.loads(data_str)
    print addr, " -&gt; ", data</pre>

&nbsp;

Why is this cool?

**This means I can spend my time working with my data, not trying to recreate a sensor bridge!
  
** 

I&#8217;ll still have to spend some time figuring out what `HeartbeartBitmap` represents, but that&#8217;s (hopefully) trivial to reverse engineering how to connect to the sensor and it&#8217;s own protocol.