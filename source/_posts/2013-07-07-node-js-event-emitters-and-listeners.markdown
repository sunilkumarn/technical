---
author: sunilkumarn
comments: true
date: 2013-07-07 08:02:26
layout: post
slug: node-js-event-emitters-and-listeners
title: 'Node.js: Event Emitters and Listeners'
wordpress_id: 1239
categories: Node.js
---

When building client side applications with Javascript, events and handling them are quite common. Consider, for example,

{% highlight javascript %}

$('#toBeClicked').on('click', function() { alert('clicked') })

{% endhighlight %}

Here we handle a _click event_ on the element with id 'toBeClicked'. When the element is clicked, a 'click' event is emitted which is handled by the above statement written by us.

Just like in the DOM,  many objects in node emit events. When they do so, they inherit from the **EventEmitter** constructor.

Lets straight away build a custom event emitter and see whats going on,

{% highlight javascript %}

EventEmitter = require('events').EventEmitter       // The Event Emitter constructor in the events.js module .

{% endhighlight %}

Now, we create our custom emitter which is an instance of EventEmitter,

{% highlight javascript %}
emitter = new EventEmitter();

{% endhighlight %}

We listen for the error event,

{% highlight javascript %}
emitter.on('error', function(msg1, msg2) { console.log('ERR: ' + msg1 + ' ' + msg2 ) })

{% endhighlight %}

Here, `function(msg1, msg2) { console.log('ERR' + msg1 + ' ' + msg2 ) }`

is the callback to be performed once the event is emitted,

We could also listen as,

{% highlight javascript %}
emitter.addListener('error', function(msg1, msg2) { console.log('ERR: ' + msg1 + ' ' + msg2 ) } )

{% endhighlight %}

Now we emit the error event,

{% highlight javascript %}
emitter.emit('error', 'Bug detected', '@ Step 4')

{% endhighlight %}

Once the 'error' event is emitted, the listener performs the callback and we see the error logged in the console(as we have written in the callback function)
`ERR: Bug detected @ Step 4`

We could add listeners on any more custom events and handle them when the event is emitted.

Now that we have got to know how events are emitted and handled via listeners, lets try out a small server that listens for requests and processes them.

{% highlight javascript %}

var http = require('http'),
    sys = require('sys');

var server = http.createServer(function(request, response) {
  request.on('data', function (chunk) { console.log(chunk.toString()); });

  request.on('end', function() {
    response.write('Request Completed!');
    response.end();
  });

});

console.log("Starting up the server");
server.listen(8000);

{% endhighlight %}

Here, `http.createServer` method returns an object which inherits from the EventEmitter constructor.

Check out the [nodejs api doc](http://nodejs.org/api/http.html#http_http_createserver_requestlistener) for the createServer method in the** http.js** module,

`http.createServer([requestListener]), `_It says that requestListener is a function which is automatically added to the 'request' event._

To check whats going on behind the scenes here, lets dive into the code base of nodejs,
As can be seen from code, the createServer method is within the http.js module. Inspecting the **http.js** module,

{% highlight javascript %}
exports.createServer = function(requestListener) {
  return new Server(requestListener);
};
{% endhighlight %}

Check for the **_http_server.js** module to find the `Server` constructor which has the following lines of code,

{% highlight javascript %}
if (requestListener) {
  this.addListener('request', requestListener); // event listener for the server
}
{% endhighlight %}

As per the above snippet, '**this**'(the current server instance ) listens for the 'request' event and attaches the requestListener function to be performed when the event is emitted.

Here,

{% highlight javascript %}
function(request, response) {
  request.on('data', function (chunk) { console.log(chunk.toString()); });

  request.on('end', function() {
    response.write('Request Completed!');
    response.end();
  });
}
{% endhighlight %}

_is our requestListener._

Now, further inspecting the **_http_server.js **module**, **we could also see how the request event is emitted,

{% highlight javascript %}
self.emit('request', req, res); // event emitter for the server
{% endhighlight %}

_'req' and 'res' are the request, response objects that are passed as arguments to the requestListener function called when the 'request' event is emitted. Here self is 'this' ( our current server instance)_.

We could well make the server listen for the request event on our own. For this,

{% highlight javascript %}
var server = http.createServer()
server.on('request', function(request, response) {
  request.on('data', function (chunk) { console.log(chunk.toString()); });

  request.on('end', function() {
    response.write('Request Completed!');
    response.end();
  })
});
{% endhighlight %}

Here when we create the server instance, we do not pass the requestListener( _Do notice that requestListener was only optional in http.createServer([requestListener]) _). Instead we attach a listener of our own on the server which listens to the 'request' event and performs the callback function when the request event is emitted, i.e,

{% highlight javascript %}
server.on('request', function(request, response) { ... });
{% endhighlight %}
