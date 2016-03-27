---
id: 135
title: Google Chrome fails the Google incognito test
date: 2008-09-09T13:18:26+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=135
permalink: /2008/09/09/google-chrome-fails-the-google-incognito-test/
categories:
  - frustration
  - Google
  - security
---
There&#8217;s been [a lot of talk](http://news.google.com/news?q=google+chrome&ie=UTF-8&oe=utf-8&rls=org.mozilla:en-US:official&client=firefox-a&um=1&hl=en&sa=X&oi=news_group&resnum=1&ct=title) about Google&#8217;s new [Chrome](http://www.google.com/chrome) [browser](http://googleblog.blogspot.com/2008/09/fresh-take-on-browser.html). If you haven&#8217;t [checked it out](http://www.google.com/chrome/intl/en/features.html) I&#8217;d recommend it from a &#8220;neat&#8221; factor but it&#8217;s less practical then upgrading to [Firefox 3](http://www.mozilla.com/en-US/firefox/).

Chrome is fast and [has some great features](http://www.google.com/googlebooks/chrome/) and one which I was excited about was an ability to go &#8220;[incognito](http://www.google.com/googlebooks/chrome/small_22.html)&#8220;. Going incognito will prevent the browser from storing cookies or you browsing history and is supposed to isolate the window as a completely separate &#8220;island&#8221; of web presence which is then &#8220;thrown away&#8221; when you close the window.

Google&#8217;s example was that when shopping you don&#8217;t want your significant other to stumble across your surprise. Although I saw suggestions of \*cough\* _other_ places you could browse where less repercussions might be welcome. You can recognize this mode by the little White Spy icon from the &#8220;[spy vs spy](http://www.myfreewallpapers.net/cartoons/pages/spy-vs-spy.shtml)&#8221; series.

**However, the site I most wanted to visit with completely private windows was [Gmail!](https://gmail.google.com/)** I don&#8217;t think I&#8217;m rare in having multiple email accounts and the challenge with Google is that they only let you be logged in to one account across all your sessions. While there [are techniques](https://addons.mozilla.org/en-US/firefox/addon/1320) which can mitigate this, I end up letting email languish because I don&#8217;t want to go through the &#8211; log out, log in, log out, and log back in as my primary ID &#8211; dance.

**So having multiple concurrently active Gmail tabs seemed like an obvious use of incognito mode!** 

<span style="text-decoration: underline;">Alas, it&#8217;s of course not to be;</span>

First, I created an incognito window and then logged into Gmail. So far so good, however **when you open a second tab and log in with a different ID it logs you out of the first tab!** That doesn&#8217;t seem to &#8220;isolated&#8221; does it?

My second thought was to create a second incognito window (since Google hasn&#8217;t been clear about the level of isolation). I noticed that this option is grayed out in the incognito window. If you go to your original &#8220;public&#8221; window and select &#8220;New incognito window&#8221; the options exists but simply opens another tab on the original incognito window (which still fails the &#8220;multiple login&#8221; test).

**Obviously, this lack of true isolation surprises to me.** <span style="text-decoration: underline;">Cookies appear to be shared across tabs and it appears you&#8217;re forced into having only one private window at time!</span> This would be awful if you were browsing multiple sites looking for a great shopping deal, but didn&#8217;t want them to know about other sites or if you were a web tester trying to isolate cookies from test runs.

Chrome&#8217;s a work in progress and Google&#8217;s [opensourceed the project](http://code.google.com/chromium/), so I can only hope someone will address these concerns. However, in the meantime it pays to test your expectations and if Google really [wants to make webapps more like desktop apps](http://www.google.com/googlebooks/chrome/small_24.html) I think this needs to be addressed.