---
title: 'Building your first node.js app - Part 1: Installing node on Windows 7'
author: jblotus
layout: post
dsq_thread_id:
  - 316386882
categories:
  - Javascript
  - node.js
  - Web Development
  - Programming
tags:
  - cygwin
  - node
  - node.js
  - node.js installation windows
  - node.js troubleshooting
---
After watching a few videos on [thinkvitamin membership][1], the node.js hype finally got to me. If you haven't heard of [node.js][2] before, it is an application framework which allows you to write server side programs using javascript. An obvious use case for node.js is as a web server and we are going to use it to create a simple todo list web application. This application will cover the basics of setting up a simple web server and explore a few of the core node.js libraries for dealing with common web development problems like request/response handling, authorization and file access.

**The Plan**

Our application is going to allow a web surfer (client) to populate a list of todo items for his profile. We will be authorizing the user, and displaying a public profile page with all of the users todo items. Only the user will be able to perform **CRUD **operations on the todo list.**

**For the purposes of this node.js tutorial, we will simply store each todo item as a serialized javascript object in a file on the system in a namespace directoy per user. This will demonstrate some of the file handling capabilities of node. In practice however, you would probably use a database to do this, which I will cover in a future tutorial.**

I will be building the application on my local machine, but you can also find node.js hosting out there in the wild. (see [google][3]). Now my machine is a windows 7 box, so I will be going over the steps to installing node on that platform using that platform. Unfortunately you will need to install a nix-like environment like cygwin on your windows machine to use node. You will also need Python to actually build the node binary. The steps to installing on a nix environment are actually fairly similar to the cygwin process, but I will not be covering them. In addition, on Windows 7/Vista there are some additional troubleshooting steps that need to be taken to get node to build successfully.

**Step 1: Install Dependencies**

**UPDATE: joyent (who employs Ryan who created node) has posted a [great tutorial to get node up and running with cygwin][4]. This would be a great place to start if my directions don't work for you.**

Download and install [cygwin][5] . Make sure to add:

  * `wget`
  * `gcc4-g++`
  * `git`
  * `make`
  * `openssl`
  * `openssl-devel`
  * `pkg-config`
  * `zlib-devel`
  * `python`

**Step 2: Build Node**

Open a cygwin prompt.

```
cd /usr/src
```

Run the following sequence of commands to download and extract node.js:

```
wget http://nodejs.org/dist/node-v0.4.8.tar.gz
tar xvzf node-v0.4.8.tar.gz
cd node-v0.4.8
```

Time to install. First we need to run the configure tool.

```
./configure
```

If configure fails for you like it failed for me, you will have to take some additional steps. After poking around for a bit I found another great article on <a href="http://boxysystems.com/index.php/step-by-step-instructions-to-install-nodejs-on-windows"> installing node.js on windows</a> which has some advice for dealing with node.js installation problems. Here is what worked for me:


<ol>
  <li>
    Exit all Bash shells
  </li>


  <li>
    Launch C:\Cygwin\bin\ash as an Administrator
  </li>


  <li>
    (Vista users) Edit line 110 of c:\cygwin\bin\rebaseall to say:
    <pre>sed -e '\=/sys-root/mingw/bin=d' -e '\=cygwin1\.dll$=d' -e '\=cyglsa.*\.dll$=d' -e 's=^=/=' >"$TmpFile"</pre>

  </li>


  <li>
    Run /usr/bin/rebaseall
  </li>

</ol>

With that out of the way, it is time to retry the build process. Open back up your cygwin prompt and cd to the node src dir and enter the following commands.

```
./configure
./make
./make install

node -v
```

should output

```v0.4.8```

<strong>Still with me?</strong>

If you are getting stuck on the configure step, be sure that you grabbed all the required packages and install using the setup.exe you initially used to install cygwin. Otherwise, you can visit this article on <a href="http://boxysystems.com/index.php/step-by-step-instructions-to-install-nodejs-on-windows">installing node.js for window</a>s to get some more possible tips for dealing with your installation headache.

###Conclusion
To be honest, the installation of node.js on Windows 7 with cygwin was actually a bit painful and it would be easy to get stuck on this step. If you can't get node running, I would recommend installing on a virtual machine running CentOS, Ubuntu or some other brand of linux. Next, in <a href="http://www.jblotus.com/2011/05/30/building-your-first-node-js-app-%E2%80%93-part-2-building-the-web-server-and-request-dispatcher/">part 2 of my node.js tutorial</a>, we will set up our webserver and get it responding to user requests.

 [1]: http://membership.thinkvitamin.com/?aid=212&utm_source=aff
 [2]: http://nodejs.org/
 [3]: http://www.google.com/search?aq=f&sourceid=chrome&ie=UTF-8&q=node.js+hosting
 [4]: https://github.com/joyent/node/wiki/Building-node.js-on-Cygwin-(Windows)
 [5]: http://cygwin.com/install.html
