---
title: 'Building your first node.js app – Part 3: View-Controller pattern w/ Mustache'
author: jblotus
layout: post
dsq_thread_id:
  - 326285079
categories:
  - Front Controller
  - Javascript
  - MVC
  - node.js
  - Web Development
  - Programming
tags:
  - controller
  - dispatcher
  - javascript
  - javascript templating
  - mustache
  - mustache js
  - mvc
  - node
  - node.js
  - nodejs
---
In [part 2 of my node.js tutorial][1], we built the web server and created a dispatcher to handle the incoming client browser requests and route them to actions. In part 3 of this series,  I cover refactoring our [Node][2] app into separate node modules and components. I am also introducing the concept of a controller and views, the [Node Package Manager (NPM)][3] and the ubiquitous [Mustache templating language][4]. By the end of part 3 we will have a good foundation for a Node MVC framework.

**Before We Dive In **(Refactoring) ** **Right now would be a good time to think about how we want to structure our node application going forward. We want to have the dispatcher, controller, model and view as separate objects. This makes sense because we want each object to be responsible for as little as possible. This will help to keep the system more decoupled. We need to extract functionality out of our dispatcher that actually belongs in a controller, and move any display logic into a view component.

So, in essence we are creating our own Node.js MVC ([Model-View-Controller][5]) framework. I will be up front and admit that applying the MVC paradigm to node is somewhat difficult if you are used to working in something like CakePHP or Rails. It feels a bit like putting a square peg in a round hole and perhaps doesn't quite leverage the true strengths of Node. I still think exploring this path will be a good step into learning about some of node's differences with your typical web stack.

**Where were at**

Currently we are storing our actions in a object called $actions inside the dispatcher.

```
/*
 * /lib/dispatcher.js
 */
var actions = {
  'view' : function(user) {
    return '<h1>Todos for ' + user + '</h1>';
  }
}
```
What I want to do is break this object out into a separate component and store the different actions in there. The controller is going to be responsible for passing data off to the view. The controller might also want to get data from a Model object, which will contain the application's business logic. Let's go ahead and create the controller now.

###Create the controller
Drop this bit of code in */lib/controller.js*

```
/*
 * /lib/controller.js
 */
var controller = function() {};
module.exports = new controller();
```

Inside of this file, we are in module scope. Any file that does a <strong>require </strong>on this module only has access to exported methods. What I am doing in this file is creating a constructor function (<em>var controller) </em>for the controller object and telling Node that it should return this object when a require is performed. You can export individual functions, but I prefer to export an entire object with prototype methods and properties using the <a href="http://geekswithblogs.net/liammclennan/archive/2011/02/06/143843.aspx">Javascript class pattern</a>. Let's go ahead and move our actions object from <em>dispatcher.js</em> into the controller.

```
/*
 * /lib/controller.js
 */
var controller = function() {};

  controller.prototype = {
    'view' : function(user) {
      return '<h1>Todos for ' + user + '</h1>';
    }
  };

  module.exports = new controller();
```

Here I converted the actions object into a prototype definition for our controller object. Now each "action" will actually be a function inside the controller object. So now when you call <em>controller.view()</em> it will return a html string that gets spit out to the browser.

Now let's modify the dispatcher so it requires our <em>controller.js</em> module, and calls the appropriate action when requested by the browser.

###Modifying the dispatcher
The role of the dispatcher will be to call an <em>action method</em> inside the controller matching the string fragment supplied in-between the requested url's first set of forward slashes. The dispatcher will also pass a single argument to the controller, the string in-between the second set of forward slashes. Here is how the requested url is broken down:


```
/*
 * requested url: /view/james
 *
 * action: 'view'
 * argument: 'james'
 */
```
The dispatcher will need to pass a

<strong>callback function</strong> to the controller to be executed when the controller is finished doing it's business. The reason a callback is needed is that we don't know if the controller will be calling asynchronous methods. We can't simply call the controller, expect it to put data into the view, and return a view due to the asynchronous nature of Node. (<em>Although there are several synchronous, blocking equivalents of most functions</em>).

Another thing that I want to do is move some request/response handling into a module so we can keep the dispatcher focused more on dispatching requests and less on handling responses. Let's first create our response handler in <em>/lib/response_handler.js</em>

```
/*
 * /lib/response_handler.js
 */
  var fs = require('fs');
  // Constructor
  var response_handler = function(res) {
    this.res = res;
  };

  // properties and methods
  response_handler.prototype = {
    //store the node response object so we can operate on it
    res: {}
  };

  // node.js module export
  module.exports = response_handler;
```

This first thing I did was require the node <em>fs</em> module to deal with file operations. I also set up the prototype response_handler class for export as a module.

The response handler object takes a constructor argument which stores a copy of a <strong>res</strong> (Node Response) object. When we require the response handler in our dispatcher, we will need to re-instantiate it with a copy of the response object that exists in the dispatcher (and was passed in by the http module).

Here is how I modified the dispatcher to handle our new response handler:


```
/*
 * /lib/dispatcher.js
 */
var fs                      = require('fs');
var url                     = require('url');
var controller              = require('./controller');
var response_handler        = require('./response_handler');```

//include our reponse handler
var response_handler = require('./response_handler');

  this.dispatch = function(req, res) {
    //set up response handler passing res object
    responseHandler = new response_handler(res);

  //more code
  };
```

I also want to add some convenience methods to the response handler to deal with common operations like outputting an http error code, sending html to the browser, determining headers via file extension and allowing for arbitrary files to be server from the <em>/app/webroot/</em> directory.

Let's add a http error handler:

```
/*
 * /lib/response_handler.js
 */
//inside prototype
  serverError : function(code, content) {
    var self = this;
    self.res.writeHead(code, {'Content-Type': 'text/plain'});
    self.res.end(content);
  },
//inside prototype
```

This method simply serves up an http error code and writes it to the browser. So in the dispatcher we can call

<em>response_handler.serverError(404, 'Missing Page')</em> to send a 404 message.

Next, a html renderer:

```
/*
 * /lib/response_handler.js
 */
//inside prototype
  renderHtml : function(content) {
    var self = this;
    self.res.writeHead(200, {'Content-Type': 'text/html'});
    self.res.end(content, 'utf-8');
  },
//inside prototype
```
I created a <strong>renderHtml()</strong> method to simply output a string of html to the browser with the correct headers. You will use this one a lot.

###Creating a webroot folder
We want to be able to serve up static files and arbitraty content, much like a public_html directory on a cpanel hosting account. Create the folder <em>/app/webroot/</em>. This folder will store all of the public files for the application. This should be the only part of your project that should be publicly accessible and will hold your css, js and other static assets. Here is the method that I built to handle webroot requests:

```
/*
 * /lib/response_handler.js
 */
  renderWebroot : function(requestedUrl) {
    var self = this;
    //try and match a file in our webroot directory
    fs.readFile('./app/webroot' + requestedUrl.href, function(error, content) {
      if (error) {
        self.serverError(404, '404 Bad Request');
      } else {
        var extension = (requestedUrl.pathname.split('.').pop());
        self.res.writeHead(200, self.getHeadersByFileExtension(extension));
        self.res.end(content, 'utf-8');
      }
    });
  }
//inside prototype
```

This method takes a node url object passed in which was generated in the dispatcher using <code>url.parse(req.url)</code> and tries to find a file to serve. If you asked for <em>/css/style.css</em> this method will look for <em>/app/webroot/css/style.css</em>. If Node doesn't find the file, we tell our response handler to render a 404 error. If we do find the file, we still have to figure out which headers to send to the browser. To accomplish this I created a simply header parsing method which analyzes the file extension of the request href.
```
/*
 * /lib/response_handler.js
 */
  getHeadersByFileExtension : function(extension) {
    var self = this;
    var headers = {};

    switch (extension) {
      case 'css':
        headers['Content-Type'] = 'text/css';
        break;
      case 'js':
        headers['Content-Type'] = 'application/javascript';
        break;
      case 'ico':
        headers['Content-Type'] = 'image/x-icon';
      default:
        headers['Content-Type'] = 'text/plain';
  }
  return headers;
  },
  //inside prototype
```

That should take care of the response handler. We are going to implement these methods in the dispatcher a bit later. Now, I want to talk about the views.

###Creating the View
We need to define some rules:

<ul>
  <li>
    The view is essentially a container for content that will be output by the application.
  </li>


  <li>
    The controller gives the view some data, tells the view to compile itself
  </li>


  <li>
    The view compiles to html via <strong>Mustache</strong>
  </li>


  <li>
    The view component puts the html into a layout template using <strong>Mustache</strong>
  </li>


  <li>
    The layout is just a template that consists of a basic html page, and is actually just another type of view
  </li>


  <li>
    Finally, the view component calls the <em>response_handler</em> and tells it to output html to the browser
  </li>

</ul>


Views for the controller actions are all being stored in an actions subfolder in <em>/app/views/</em>. Layouts will be stored in <em>/app/layouts/</em>. The filename convention for a view is the underscored, lowercased name of the action, a period, and the format extension.

#Folder structure:
```
/app/views/
/app/views/actions
/app/views/actions/view.html
/app/view/layouts/default.html
```

Here is the view to for the "<strong>view</strong>" action:

```
{% verbatim %}
<h1>
  Todos for {{user}}
</h1>
{% endverbatim %}
```
You can see that it is pretty bare bones. What Mustache will do is take the user data from the controller action and replace it in this template. We will then collect that html into a view. I don't want to get into a huge overview of the Mustache syntax, so I would ask you to visit the <a href="https://github.com/janl/mustache.js">Mustache JS Documentation</a>.

###Installing the Mustache Module for Node
To install Mustache, we should use the<strong> Node Package Manager (NPM) </strong>which will install Mustache as a common module so we can use it in any project. <a href="http://npmjs.org/">Click here for complete instructions for installing Node Package Manager</a>.

Once we have Node Package Manager installed go ahead and install Mustache:

```
jblotus@jblotus-PC ~/sites/todos
$ npm install mustache
```

NPM 1.0.13 will install the module local to your project in the .node_modules folder. Now that we have Mustache installed we can use it as a module. We are now ready to create our view module. Create the following file, <em>/lib/view.js</em>

```
/*
 * /lib/view.js
 */
var fs = require('fs');
var Mustache = require('Mustache');
var view = function() {};

view.prototype = {};
module.exports = new view();
```

We create a view prototype class, making sure to include the Mustache module at the top. Next we need a method to render a view which will be invoked by the controller.

```
/*
 * /lib/view.js
 */
view.prototype = {

  renderView : function(name, data, callback) {
      var self = this;

      if (typeof callback !== 'function') throw ViewCallbackException;

        self.getView(name, 'html', function(content) {
          var template = Mustache.to_html(content, data);

          self.getLayout({}, function(content) {
            content = self.setLayoutContent(content, template);
            callback(content);
       });
    });
  },
};
```

Inside the prototype, I added a <strong>renderView()</strong> method, which takes a view name (<em>string</em>), some data (<em>object</em>), and a callback function. The <strong>renderView()</strong> method first checks to see that we supplied it a valid callback function. Then it fires off a call to the <strong>getView()</strong> method located in the same prototype (which have have yet to create). The <strong>getView()</strong> method is going to take a view name (<em>string</em>), a format (<em>string</em>) and finally, another callback function. This second callback function is going to take the compiled view and inject it into a layout. Finally the original callback passed to <strong>renderView()</strong> will be executed and passed the compiled layout as an argument.

###On Callback functions
The callback function is important enough to throw an exception if we don't supply one. The callback function the view's link to the browser. Since the view is performing a file read operation that is asyncrounous in node, we wont be able to capture that output and hand it off the the response handler. We need to actually pass the response handler method we want in as a callback so it can be fired off when the file is ready. This is one of those things that takes a bit of practice to get the hang of when coming to Node from a threaded, blocking environment.

Before I get back to the controller and dispatcher to demonstrate the callback, I want to show you the other methods in the view module.


```
/*
 * /lib/view.js
 */

  //inside prototype
  getView : function(name, format, callback) {
    var self = this;

    if (!name) {
      return '';
    }

    var format = format ? format : 'html';
    var path = './app/views/actions/' + name + '.' + format;

    // callback handling
    var callback = (typeof callback === 'function') ? callback : function() {
  };

  fs.readFile(path, 'utf-8', function(error, content) {
    if (error) {
      throw ViewNotFoundException;
    } else {
      callback(content);
    }
  });
  },
    //inside prototype
```

The <strong>getView()</strong> method basically looks in <em>/app/views/actions/</em> for a view file matching the supplied name + format string. The <strong>foo</strong> action would map to <em>/app/views/actions/foo.html</em> as an example. I am leaving the format argument in even though we are currently only returning html. In the future we might wish to return json, xml or perhaps some other format. If the view file can not be found, and exception is throw which will bubble up until it is caught. I intend to catch it in the dispatcher or response handler and render a 500, or perhaps 404 error. If the view file is found, the supplied callback function is executed with the content of the file as an argument.

See how we aren't returning the content to the <strong>renderView() </strong>method? That's because of all this file reading business. So the trick in Node is to think in terms of supplying your methods with the functionality they need in the form of a callback function that can be passed along. I like to think of it like sending out instructions with your package. The recipient is going to need to know what to do once it gets to their house. You as the send will not have total control as to when the package is actually recieved so you need to ensure that the receiver has what he needs.

At this point in the flow, <strong>renderView()</strong> has called <strong>getView()</strong>, and given it a closure that compiles the unprocessed view template and puts it into a layout. It needs to also get the layout.

```
/*
 * /lib/view.js
 */
  //inside view prototype
  getLayout : function(options, callback) {
    var self = this;
    var options = options ? options : {
      'name' : 'default',
      'format' : 'html'
    };
    var name   = options.name ? options.name : 'default';
    var format = options.format ? options.format : 'html';

// callback handling
var callback = (typeof callback === 'function') ? callback : function() {
};

var path = './app/views/layouts/' + name + '.' + format;

fs.readFile(path, 'utf-8', function(error, content) {
  if (error) {
    throw LayoutNotFoundException;
  } else {
    callback(content);
  }
});
  }
    //inside view prototype
```


This method is extremely similar to the <strong>getView()</strong> method in the sense that it takes a view name (<em>string</em>), format (<em>string</em>) and callback function. In fact I may wish to refactor that in the future. It will pull up the layout file and execute the supplied callback function, which was defined in this case by the <strong>renderView()</strong> method. The <strong>renderView()</strong> method will take this raw file string and compile it using Mustache, passing in the view action's compiled html string as the data.


```
//inside view prototype
  setLayoutContent : function(layout, content) {
    var self = this;
    var layout = layout ? layout : '';
    var context = {
      'content_for_layout' : content ? content : ''
    };
    return Mustache.to_html(layout, context);
  },
  //inside view prototype
```

###Building the Layout
Create the following file <em>/app/views/layouts/default.html</em>

```
{% verbatim %}
  <!DOCTYPE html>
  <head>
    <meta charset="utf-8">
    <title>Todo List in Node</title>
    <link rel="stylesheet" href="/css/style.css">
  </head>


  <body>
      {{{content_for_layout}}}


  <!-- Grab Google CDN's jQuery, with a protocol relative URL; fall back to local if offline -->
  <script src="//ajax.googleapis.com/ajax/libs/jquery/1.6.1/jquery.min.js"></script>
  <script>window.jQuery || document.write('<script src="js/libs/jquery-1.6.1.min.js">\x3C/script>')</script>

  <script src="/js/script.js"></script>
  </body>
</html>
{% endverbatim %}
```

This is going to serve as the layout for the web application.  It is just a regular HTML5 page, including out css in the HEAD, which we create in <em>/app/webroot/css/style.css</em>
Also we have included jQuery from the Google CDN in addition to a fallback copy which is placed in <em>/app/webroot/js/libs/jquery-1.6.1.min.js</em>
I have made a separate file for our application javascript in <em>/app/webroot/js/script.js</em>

You will see a Mustache template variable called `content_for_layout`

The view for the requested action is going to drop in the layout in place of this variable. You will notice I used 3 <em>mustaches/handlebars/curlys</em> to indicate to Mustache not to escape the data it puts here, otherwise we will end up with html entities being encoded which is not what we want since we need to get back rendered html from Mustache.


###Wiring up the controller to the view
We need to modify our controller to call up the view instead of just returning a string literal.

```
/*
 * /lib/controller.js
 */
var view = require('./view');
var controller = function() {};

controller.prototype = {
  view : function(user, callback) {
    var callback = (typeof callback === 'function') ? callback : function() {};
    var data = {
      'user' : user ? user : 'nobody'
    };

    view.renderView('view', data, function(data) {
      callback(data);
    });
 }
};

 module.exports = new controller();
```

I modified the <strong>view()</strong> action inside the controller to take a user name (<em>string</em>) and a callback function (from the dispatcher). If no callback is supplied we just make a empty function so we don't throw an error by calling a non existent function. The <em>data </em>object is populate with some properties which are then handed off to the view object. Since the view module has been included using <strong>require()</strong>, we call the <strong>view.renderView() </strong>method with the action name &#8216;<em>view</em>&#8216; (sorry bad name to start off with), the data for the view, and finally a callback function telling <strong>renderView()</strong> what to do once it has finished it business. This callback supplied to <strong>renderView() </strong>is simply the execution of the callback we supplied our controller with. That callback is going to come in from the dispatcher. In fact it would be a good idea to do that now.

At this point, I also want to add a view for our homepage, and the best way to do this is to simply create an action in the controller for it:

```
/*
 * /lib/controller.js
 */
//inside controller prototype
  home : function(arg, callback) {

var callback = (typeof callback === 'function') ? callback : function() {};

var data = {
  'users' : {
    'name'    : 'James',
    'viewLink': '/view/james/'
  }
};

view.renderView('home', data, function(data) {
  callback(data);
});
  },
```

Let's also create the view template for the &#8216;home' action. <em>/app/views/actions/home.html</em>

```
/*
 * /app/views/actions/home.html
 */
//inside controller prototype
```

```
<h1>
  Todo list in Node
</h1>
```
View Some todo lists:
```
{% verbatim %}
  {{#users}}
    <a href="{{viewLink}}">{{name}}</a>
  {{/users}}
{% endverbatim %}
```
Mustache will take the 'users' key of the data we pass to the view and iterate through it. From there I build a list of links to view the todo lists of each user. For now I just created a dummy user with the name james.


###Connecting the dispatcher to the controller
Let's take a look at the complete code for the dispatcher:

```
/*
 * /lib/dispatcher.js
 */
var fs                      = require('fs');
var url                     = require('url');
var controller            = require('./controller');

  this.dispatch = function(req, res) {

  //set up response object
    responseHandler = new response_handler(res);

  var requestedUrl = url.parse(req.url);
    var parts, action, argument;

  if (requestedUrl.pathname == '/') {
      action = 'home';
    } else {
      parts    = req.url.split('/');
      action   = parts[1];
      argument = parts[2];
    }

  //only executing registered actions
    if (typeof controller[action] == 'function') {
      try {
        controller[action](argument, function(content) {

        if (content) {
          responseHandler.renderHtml(content);
        } else {
          responseHandler.serverError(404);
        }
        });

      } catch (error) {
        console.log(error);
      }
  } else {
    responseHandler.renderWebroot(requestedUrl);
  }
};
```
The dispatch method receives a request object from server.js which is parsed into a url object. I split up the url to determine the requested action, routing to the &#8216;<em>home</em>&#8216; action if / is requested. The dispatcher then searches for a matching action name in our controller object. If it does not find one, it looks for a file matching the url in the <em>/app/webroot/</em> section of our site.

If the action is found in the controller, the first thing to do is wrap the whole mess in a <em>try/catch</em>. This way if an exception is thrown, it can be caught for debugging purposes. We then execute the controller action requested, passing it an argument and a callback function. The callback in this case is the response handler, and it needs to check and see if it needs to render the content or render an error page. I am giving it the <strong>renderHtml() </strong>method because at this point our application only supports getting back html from the controller, although that will change in the next tutorial.

At this point, everything should be wired up correctly. If you fire up your browser to the homepage at <em>http://localhost:1337/</em> you should get a simple page like this:

```
<h1>Todo list in Node</h1>
View Some todo lists:
<a href="http://localhost:1337/view/james/">James</a>
```

When you click the link, you will be treated to something like this fine headline:

```
<h1>Todos for james</h1>
```

###Part 3 Summary
This was a long tutorial and we covered a lot of group. To sum it up, we created separate components for our dispatcher, created the controller, and introduced views with Mustache. Wiring it all together was the toughest part! We have a fully functional webserver that we can add actions and views, so our foundation is starting to shape up. In part 4 of this Node js tutorial series  we will add the CRUD (create, update edit and delete) functionality to todo lists as well as performing user authentication for our application.

[1]: http://www.jblotus.com/2011/05/30/building-your-first-node-js-app-%E2%80%93-part-2-building-the-web-server-and-request-dispatcher/
[2]: http://nodejs.org/
[3]: http://npmjs.org/
[4]: http://mustache.github.com/
[5]: http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller
