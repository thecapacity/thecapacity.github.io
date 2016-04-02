---
id: 168
title: 'Training Neural Nets with CouchDB – part 1'
date: 2008-12-01T13:52:57+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=168
permalink: /2008/12/01/training-neural-nets-with-couchdb-part-1/
categories:
  - code
  - couchdb
  - python
---
My goal for this post is a bit technical and I’ll try warping both an artificial neural net, as well as my biological one, around an exploration of CouchDB, so read on if appropriate to your interests.

As you may have noticed in some of my earlier posts I’ve been playing with couchdb but it wasn’t until I started following [Janl](http://twitter.com/janl) and reading [some of his blog posts](http://jan.prima.de/~jan/plok/index.php?url=archives/108-Programming-CouchDB-with-Javascript.html&serendipity[csuccess]=moderate) that things started to click into place.

I would be remiss if I didn’t also mention [the fantastic Eric Florenzano](http://www.eflorenzano.com/blog/post/why-couchdb-rocks/) who seem to get this much more intuitively then I do, and [the amazing jChris](http://jchris.mfdz.com/code/2008/11/my_couch_or_yours__shareable_ap) who provides code to go along with his great ideas.

My desire to give back was peeked by Jan taking the time to answer a few desperate twitters I had. [My first question](http://twitter.com/wjhuie/status/1022082284) was simple… why does some code say “map” while others use “emit”… simple answer; it was changed and “emit()” is now the proper syntax.

The second question occurred when I was querying a view with a map/reduce pair and couldn’t get the python library to return the key / value pairs to me as expected. As Jan [states](http://twitter.com/janl/status/1023371802), if you pass “group=True” you can use results.key & results.value.

Ok, so that’s the simple Q&A recorded for posterity, but what about the neuron bending I promised. First let me say that if you want a tutorial on Neural Nets and AI programming you’ve come to the wrong place, and secondly this is my “answer” that’s evolved more then I care to admit.

So I’ll try not to take you through the brain process I went through but I’m sure we’ll both see how this could be made more “map/reduce”-y with future iterations. If you’ve got some of those insights I’d love to hear them because most of the examples I found already had that “AhhHA” moment made obvious.

One last disclaimer – I was looking to practice my jQuery and Django programming too so while this could definitely be made more “compact” it met my qualifications of building an “AJAX web application built with Django and utilizing CouchDB” application which was “simple”.

Now onward and upward! You’ll want to [start a Django program](http://docs.djangoproject.com/en/dev/intro/tutorial01/?from=olddocs) to hold all this code, a process which is better covered in many other tutorials. The title for my project is “nn_clicks” and I creates a “clicks” application underneath this directory.

So after you’ve got your project made, let’s start with the “front-end” which in this modern age is nearly always the web browser. Of course we need a webpage, and you can find mine [here](http://media.thecapacity.org/files/nn_click_template.html).

It’s nothing fancy but you’ll see there are two HTML elements which will report coordinates on the page. One, “#loc” obviously tracks the mouse and the other, “#guess” holds the contents of our AJAX call, which won’t work now given my blog’s hosting provider.

The magic happens thanks to two jQuery calls. The first simply links the ‘#loc’ element to report our mouse locations;

$(document).ready(function() {
  
$().mousemove(function(e) {
  
$(‘#loc’).html(e.pageX +’, ‘+ e.pageY);
  
});

While the second is where the magic happens when a click occurs;

$().click(function(e) {
  
loc = $(‘#loc’).html(e.pageX +’, ‘+ e.pageY);
  
$(‘#loc’).animate({left: e.pageX, top: e.pageY}, 450);

$.post(“process_click”,
  
{ “X”: e.pageX, “Y”: e.pageY },
  
function (r, status) {
  
guess = $(‘#guess’);
  
guess.html(r.X + ‘, ‘ + r.Y);
  
guess.animate({left: r.X, top: r.Y}, 450);
  
},
  
“json”
  
);
  
});

This function first sets the location information for #loc again, just in case, and then proceeds to move that HTML element to where you clicked (a fun effect). For this to work you have to have the CSS “position” attribute set to “absolute”. Which is something I’ve done with inline CSS, which is ugly and poor practice for my style guidelines, but sufficient for this tutorial.

The call to the $.post(…) function handles the AJAX magic. It packs the click coordinates (which are really a measure of ‘X’ and ‘height’ rather then a strict X,Y interpretation) into a JSON structure and the part that reads; “function (r, status) { … }” is then called when the django call “def process_click(…)” completes (more on this later).

So copy the HTML file to the “templates” directory under your Django project (make it if this doesn’t exist) and edit your urls.py file to include these two lines;

(r’^process\_click/?’, ‘nn\_click.clicks.views.process_click’),
  
(r’^(.*)/?’, ‘nn_click.clicks.views.index’),

The first line will receive our jQuery POST call which has the mouse coordinates as the data and the second will send anything else to our main page. So now we can edit the applications view (vi clicks/view.py) and add the next phase of changes.

Here’s the necessary line to take care of the second url redirection;

def index(request, something):
  
return render\_to\_response(‘nn\_click\_template.html’, { })

This simply takes our template and returns it with room for future data (the empty “{ }” part) if I need it later. That was simple and if you start up your Django project you should be able to load the webpage and it will at least follow your mouse. Since I serve files from a different box then I use for development try; “python manage.py runserver 0.0.0.0:8000”.

Now let’s figure out how to process that POST data we get from our jQuery POST call.

Again in click/view.py add this code;

def process_click(request):
  
click_X = int(request.POST[‘X’])
  
click_Y = int(request.POST[‘Y’])
  
return HttpResponse( “{ “X”: %s, “Y”: %s}” % (click\_X, click\_Y)

Now the POST call comes in, we parse out the X & Y values and return that data to the page (asynchronously). What should happen is that shortly after the click you’ll see the second tuple move to the same location. You may decide to divide the data by 2 or swap the X, Y values for a little bit of fun.

That seems like a natural place to conclude part one and I know we didn’t get into the couchdb part but never fear I’ll have part two out shortly!

**Update:** I have this part running (minus the couchdb and neural net code since I can’t figure how how to get those working via fastCGI) but you can check out the basic idea here;

http://nn-click.thecapacity.org/

