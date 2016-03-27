---
id: 171
title: 'Training Neural Nets with CouchDB &#8211; part 2'
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
It&#8217;s always nice to have a little [encouragement](http://twitter.com/dturkenk/status/1033303086) especially when trying to work through some tough posts. I really prefer white boards and pictures but I find these too hard to make for blogging so I try to let the words shape the images in your mind.

So let&#8217;s get started with your first exercise&#8230; picture or graph the output of tanh() from [-1,1]. For those who will skip the process of pulling out their graphing calculator, you&#8217;ll get a sinusoidal curve, i.e. shaped like an &#8220;S&#8221;, with asymptotes at y=-1 and y=1.

So, I know that&#8217;s neat and all but what does it have to do with couchdb? Well it doesn&#8217;t per say but it represents our &#8220;trigger&#8221; function for our neurons. We&#8217;ll use this to convert a nodes input to its output, and let&#8217;s call this function sigmoid().

The input to this function is actually the weight our node attributes to a specific input. So let&#8217;s say I&#8217;m a node, N, and I have an input, I. That input will be a value, numeric in this case, and I&#8217;ll assign it a weight, W. That weight will correlate with my &#8220;trust&#8221; in that input (after an appropriate training process). But the key part now is that the output, O, of N will be; O = I * sigmoid(W).

If you play around and graph some of this, you&#8217;ll see that if I don&#8217;t &#8220;trust&#8221; I, and W approaches 0 then even if I is a very big number O will be near 0 also.

Before we get into this business of trust let me give you two functions;

> def sigmoid(x):
  
> return tanh(float(x))
> 
> def slope_sigmoid(y):
  
> return 1.0-y*y

Let me comment on that slope function there. The slope tells us how &#8220;drastic&#8221; a small change will make our output and we&#8217;ll use this function in order to adjust our weights appropriately. This is important because if we&#8217;re at the center of our sigmoid (x=.5) and want to reinforce the weight then we may only need to move it by +.01 but if we&#8217;re at an extreme on our curve (e.g. x = -.8) then we may need to adjust the weight by -.1 (i.e. 10 times as much) in order to achieve a noticeable change.

OK, let&#8217;s talk about that mythic &#8220;trust&#8221; factor and in order to do that let&#8217;s talk about neural nets. NN&#8217;s are build by collecting Neurons, connecting outputs to inputs (not usually circular but they can be) and then stimulating an input layer with a set of values and reading an output layer to see what it produced.

Here&#8217;s, in psuedo python, is how I represented a neuron;

> class neuron():
> 
> name = &#8220;&#8221;
> 
> inputs = []

It&#8217;s relatively a simple structure. Neuron&#8217;s have a name (which are forced to be unique) and a set of inputs (which is actually a list of dictionaries). An important thing to clarify is that I never actually had to define this class, since CouchDB doesn&#8217;t demand a schema; Let&#8217;s make this a bit more concrete with three examples;

> >>> nn[&#8216;input_x&#8217;]
  
> <Document u&#8217;input_x&#8217;@u&#8217;82302167&#8242; {u&#8217;inputs&#8217;: [{u&#8217;node&#8217;: u&#8217;static&#8217;, u&#8217;weight&#8217;: u&#8217;474&#8242;}], u&#8217;increment&#8217;: 0}>
  
> >>> nn[&#8216;1&#8242;]
  
> <Document u&#8217;1&#8217;@u&#8217;2027135756&#8242; {u&#8217;inputs&#8217;: [{u&#8217;node&#8217;: u&#8217;input\_x&#8217;, u&#8217;weight&#8217;: 1}, {u&#8217;node&#8217;: u&#8217;input\_y&#8217;, u&#8217;weight&#8217;: -1}], u&#8217;increment&#8217;: 0}>
  
> >>> nn[&#8216;output_x&#8217;]
  
> <Document u&#8217;output_x&#8217;@u&#8217;1458836188&#8242; {u&#8217;inputs&#8217;: [{u&#8217;node&#8217;: u&#8217;1&#8242;, u&#8217;weight&#8217;: 1}, {u&#8217;node&#8217;: u&#8217;2&#8242;, u&#8217;weight&#8217;: 9.536375211640632e+179}, {u&#8217;node&#8217;: u&#8217;3&#8242;, u&#8217;weight&#8217;: -194411155905.33661}], u&#8217;increment&#8217;: 0}>

Hopefully that shows up ok on your browser. The variable &#8220;nn&#8221; is a link to the &#8220;neural_net&#8221; database on my couchdb server;

> try:
  
> server = couchdb.Server(&#8216;http://127.0.0.1:5984&#8217;)
  
> nn = server[&#8216;neural_net&#8217;]
  
> len(nn)
  
> except couchdb.client.ResourceNotFound:
  
> server.create(&#8216;neural_net&#8217;)
  
> nn = server[&#8216;neural_net&#8217;]
  
> len(nn)

I&#8217;ve printed out three neuron nodes; The first node, &#8216;input\_x&#8217;, is part of the input layer and you&#8217;ll see it has list &#8216;inputs&#8217; with a single dictionary element { &#8216;node&#8217;: &#8216;&#8230;&#8217;, &#8216;weight&#8217;: &#8216;&#8230;&#8217; }. I&#8217;ve opted to use the name &#8220;static&#8221; as a keyword to represent an input which doesn&#8217;t point to another node and use the &#8216;weight&#8217; as the actual input value. The second node &#8216;1&#8217; is more of a typical neuron which would be considered a &#8220;second level neuron&#8221;. This takes two inputs, one from &#8216;input\_x&#8217; and one from &#8216;input_y&#8217;. The output of &#8220;1&#8221; will be;

for input in nn\[&#8216;1&#8217;\]\[&#8216;inputs&#8217;\]

output += get_output(inputs[&#8216;node&#8217;]) * sigmoid(input[&#8216;weight&#8217;])

Note I used a function, called &#8220;get\_output&#8221; to find the output value of a node. If the node is static, as &#8216;input\_x&#8217; is, then we could simply dereference it and get the &#8220;weight&#8221; value but if the input is another &#8220;pure&#8221; neuron then it may have some calculations to do.You can see how this would work in practice by examining the final node, &#8216;output\_x&#8217;. In this case we have to query many nodes just like node &#8220;1&#8221; and allow it to do it&#8217;s calculation before we can output our values. So a call to &#8220;get\_output(&#8220;output_x'&#8221;) actually recurses to the various nodes in turn.

Let me take a moment to diverge from &#8220;what I did&#8221; to talk about &#8220;what I almost did&#8221;. I&#8217;d intended for &#8220;get_output()&#8221; to be a CouchDB view and take advantage of quick, asynchronous, lookups. However, if this was done as a view then I&#8217;d need the emit function to reference the database and I don&#8217;t think this is allowed, i.e. I&#8217;d need a map function something like;

**Note this won&#8217;t, and to the best of my knowledge can&#8217;t, work;**

function(doc) {
  
if ( doc[&#8216;inputs&#8217;].length > 0 ) {
  
for ( i in doc[&#8216;inputs&#8217;] ) {
  
emit( doc[&#8216;_id&#8217;], **&#8220;_view/getoutput(doc\[&#8216;inputs&#8217;\]\[i\])&#8221;** );
  
}
  
}
  
}

The broken part of that view is the value part of the emit function (remember emit produces key/value pairs). However, since we&#8217;re on the topic of couchdb views here&#8217;s one way we can build a view to see what inputs a node has;

> function(doc) {
  
> if ( doc[&#8216;inputs&#8217;].length > 0 ) {
  
> for ( i in doc[&#8216;inputs&#8217;] ) {
  
> emit( doc\[&#8216;_id&#8217;], doc[&#8216;inputs&#8217;\]\[i\] );
  
> }
  
> }
  
> }

I also wanted a reduce function to combine the value parts to a single key (so it matched my &#8220;data structure&#8221; so here&#8217;s the reduce;

> function(keys, values, rereduce) {
  
> return values
  
> }

The great part of couchdb is that you can input these views in it&#8217;s code window and get immediate feedback on what&#8217;s being produced! Now I&#8217;ll show you the sad part of my design, here&#8217;s how to query and act on the view;

> #This loop gives us all the inputs to node: name
  
> for input_nodes in db.view(&#8220;/nnodes/inputs&#8221;, group=True)[name]:
  
> #This loop gives us all the input nodes to node: name
  
> for in\_node in input\_nodes.value:
  
> if u&#8217;static&#8217; in in_node[&#8216;node&#8217;]:
  
> output += float(in_node[&#8216;weight&#8217;])
  
> else: #Not a static input
  
> output += get\_output(in\_node[&#8216;node&#8217;]) * sigmoid(in_node[&#8216;weight&#8217;])

What&#8217;s sad to me is that it would be less code to query the documents directly;

> node = db[str(name)]
  
> for in_node in node[&#8216;inputs&#8217;]:
  
> if u&#8217;static&#8217; in in_node[&#8216;node&#8217;]:
  
> output += float(in_node[&#8216;weight&#8217;])
  
> else: #Not a static input
  
> output += get\_output(in\_node[&#8216;node&#8217;]) * sigmoid(in_node[&#8216;weight&#8217;])

You&#8217;ll see that I used the &#8220;group=True&#8221; parm that I mentioned in my previous post. This just made things match my conceptual model but I wish the python library didn&#8217;t force me to dereference .key and .value to get them (it should turn them into a dictionary instead). I&#8217;ll also mention that several times I got confused trying [&#8216;value&#8217;] instead of .value (something that wouldn&#8217;t matter in javascript but the former seems more &#8220;pythonic&#8221;).

I think this is a clear example that I&#8217;ve got more to learn about views and better ways to represent my data structures. Here&#8217;s [a great example](http://www.cmlenz.net/archives/2007/10/couchdb-joins) which I think will find a lot of analogous fits so read it often.

Back to my situation though, I&#8217;d thought maybe each node could store an array called &#8220;output_history&#8221; which could then be queried with an &#8220;increment&#8221; value (which would make it a simple emit() process). However, this was much more complicated then it was worth for an initial pass and, since if the value didn&#8217;t exist, it would still have to be calculated via a non-Map/Reduce function (because it would have to reference the database).

Before I get into much more detail let me [show you the code](http://dpaste.com/hold/95456/) so you can take a moment to look it over and formulate some thoughts.

I&#8217;ll be back with post 3 to try and tie it all together (including a step back to revisit the overall connections, talk more about Neural Nets and my impressions of couchdb).