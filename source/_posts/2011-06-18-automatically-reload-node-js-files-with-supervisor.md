---
title: Automatically reload node.js files with Supervisor
author: jblotus
layout: post
dsq_thread_id:
  - 335878606
categories:
  - Javascript
  - node.js
  - Web Development
  - Programming
tags:
  - javascript
  - node
  - node.js
  - node.js error handling
  - node.js server restart
  - node.js supervisor
---
One of the annoyances of [Node.js][1] for development purposes is the fact that you have to restart the server everytime you make changes to your included modules. This is because Node actually caches module includes for better performance. A great workaround is using the Node Module "[Supervisor][2]".

Supervisor will monitor all of your included modules and restart the server automatically when changes are made.

Installing supervisor is easy with [NPM (Node Package Manager)][1]
```
npm install -g supervisor
```
This will install supervisor globally which is probably a smart idea. Invoking your server with supervisor is also really easy:

```
supervisor server.js
```
Here is an example of the startup output:

```
DEBUG: Running node-supervisor with
DEBUG:   program 'server.js'
DEBUG:   --watch '.'
DEBUG:   --extensions 'node|js'
DEBUG:   --exec 'node'</p>

DEBUG: Starting child process with 'node server.js'
DEBUG: Watching directory '/home/Owner/sites/Node-JS-todo-list-tutorial/.' for changes.

Starting server @ http://127.0.0.1:1337/
Server running at http://127.0.0.1:1337/
```

Now if I make a change in server.js or any included modules:

```
DEBUG: crashing child
DEBUG: Starting child process with 'node server.js'
Starting server @ http://127.0.0.1:1337/
Server running at http://127.0.0.1:1337/</pre>
```
If the saved changes have errors, supervisor will output the errors, but keep trying to restart the server.

```
DEBUG:
node.js:134
    throw e; // process.nextTick error, or 'error' event on first tick
DEBUG:         ^
DEBUG: ReferenceError: zxc is not defined
    at Object.<anonymous> (/home/Owner/sites/Node-JS-todo-list-tutorial/server.js:10:1)
    at Module._compile (module.js:407:26)
    at Object..js (module.js:413:10)
    at Module.load (module.js:339:31)
    at Function._load (module.js:298:12)
    at Array.<anonymous> (module.js:426:10)
    at EventEmitter._tickCallback (node.js:126:26)

DEBUG: Starting child process with 'node server.js'
Starting server @ http://127.0.0.1:1337/
```
Supervisor seems like a great module to keep your Node App running fresh. There are also a few command line options for supervisor that might come in handy that I may explore in a future post. For now I recommend using supervisor at least in a development environment to save you fingers from a few extra <strong>CTRL-C</strong> stretches.

 [1]: http://npmjs.org/
 [2]: https://github.com/isaacs/node-supervisor
