---
id: 181
title: 'Training Neural Nets with CouchDB – part 3'
date: 2008-12-04T13:08:21+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=181
permalink: /2008/12/04/training-neural-nets-with-couchdb-part-3/
categories:
  - code
  - couchdb
  - python
---
Hopefully you’ve been following parts [1](http://blog.thecapacity.org/2008/12/01/training-neural-nets-with-couchdb-part-1/) and [2](http://blog.thecapacity.org/2008/12/02/training-neural-nets-with-couchdb-part-2/) and I didn’t leave anyone too confused by my approach.

Please visit my posts for a far better recap then I can provide here ([DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself)); In part 1, I introduced the overall project discussed the django layout and focused on the jquery I/O part. My goal in part 2 was to show some of the underlying mechanics of how we triggered a neuron as well as queried couchdb for info.

I know we honed in on some serious specifics but now that we’ve got those specifics I’d like to step back and put together the pieces.

If you check out a copy of [the interface](http://nn-click.thecapacity.org/) (which doesn’t have the NN backend) you can get a sense of the I/O process. In the full application here’s the process;

> (0) When a user clicks anywhere on the page that’s sent to our django URL and (1) django records the location then (2) queries our neural net for a guess. (Step 6) This guess will be sent back to the page to move the second coordinate box, and in the sample is set to be equal to the click location. However, before returning the guess, (3,4) the difference between the guess and the actual click is sent to the network so it can train for next time. Also, (5) the input nodes need to be set to the click location so that next time the net can base it’s output on your previous click.

Here’s the relevant code;

> #get the clicked coordinates
  
> click_X = int(request.POST[‘X’])
  
> click_Y = int(request.POST[‘Y’])
> 
> #Figure out what the net would have guessed
  
> guess\_X = int(get\_output(“output_x”))
  
> guess\_Y = int(get\_output(“output_y”))
> 
> #Find the error and propagate that back to the net
  
> error\_X = float(click\_X) – float(guess_X)
  
> error\_Y = float(click\_Y) – float(guess_Y)
  
> back\_prop(“output\_x”, error_X)
  
> back\_prop(“output\_y”, error_Y)
> 
> #Set the inputs for next time
  
> set\_input\_weight(“input_x”, “static”, request.POST[‘X’])
  
> set\_input\_weight(“input_y”, “static”, request.POST[‘Y’])
> 
> #return the guess to jQuery
  
> return HttpResponse( “{ “X”: %s, “Y”: %s}” % (guess\_X, guess\_Y) )

Now I know you only know how a neuron is defined and have no idea of our neural net structure but I think that process will be very illustrative.

Since our net needs to predict X and Y coordinates we need to have two output nodes which I’ve named “output\_x” and “output\_y” and similarly we have “input\_x” and “input\_y” in order to inject the coordinate values into the network. You can see the keyword “static” trick that we discussed previously being used.

Neural Nets are typically called [“Feed Forward Neural Nets”](http://en.wikipedia.org/wiki/Feedforward_neural_network) because you stimulate node “A” and then things cascade A -> B ->C and you can read the output at “C”.

However, programtically this can be a royal pain.

  * In some situations you’d need a clock to sync with so you can ensure that you’re reading the n’th output of C and not the n+1 (e.g. if the input to A was controlled via a separate thread then things could shift underneath you before you read the values).
  * You could also build this communication via a messaging service. Node “A” could broadcast its output at a given time and then use a pub/sub model so interested nodes could be alerted as events progressed.

The last approach probably scales the best if you had a ready queuing mechanism then I’d go this way for larger networks. This is more apparent when you realize that “A,B & C” aren’t necessarially single nodes.

Neural nets are commonly built with layers, each of which typically contains multiple nodes all of which are connected to all of the nodes in the previous and subsequent layers.

So in my case “A” is the first layer containing “input\_x” and “input\_y” and “C” is the output layer containing “output\_x” and “output\_y”. Layer B would have many nodes all receiving the output from both nodes in layer A. (As an aside you can have more complicated layering systems, for example “output\_x” might also have a direct link from “input\_x” to further augment it’s correlation and it is legal for a layer to only have a single node.)

So you’ve the “API” for our neural net and part 2 covers the underlying mathematical (and procedural) mechanics so I should be able to wrap up next time by discussing how to get this thing off the ground and see how it works!
