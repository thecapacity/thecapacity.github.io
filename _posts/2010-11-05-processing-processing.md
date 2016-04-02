---
id: 368
title: 'Processing… Processing'
date: 2010-11-05T14:35:46+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=368
permalink: /2010/11/05/processing-processing/
categories:
  - code
  - visualization
---
Like most people I’ve been bitten by the “social networks take up all my blogging time” bug.

But in reality, I think that’s OK. I once read someone who said SMS is becoming the polite way of conversing, as opposed to a phone call, because it permits the recipient to respond when it’s convenient for them as opposed to when it’s convenient for the initiator.

So blogging, remains a ‘slow and steady’ system to support the hivemind; Convenient for the author, and the searcher.

With that in mind here’s a bit of code I wrote a while back to create a [Processing](http://processing.org/) visualization.

I’ve loved the ‘clock’ charts where data is presented around a circle, from 1 o’clock to 12 o’clock, but never had a solid reason to use one. Unlike the [Sunlight Foundation’s](http://sunlightfoundation.com/) excellent example [visualizing transparency](http://blog.sunlightfoundation.com/2009/03/06/transparency-visualized/).

However, ‘recently’ (in blogger time) I had a unique opportunity at work to apply the technique to visualizing how participants interacted with a site over time.

Here’s what I came up with:

<div id="attachment_369" style="width: 310px" class="wp-caption aligncenter">
  <a href="http://blog.thecapacity.org/wp-content/uploads/2010/11/visual.png"><img class="size-medium wp-image-369" title="Processing Visualization" src="http://blog.thecapacity.org/wp-content/uploads/2010/11/visual-300x200.png" alt="'clock' chart of activity" width="300" height="200" srcset="http://blog.thecapacity.org/wp-content/uploads/2010/11/visual-300x200.png 300w, http://blog.thecapacity.org/wp-content/uploads/2010/11/visual-1024x682.png 1024w, http://blog.thecapacity.org/wp-content/uploads/2010/11/visual.png 1200w" sizes="(max-width: 300px) 100vw, 300px" /></a>
  
  <p class="wp-caption-text">
    User Website Activity
  </p>
</div>

This represents how often users interacted with our site during it’s availability period (which spanned a number of days).

I first created the data with Python, taking the time portion of each action and creating a count, which I recorded East Coast (blue) and West Coast (orange) time. You’ll notice that the data is exactly the same in intensity, simply shifted by the appropriate timezone. This is because we had no way of knowing where the users were connecting from and wanted to simply look at the overall data to discern any patterns (and my brain doesn’t have a timezone converter built in).

Looking at the data, it should be fairly obvious that:

  * An “East Coast” interepretation meant that users interacted with the site either as the first thing during their day (i.e. 8am) or during the latter part of lunch (noon and 1:00 PM).
  * Meanwhile, “West Coast” users probably responded before lunch (11) or towards the end of the day.

I was able to build upon [a great example](http://blog.blprnt.com/blog/blprnt/processing-json-the-new-york-times) from Jer Thorp and I apologize greatly for butchering his solution. Thankfully I didn’t need anything near as complex (and didn’t have to fight Java for JSON data since I embedded my arrays directly in the code).

So in case it helps anyone else, here’s my solution with all its warts (e.g. I know the data keys are drawn twice and there’s some manual tweaking to the drawHeight for sizing):

<pre><span style="color: #cc6600;">import</span> processing.opengl.*;

<span style="color: #cc6600;">int</span> maxVal = 0;           <span style="color: #7e7e7e;">//Keeps track of the maximum returned value over the all terms</span>
<span style="color: #cc6600;">int</span> localMax = 0;         <span style="color: #7e7e7e;">//Keeps track of the maximum returned value over the each term</span>
<span style="color: #cc6600;">float</span> drawHeight = 0.45;  <span style="color: #7e7e7e;">//Portion of the screen height that the largest bar takes up</span>
<span style="color: #7e7e7e;">//.45 for hours .65 for dates</span>
<span style="color: #cc6600;">float</span> drawWidth = .25;  <span style="color: #7e7e7e;">//Portion of the screen height that the largest bar takes up</span>

<span style="color: #cc6600;">int</span> center_size = 10;
<span style="color: #cc6600;">int</span> border = 1;           <span style="color: #7e7e7e;">//Border between bars</span>
<span style="color: #cc6600;">int</span> lastTotal = 0;        

<span style="color: #cc6600;">color</span>[] colours = { #400101, #E46D0A, #009BF1 }; <span style="color: #7e7e7e;">//Graph Colours</span>
<span style="color: #cc6600;">color</span> textColor1 = #D98D30;
<span style="color: #cc6600;">color</span> textColor = #333333;
<span style="color: #cc6600;">color</span> backColor = #F2F2F2;
<span style="color: #cc6600;">color</span> curColor;

<span style="color: #cc6600;">int</span> MAX_HEIGHT = 1200;
<span style="color: #cc6600;">int</span> MAX_WIDTH = 800;

<span style="color: #7e7e7e;">// Time Shifted Data built with a separate Python script </span>
<span style="color: #cc6600;">String</span>[] Hours = {<span style="color: #006699;">"00:00"</span>, <span style="color: #006699;">"01:00"</span>, <span style="color: #006699;">"02:00"</span>, <span style="color: #006699;">"03:00"</span>, <span style="color: #006699;">"04:00"</span>, <span style="color: #006699;">"05:00"</span>, <span style="color: #006699;">"06:00"</span>, <span style="color: #006699;">"07:00"</span>, <span style="color: #006699;">"08:00"</span>, <span style="color: #006699;">"09:00"</span>, <span style="color: #006699;">"10:00"</span>, <span style="color: #006699;">"11:00"</span>, <span style="color: #006699;">"12:00"</span>, <span style="color: #006699;">"13:00"</span>, <span style="color: #006699;">"14:00"</span>, <span style="color: #006699;">"15:00"</span>, <span style="color: #006699;">"16:00"</span>, <span style="color: #006699;">"17:00"</span>, <span style="color: #006699;">"18:00"</span>, <span style="color: #006699;">"19:00"</span>, <span style="color: #006699;">"20:00"</span>, <span style="color: #006699;">"21:00"</span>, <span style="color: #006699;">"22:00"</span>, <span style="color: #006699;">"23:00"</span>};

<span style="color: #cc6600;">int</span>[] mod_est_Activity_Hours_Values = {3, 0,0,0,0,0,0, 3, 7, 9, 10, 26, 4, 7, 6, 20, 38, 10, 1, 5, 3, 2, 1, 1};
<span style="color: #cc6600;">int</span>[] mod_pdt_Activity_Hours_Values = {0,0,0,0, 3, 7, 9, 10, 26, 4, 7, 6, 20, 38, 10, 1, 5, 3, 2, 1, 1,3, 0,0};
<span style="color: #7e7e7e;">// end generated Data</span>

<span style="color: #cc6600;">void</span> <span style="color: #cc6600;"><strong>draw</strong></span>() {

};

<span style="color: #cc6600;">void</span> <span style="color: #cc6600;"><strong>setup</strong></span>() {
  <span style="color: #cc6600;">PFont</span> font = <span style="color: #cc6600;">loadFont</span>(<span style="color: #006699;">"Meta-Normal-48.vlw"</span>);
  <span style="color: #cc6600;">textFont</span>(font);

  <span style="color: #7e7e7e;">//Set the size of the stage & set the background</span>
  <span style="color: #cc6600;">size</span>(MAX_HEIGHT,MAX_WIDTH);
  <span style="color: #006699;">frameRate</span>(60);
  <span style="color: #cc6600;">background</span>(backColor);
  <span style="color: #cc6600;">smooth</span>();

  curColor = colours[1];
  drawData(Hours, mod_est_Activity_Hours_Values, Hours.<span style="color: #cc6600;">length</span>);

  curColor = colours[2];
  drawData(Hours, mod_pdt_Activity_Hours_Values, Hours.<span style="color: #cc6600;">length</span>);

  <span style="color: #cc6600;">save</span>(<span style="color: #006699;">"visual.png"</span>);
};

<span style="color: #cc6600;">void</span> drawKey(<span style="color: #cc6600;">String</span> <span style="color: #006699;">key</span>, <span style="color: #cc6600;">float</span> xinc, <span style="color: #cc6600;">float</span> theta) {
      <span style="color: #cc6600;">float</span> y = 0;
      <span style="color: #cc6600;">float</span> x = .5;
      <span style="color: #cc6600;">String</span> s = <span style="color: #006699;">key</span>;

      <span style="color: #cc6600;">pushMatrix</span>();
      <span style="color: #cc6600;">translate</span>(x + xinc/2 * drawWidth, y - (xinc * 2) * drawHeight);

      <span style="color: #7e7e7e;">//Draw key</span>
      <span style="color: #cc6600;">rotate</span>(-<span style="color: #006699;">PI</span>/2);
      <span style="color: #cc6600;">fill</span>(textColor);

      <span style="color: #cc6600;">textSize</span>(<span style="color: #cc6600;">max</span>(xinc/2, 13));
      <span style="color: #cc6600;">text</span>(s, 0, 0);

      <span style="color: #cc6600;">translate</span>(<span style="color: #cc6600;">cos</span>(theta), <span style="color: #cc6600;">sin</span>(theta));

      <span style="color: #cc6600;">popMatrix</span>();
};

<span style="color: #cc6600;">void</span> drawData(<span style="color: #cc6600;">String</span>[] keys, <span style="color: #cc6600;">int</span>[] data, <span style="color: #cc6600;">int</span> len) {
  parseData(keys, data, len);

  <span style="color: #cc6600;">fill</span>(curColor);
  <span style="color: #cc6600;">float</span> xinc = <span style="color: #cc6600;">float</span>(<span style="color: #006699;">width</span>)/len;

  <span style="color: #7e7e7e;">//Move to the center of the screen</span>
  <span style="color: #cc6600;">pushMatrix</span>();
  <span style="color: #cc6600;">translate</span>(<span style="color: #006699;">width</span>/2, <span style="color: #006699;">height</span>/4);
  <span style="color: #cc6600;">noStroke</span>();

  <span style="color: #7e7e7e;">//Draw each value as a bar</span>
  <span style="color: #cc6600;">for</span> (<span style="color: #cc6600;">int</span> i = 0; i < len; i++) {
    <span style="color: #cc6600;">color</span> c = <span style="color: #cc6600;">color</span>(<span style="color: #cc6600;">red</span>(curColor), <span style="color: #cc6600;">green</span>(curColor), <span style="color: #cc6600;">blue</span>(curColor), <span style="color: #cc6600;">random</span>(100,255));
    <span style="color: #cc6600;">fill</span>(c);

    <span style="color: #cc6600;">float</span> h = <span style="color: #cc6600;">float</span>(data[i])/<span style="color: #cc6600;">float</span>(maxVal);
    <span style="color: #cc6600;">float</span> theta = i * (<span style="color: #006699;">PI</span> / (len/2));

    <span style="color: #cc6600;">translate</span>(<span style="color: #cc6600;">cos</span>(theta) * center_size, <span style="color: #cc6600;">sin</span>(theta) * center_size);

    <span style="color: #7e7e7e;">//Rotate</span>
    <span style="color: #cc6600;">pushMatrix</span>();
    <span style="color: #cc6600;">rotate</span>(theta);

    <span style="color: #7e7e7e;">//Draw the bar</span>
    <span style="color: #cc6600;">rect</span>(0, 0, xinc * drawWidth, -h * <span style="color: #006699;">height</span> * drawHeight);

    drawKey(keys[i], xinc, theta);

    <span style="color: #cc6600;">popMatrix</span>();
  };

  <span style="color: #cc6600;">popMatrix</span>(); <span style="color: #7e7e7e;">//back to original center</span>
};

<span style="color: #7e7e7e;">// Helps normalize the chart</span>
<span style="color: #cc6600;">void</span> parseData(<span style="color: #cc6600;">String</span>[] keys, <span style="color: #cc6600;">int</span>[] data, <span style="color: #cc6600;">int</span> len) {
  <span style="color: #cc6600;">for</span> (<span style="color: #cc6600;">int</span> i = 0; i < len; i++) {
    <span style="color: #cc6600;">if</span> (data[i] > localMax) {
      localMax = <span style="color: #cc6600;">min</span>(data[i], 3000);
      <span style="color: #cc6600;">if</span> (localMax > maxVal) maxVal = localMax;
    };
  };
};
</pre>

To make it work you’ll need to make sure the font’s installed (see Jer’s original tutorial) but it should be relatively straightforward after that.

Cheers!
