---
title: 'Building your first node.js app – Part 2: Building the web server and request dispatcher'
author: jblotus
layout: post

dsq_thread_id:
  - 317333383
categories:
  - Design Patterns
  - Front Controller
  - Web Development
  - Programming
tags:
  - front controller
  - javascript
  - node
  - node setup webserver
  - node.js
  - try/catch
---
In my [part 1 of my node tutoria][1]l, I talked about getting [Node][2] up and running with [cygwin][3]. Now in part 2, I am going to show you how to implement a basic web server and request dispatcher. In Node, we are responsible for creating much of the low level functionality that is normally abstracted away from us by web servers like [Apache ][4]and [nginx][5]. While you can use custom node userland modules to handle the basics, it is important to learn how to build a web server in node from the ground up.

**Handling Client Requests** We have to think about how to handle our clients requests and how to respond to them. Since I come from the PHP world, I prefer to use a[ front controller pattern][6] to handle client requests. What that means is that we want all requests in our application to come in through one entry point and route them depending upon the url requested. With Node, the front-controller pattern is a natural fit, since implementing a direct file access pattern would actually require us to write and implement it in our server-side code.

Let&#8217;s begin by creating the code for our web server. In your project root directory (for me it&#8217;s* /home/jblotus/sites/todos/*):

<pre class="brush:shell">touch server.js</pre> Now open server.js and enter the following code:

<pre class="brush:js">//include our modules
var sys   = require('sys');
var http  = require('http');
var url   = require('url');</p>



<p>
  //require custom dispatcher
  var dispatcher = require('./lib/dispatcher.js');
</p>



<p>
  console.log('Starting server @ http://127.0.0.1:1337/');
</p>



<p>
  http.createServer(function (req, res) {
    //wrap calls in a try catch
    //or the node js server will crash upon any code errors
    try {
      //pipe some details to the node console
      console.log('Incoming Request from: ' +
                   req.connection.remoteAddress +
                  ' for href: ' + url.parse(req.url).href
      );
</p>



<pre><code>//dispatch our request
dispatcher.dispatch(req, res);
</code></pre>



<p>
  } catch (err) {
      //handle errors gracefully
      sys.puts(err);
      res.writeHead(500);
      res.end('Internal Server Error');
    }
</p>



<p>
  }).listen(1337, "127.0.0.1", function() {
    //runs when our server is created
    console.log('Server running at http://127.0.0.1:1337/');
  });</pre>
  Let&#8217;s go over the code section by section. First we require some <a href="http://nodejs.org/docs/v0.4.8/api/modules.html">node core modules</a>.


  <pre class="brush:js">//include our modules
var sys   = require('sys');
var http  = require('http');
var url   = require('url');</pre>
  In order to actually create a web server, we are going to need the node http library. The sys library is useful to dump things to the web console, although I have been using console.log() method. The URL library has a bunch of URL methods that breaks a URL into a nicely structured object. I am only using it here to output a nicely formatted message to our server console.
</p>



<p>
  Next, I include our custom dispatcher module, which we will create in<em> lib/dispatcher.js </em>. In fact let&#8217;s go ahead and do this now. From the <em>root project directory</em>:


  <pre class="brush:shell">mkdir lib
touch lib/dispatcher.js</pre>
  We will add some functionality to the

  <strong>dispatcher </strong>in a minute. For now let&#8217;s get our server up and running. You will notice that I output a message to the console before I invoke the server.


  <pre class="brush:shell">console.log('Starting server @ http://127.0.0.1:1337/');</pre>
  This is not really necessary but I want to emphasize the importance of outputting good debug messages when beginning to learn how to use Node. Due to the non blocking nature of node, things don&#8217;t always happen in the order that you would expect. If you are used to doing ajax calls or even animation in jQuery then you should understand that most i/o calls in node are performed asynchronously (but not all).
</p>



<p>
  Now to create a web server:


  <pre class="brush:js">http.createServer(function (req, res) {</p>



<p>
  }).listen(1337, "127.0.0.1", function() {
</p>



<p>
  //runs when our server is created
    console.log('Server running at http://127.0.0.1:1337/');
  });</pre>
  We call the <strong>createServer()</strong> method on the Node core http object. The argument we pass to the method is an anonymous function with two arguments, a request object and a response object. We then chain the <strong>listen()</strong> method to server object returned by <strong>createServer()</strong> and tell it to listen at port <em>1137 </em>on our <em>localhost</em>.  (<em>You can use any port you want</em>). You will notice that the third argument of the <strong>listen()</strong> method is a callback function which fires after the server instance is created. I though that would be a good place to add a debug message.
</p>



<p>
  Now to start the server:


  <pre class="brush:shell">$ node server.js
Starting server @ http://127.0.0.1:1337/
Server running at http://127.0.0.1:1337/</pre>
  If you fire up your browser at

  <em>http://127.0.0.1:1337/</em> at this point you are just going to be served an infinite loop since we haven&#8217;t actually told our web server what to do with the requests it recieves from client browsers. You might also get a &#8220;<em>could not connect</em>&#8221; error if you had any typos or javascript errors in<em> server.js</em>. That brings me to my next point.
</p>



<p>
  <strong>Javascript errors crash your web server? That sucks!</strong>
  I agree, but how should a web server handle errors? You need to make your code resilient enough to handle errors and make liberal use of the<em><a href="https://developer.mozilla.org/en/JavaScript/Reference/Statements/try...catch"> try/catch construct</a></em> if you don&#8217;t want your shiny new web server to <a href="http://www.youtube.com/watch?v=rcNeorjXMrE">explode like a Ford Pinto</a> on the first error it runs over. My approach to dealing with this is wrapping my request/response handling code in a try catch block.


  <pre class="brush:js">http.createServer(function (req, res) {
  //wrap calls in a try catch or the node js server will crash upon any code errors
  try {
    //pipe some details to the node console
    console.log('Incoming Request from: ' +
                 req.connection.remoteAddress +
                ' for href: ' + url.parse(req.url).href
   );</p>



<pre><code>//dispatch our request
dispatcher.dispatch(req, res);
</code></pre>



<p>
  } catch (err) {
      //handle errors gracefully
      sys.puts(err);
      res.writeHead(500);
      res.end('Internal Server Error');
    }
</p>



<p>
  })</pre>
  The first thing I do is output some information about the incoming request to the console.


  <pre class="brush:js">    console.log('Incoming Request from: ' +
                req.connection.remoteAddress +
               ' for href: ' + url.parse(req.url).href
   );</pre>
  This takes a remote client IP address from the

  <em>req.connection</em> object passed when a client hits the server. I also output the url requested by running the handy<em> url.parse()</em> method on the req.url object and output just the href piece.
</p>



<p>
  This gives a message like this when you hit the page once:


  <pre class="brush:shell">Incoming Request from: 127.0.0.1 for href: /
Incoming Request from: 127.0.0.1 for href: /favicon.ico</pre>


  <strong>Whoa two requests?? What gives man? </strong>
  Keep in mind that the average web browser will automatically request the file <em>favicon.ico</em>. We could write code to handle these uri&#8217;s individually, but that is somewhat tedious approach if not entirely pointless. Remember that the web server is going to have to respond to all manner of outlandish urls like apple touch icons, hackers, and mistyping grandmas. Our code has to be durable enough to handle these requests. But we won&#8217;t worry about that until we get to the <strong>dispatcher </strong>portion.
</p>



<p>
  <strong>Lets look at the try/catch block:</strong>


  <pre class="brush:js">  try {
    //pipe some details to the node console
    console.log('Incoming Request from: ' +
                 req.connection.remoteAddress +
                ' for href: ' + url.parse(req.url).href
   );
   //dispatch our request
    dispatcher.dispatch(req, res);</p>



<p>
  } catch (err) {
      //handle errors gracefully
      sys.puts(err);
      res.writeHead(500);
      res.end('Internal Server Error');
    }</pre>
  We will first try and call the <strong>dispatch()</strong> method of our as yet unimplemented <strong>dispatcher </strong>object. If we get any JavaScript errors from inside that code we will catch them gracefully and output a message to the console saying what went wrong. We then call the <strong>writeHead()</strong> method on the node response object and pass it a value of <em>500 </em>(a standard http error code). From there we need to end the connection with the client by calling the <strong>end()</strong> method on the response object which takes a message which can be output to the browser.
</p>



<p>
  <strong>Done Yet please?</strong>
</p>



<p>
  Hey, take it easy bud. The <em>server.js</em> file is done for now. A nice side effect of pushing the request/response cycle of onto the <strong>dispatcher </strong>is that we don&#8217;t have to overload our <em>server.js</em> with too much functionality. Any errors that happen from inside the dispatcher won&#8217;t crash the node server since they are being caught. Since our server doesn&#8217;t actually work yet, let&#8217;s go ahead an implement the <strong>dispatcher</strong> object.
</p>



<p>
  Here is the sample code for <em>lib/dispatcher.js</em>


  <pre class="brush:js">var fs = require('fs');</p>



<p>
  var actions = {
    'view' : function(user) {
      return '<h1>Todos for ' + user + '</h1>';
    }
  }
</p>



<p>
  this.dispatch = function(req, res) {
</p>



<p>
  //some private methods
    var serverError = function(code, content) {
      res.writeHead(code, {'Content-Type': 'text/plain'});
      res.end(content);
    }
</p>



<p>
  var renderHtml = function(content) {
      res.writeHead(200, {'Content-Type': 'text/html'});
      res.end(content, 'utf-8');
    }
</p>



<p>
  var parts = req.url.split('/');
</p>



<p>
  if (req.url == "/") {
      fs.readFile('./webroot/index.html', function(error, content) {
        if (error) {
          serverError(500);
        } else {
          renderHtml(content);
        }
      });
</p>



<p>
  } else {
      var action   = parts[1];
      var argument = parts[2];
</p>



<pre><code>if (typeof actions[action] == 'function') {
  var content = actions[action](argument);
  renderHtml(content);
} else {
  serverError(404, '404 Bad Request');
}
</code></pre>



<p>
  }
  }</pre>
  Looks like a lot going on in there, but let&#8217;s take it apart line by line.
</p>



<p>
  First we include the file system node core component.


  <pre class="brush:js">//node filesystem module
var fs = require('fs');</pre>
  We are going to use this to read files, and specifically to read our

  <em>index.html</em> template.
</p>



<p>
  Next, an <em>actions </em>object is defined. In here I am hard coding a view action, which takes an argument, <em>user</em>, and returns some hard-coded html with the users name. Eventually we will move this into a separate file or template, but for now this works fine.


  <pre class="brush:js">var actions = {
  'view' : function(user) {
    return '&lt;h1&gt;Todos for ' + user + '&lt;/h1&gt;';
  }
}</pre>
  Next we define our

  <strong>dispatch()</strong> method. This can be done by typing <strong>exports.dispatch</strong>, but <strong>this.dispatch</strong> also works. It is important to note that anything declared with <em>var</em> will not be accessible outside the module scope (<em>we are in a module now</em>) since node uses the <a href="http://www.commonjs.org/">CommonJS</a> method of sandboxing scripts.


  <pre class="brush:js">this.dispatch = function(req, res) {
}</p>



<p>
  //or
  exports.dispatch = function(req, res) {
  }</pre>
  <strong>For this project I am defining the role of the dispatcher as follows:</strong>
</p>



<ul>
  <li>
    Parse the url requested.
  </li>


  <li>
    The first portion after the front slash <strong>/</strong> will be for the action.
  </li>


  <li>
    If the action matches any defined actions in our actions object, execute that action and pass the next piece of the url (<em>delimeted by a forward slash</em>) as an argument.
  </li>


  <li>
    For now our actions only take one argument.
  </li>


  <li>
    If we do not match any defined actions, throw a 404 error.
  </li>

</ul>



<p>
  You will notice that in the dispatch method I use two functions, <strong>serverError()</strong>, and <strong>renderHtml()</strong>. I did this because in the <strong>dispatcher</strong>, we will often be outputting an error or some html and instead of specifying headers each time, we can keep it contained in a private method.


  <pre class="brush:js">  //some private methods
  var serverError = function(code, content) {
    res.writeHead(code, {'Content-Type': 'text/plain'});
    res.end(content);
  }</p>



<p>
  var renderHtml = function(content) {
      res.writeHead(200, {'Content-Type': 'text/html'});
      res.end(content, 'utf-8');
    }</pre>
  First I want to return our <em>index.html</em> page if no specific uri is requested.


  <pre class="brush:js">  if (req.url == "/") {
     fs.readFile('./webroot/index.html', function(error, content) {
      if (error) {
        serverError(500);
      } else {
        renderHtml(content);
      }
    });</p>



<p>
  }</pre>
  Inside this block, if no url is requested I call the node fs module&#8217;s <strong>readFile()</strong> method, passing it the location of an index template (<em>not yet built</em>). The second argument is a callback function which passes an error and content parameter. Inside of this callback we can check to see if there was an error reading the file, in which case a <em>500 error</em> is triggered, otherwise we render the html page.
</p>



<p>
  Let&#8217;s go ahead and create <em>index.html</em>.


  <pre class="brush:shell">mkdir webroot
touch webroot/index.html</pre>


  <strong>For now let&#8217;s make it really simple:</strong>


  <pre class="brush:plain"><strong>&lt;h1&gt;Todo list in Node&lt;/h1&gt;</strong></pre>
  That takes care of our static html index page, but next we need to handle the view action. In our app, we are going to say that if you go to

  <em> /view/james</em>, you are going to get a todo list belonging to the user <em>james</em>. To do this, we have to split the requested url on the forward slash character.


  <pre class="brush:js">var parts = req.url.split('/')</pre>
  If we hit our webserver at

  <em> /view/james</em>, <strong>split() </strong>will return an array that looks like this:


  <pre class="brush:js">['', 'view', 'james']</pre>
  Since

  <strong> parts[1]</strong> will be our action and <strong>parts[2]</strong> will be the argument, I  assign them to be variables.


  <pre class="brush:js">    var action   = parts[1];
    var argument = parts[2];</pre>
  Now the

  <strong>dispatcher </strong>will do some checking to see if the <strong>action </strong>is actually a callable function in our <strong>actions object</strong>, and if so execute it. Otherwise of course we would output a 404 error to indicate that the page was not found.
</p>



<p>
  <strong>Note</strong>: Eventually, we will add in code to first check the webroot dir for a file matching the request an return it (like a<em> css file </em>or<em> static asset</em>). For now we are just going to return a <em>404</em>.


  <pre class="brush:js">    if (typeof actions[action] == 'function') {
      var content = actions<a href="argument">action</a>;
      renderHtml(content);
    } else {
      serverError(404, '404 Bad Request');
    }</pre>
  Since we have hardcoded the response of the view action, when the browser hits

  <em>/view/james</em> you will get:


  <pre class="brush:plain">&lt;h1&gt;Todos for james&lt;/h1&gt;</pre>


  <strong>Conclusion</strong>
</p>



<p>
  There it is. We have a working <strong>dispatcher </strong>and now we can start to deal with requests on our webserver. In <a href="http://www.jblotus.com/2011/06/09/building-your-first-node-js-app-%E2%80%93-part-3-view-controller-pattern-w-mustache/">part 4 of my node js tutorial series</a>, I am going to show you how to implement a view template and start dealing with the actual functionality of our web app. As you can see, Node really makes you do a lot of the setup work for yourself. There are several modules out in the wild that handle this sort of thing for you, but learning the low level api will be of the most benefit for future node js ninjas.
</p>

 [1]: http://www.jblotus.com/2011/05/28/building-your-first-node-js-app-part-1-installing-node-on-windows-7/
 [2]: http://nodejs.org/
 [3]: http://www.cygwin.com/
 [4]: http://www.apache.org/
 [5]: http://nginx.net/
 [6]: http://en.wikipedia.org/wiki/Front_Controller_pattern
