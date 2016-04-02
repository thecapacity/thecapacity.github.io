---
id: 171
title: 'Training Neural Nets with CouchDB – part 2'
date: 2008-12-02T14:37:54+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=171
permalink: /2008/12/02/training-neural-nets-with-couchdb-part-2/
categories:
  - code
  - couchdb
  - python
---
It’s always nice to have a little [encouragement](http://twitter.com/dturkenk/status/1033303086) especially when trying to work through some tough posts. I really prefer white boards and pictures but I find these too hard to make for blogging so I try to let the words shape the images in your mind.

So let’s get started with your first exercise… picture or graph the output of tanh() from [-1,1]. For those who will skip the process of pulling out their graphing calculator, you’ll get a sinusoidal curve, i.e. shaped like an “S”, with asymptotes at y=-1 and y=1.

So, I know that’s neat and all but what does it have to do with couchdb? Well it doesn’t per say but it represents our “trigger” function for our neurons. We’ll use this to convert a nodes input to its output, and let’s call this function sigmoid().

The input to this function is actually the weight our node attributes to a specific input. So let’s say I’m a node, N, and I have an input, I. That input will be a value, numeric in this case, and I’ll assign it a weight, W. That weight will correlate with my “trust” in that input (after an appropriate training process). But the key part now is that the output, O, of N will be; O = I * sigmoid(W).

If you play around and graph some of this, you’ll see that if I don’t “trust” I, and W approaches 0 then even if I is a very big number O will be near 0 also.

Before we get into this business of trust let me give you two functions;

> def sigmoid(x):
  
> return tanh(float(x))
> 
> def slope_sigmoid(y):
  
> return 1.0-y*y

Let me comment on that slope function there. The slope tells us how “drastic” a small change will make our output and we’ll use this function in order to adjust our weights appropriately. This is important because if we’re at the center of our sigmoid (x=.5) and want to reinforce the weight then we may only need to move it by +.01 but if we’re at an extreme on our curve (e.g. x = -.8) then we may need to adjust the weight by -.1 (i.e. 10 times as much) in order to achieve a noticeable change.

OK, let’s talk about that mythic “trust” factor and in order to do that let’s talk about neural nets. NN’s are build by collecting Neurons, connecting outputs to inputs (not usually circular but they can be) and then stimulating an input layer with a set of values and reading an output layer to see what it produced.

Here’s, in psuedo python, is how I represented a neuron;

> class neuron():
> 
> name = “”
> 
> inputs = []

It’s relatively a simple structure. Neuron’s have a name (which are forced to be unique) and a set of inputs (which is actually a list of dictionaries). An important thing to clarify is that I never actually had to define this class, since CouchDB doesn’t demand a schema; Let’s make this a bit more concrete with three examples;

> >>> nn[‘input_x’]
  
> <Document u’input_x’@u’82302167′ {u’inputs’: [{u’node’: u’static’, u’weight’: u’474′}], u’increment’: 0}>
  
> >>> nn[‘1′]
  
> <Document u’1’@u’2027135756′ {u’inputs’: [{u’node’: u’input\_x’, u’weight’: 1}, {u’node’: u’input\_y’, u’weight’: -1}], u’increment’: 0}>
  
> >>> nn[‘output_x’]
  
> <Document u’output_x’@u’1458836188′ {u’inputs’: [{u’node’: u’1′, u’weight’: 1}, {u’node’: u’2′, u’weight’: 9.536375211640632e+179}, {u’node’: u’3′, u’weight’: -194411155905.33661}], u’increment’: 0}>

Hopefully that shows up ok on your browser. The variable “nn” is a link to the “neural_net” database on my couchdb server;

> try:
  
> server = couchdb.Server(‘http://127.0.0.1:5984’)
  
> nn = server[‘neural_net’]
  
> len(nn)
  
> except couchdb.client.ResourceNotFound:
  
> server.create(‘neural_net’)
  
> nn = server[‘neural_net’]
  
> len(nn)

I’ve printed out three neuron nodes; The first node, ‘input\_x’, is part of the input layer and you’ll see it has list ‘inputs’ with a single dictionary element { ‘node’: ‘…’, ‘weight’: ‘…’ }. I’ve opted to use the name “static” as a keyword to represent an input which doesn’t point to another node and use the ‘weight’ as the actual input value. The second node ‘1’ is more of a typical neuron which would be considered a “second level neuron”. This takes two inputs, one from ‘input\_x’ and one from ‘input_y’. The output of “1” will be;

for input in nn\[‘1’\]\[‘inputs’\]

output += get_output(inputs[‘node’]) * sigmoid(input[‘weight’])

Note I used a function, called “get\_output” to find the output value of a node. If the node is static, as ‘input\_x’ is, then we could simply dereference it and get the “weight” value but if the input is another “pure” neuron then it may have some calculations to do.You can see how this would work in practice by examining the final node, ‘output\_x’. In this case we have to query many nodes just like node “1” and allow it to do it’s calculation before we can output our values. So a call to “get\_output(“output_x'”) actually recurses to the various nodes in turn.

Let me take a moment to diverge from “what I did” to talk about “what I almost did”. I’d intended for “get_output()” to be a CouchDB view and take advantage of quick, asynchronous, lookups. However, if this was done as a view then I’d need the emit function to reference the database and I don’t think this is allowed, i.e. I’d need a map function something like;

**Note this won’t, and to the best of my knowledge can’t, work;**

function(doc) {
  
if ( doc[‘inputs’].length > 0 ) {
  
for ( i in doc[‘inputs’] ) {
  
emit( doc[‘_id’], **“_view/getoutput(doc\[‘inputs’\]\[i\])”** );
  
}
  
}
  
}

The broken part of that view is the value part of the emit function (remember emit produces key/value pairs). However, since we’re on the topic of couchdb views here’s one way we can build a view to see what inputs a node has;

> function(doc) {
  
> if ( doc[‘inputs’].length > 0 ) {
  
> for ( i in doc[‘inputs’] ) {
  
> emit( doc\[‘_id’], doc[‘inputs’\]\[i\] );
  
> }
  
> }
  
> }

I also wanted a reduce function to combine the value parts to a single key (so it matched my “data structure” so here’s the reduce;

> function(keys, values, rereduce) {
  
> return values
  
> }

The great part of couchdb is that you can input these views in it’s code window and get immediate feedback on what’s being produced! Now I’ll show you the sad part of my design, here’s how to query and act on the view;

> #This loop gives us all the inputs to node: name
  
> for input_nodes in db.view(“/nnodes/inputs”, group=True)[name]:
  
> #This loop gives us all the input nodes to node: name
  
> for in\_node in input\_nodes.value:
  
> if u’static’ in in_node[‘node’]:
  
> output += float(in_node[‘weight’])
  
> else: #Not a static input
  
> output += get\_output(in\_node[‘node’]) * sigmoid(in_node[‘weight’])

What’s sad to me is that it would be less code to query the documents directly;

> node = db[str(name)]
  
> for in_node in node[‘inputs’]:
  
> if u’static’ in in_node[‘node’]:
  
> output += float(in_node[‘weight’])
  
> else: #Not a static input
  
> output += get\_output(in\_node[‘node’]) * sigmoid(in_node[‘weight’])

You’ll see that I used the “group=True” parm that I mentioned in my previous post. This just made things match my conceptual model but I wish the python library didn’t force me to dereference .key and .value to get them (it should turn them into a dictionary instead). I’ll also mention that several times I got confused trying [‘value’] instead of .value (something that wouldn’t matter in javascript but the former seems more “pythonic”).

I think this is a clear example that I’ve got more to learn about views and better ways to represent my data structures. Here’s [a great example](http://www.cmlenz.net/archives/2007/10/couchdb-joins) which I think will find a lot of analogous fits so read it often.

Back to my situation though, I’d thought maybe each node could store an array called “output_history” which could then be queried with an “increment” value (which would make it a simple emit() process). However, this was much more complicated then it was worth for an initial pass and, since if the value didn’t exist, it would still have to be calculated via a non-Map/Reduce function (because it would have to reference the database).

Before I get into much more detail let me [show you the code](http://dpaste.com/hold/95456/) so you can take a moment to look it over and formulate some thoughts.

I’ll be back with post 3 to try and tie it all together (including a step back to revisit the overall connections, talk more about Neural Nets and my impressions of couchdb).

