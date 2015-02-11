---
title: Simply scale with Nginx, Memcached, PHP-FPM and APC
author: jblotus
layout: post

dsq_thread_id:
  - 1059208034
categories:
  - Caching
  - MySQL
  - nginx
  - PHP
  - Scaling
  - Web Development
  - Programming
tags:
  - apache
  - apc
  - async
  - fastcgi
  - memcache
  - memcached
  - mysql
  - nginx
  - php
  - php-fpm
  - scaling
  - web servers
---
{% block excerpt %}
When I started programming in PHP, my hosting setup involved a cPanel installation with Apache and MySQL. This has been the de facto standard for many PHP developers and for the most part I don't think any of that needs to change. I simply never had to deal with websites that got more than 10,000 visits a day. This all changed when I started work at my current job a few years ago. We sell an educational product that serves a predictable 15,000 requests per minute for 10+ hours/day, every day. Instead of Apache, we use nginx with [PHP-FPM][1] to handle this traffic. This is becoming a very popular setup for many companies with non-trivial traffic, but I have also found success with it in my small [256MB Ram VPS][2]. For various reasons, nginx does a better job with memory and concurrent connection handling than Apache. In this post, I want to talk about some of the reasons you might want to go with this setup.
{% endblock %}

{% block content %}
### I/O is slow

CPU's are fast.[ L1 cache and L2 cache is the nuts][3]. Next in line, you want to live in RAM. Since we can't fit everything into CPU cache or RAM, we end up going with some sort of disk. You could go SSD, but most systems are some type of spinning platter hard drive. RAID and disk cache speeds it up, but eventually things like fragmentation and load slow down access speeds. We also have swap situations which murder our performance, when the OS has to dump memory to disk to fill it up with new things constantly, and shuffle between them (overburden). Then you have things like network access, which in some cases is faster than disk depending on what they do. One example of that is memcached. If you save processing on a database server that might need to analyze millions of records by pulling the results directly out of memory, you have created huge efficiency gains.

### Nginx is also a win for efficiency

Nginx is low-profile, event-driven web server that excels at serving multiple clients due to it's asynchronous nature. Here a few ways that people commonly use nginx:

  * reverse proxy for Apache
  * primary web server
  * reverse proxy for php-fpm

#### So what is a reverse proxy?

It's basically a program that sits in front of other servers in your stack. A request comes in to nginx and it will be forwarded along to other machines. This could be used to load-balance your application servers, perhaps running Apache with mod_php. To understand why we would want to do that we need to understand a bit about how nginx handles connections and resource usage compared to Apache.

Some reasons Apache has problems under load:

  * Apache holds connections open until they complete
  * Those connections use a thread, and each thread loads all Apache modules
  * These Apache threads suck up memory and slow down other threads
  * High memory usage leads the operating system into swap
  * Swap means that memory has to be dumped to the hard-drive temporarily
  * Hard drives are slow, remember?
  * Performance degrades across the entire machine
  * Most of the time Apache spends in a thread is simply waiting, waiting, waiting

When Apache handles a web request, typically all the modules Apache needs are loaded to handle that connection, e.g. mod_php. This is not really a desirable thing when you are serving static assets like images, JavaScript or css. The reason it is not desirable is that for every connection, Apache forks a new process to handle it (with prefork the default method). This leads to a few problems: more processes = more memory and CPU  Remember that Apache is loading all its modules for each request, so there is a lot of waste here. For a single web page request, you might pull in a ton of assets, meaning Apache has to create a process for each one, and holds the process until the response has been delivered back to the client. This can quickly get out of hand and if the problem is not addressed via some caching layer like varnish, the machine will start utilizing all available memory and go into swap. Swap is bad because we are shuffling things back and forth from disk to memory.

You can tweak Apache to use worker mpm instead of prefork, which changes the way apache runs to more efficiently handle this problem, but then we get into an issue of thread safety. I don't think most programmers want to be concerned about threads in general and PHP has a reputation for not being entirely thread safe to begin with.

### So what does Nginx do differently compared to Apache?

  * nginx uses a set number of "workers" (Apache 2.4 has something similar now) and those workers utilize a predictable number of threads.
  * when a request is received, the response is asynchronous so the thread won't stay open until it has something to do. When the web server has a response ready it will be sent back to the client.
  * This is called "never block, finish fast". Think of an event loop in JavaScript.
  * Most static assets are buffered to memory so they get served very fast

Nginx handles this problem by using the Reactor pattern, which is another way of saying it is event driven. When a request comes in, nginx passes it off to the appropriate handler, but it does not hold a process or thread open waiting for the response! Only when the response is ready does nginx send it back to the client. Nginx also buffers static files to memory for increased performance, meaning that the hard disk doesn't have to be touched for some requests. I would argue the majority of requests against a web server are for static assets.

Now if you run nginx in front of Apache then you are essentially keeping Apache busier and it doesn't have to waste time dealing with asset delivery to the client, thereby leading to more efficient memory usage since less connections have to be maintained as threads/processes. This can be a very effective strategy if you don't want to completely replace apache in your stack. Nginx does this via proxy pass.

Ditching Apache might not be possible for you, especially if you are doing any sort of client hosting, since we lose the convenience of htaccess files. By the way, htaccess files are another performance killer for apache stacks since the file system has to be checked often (starting to see a theme here? file system is slow). If you have AllowOverride enabled, Apache also tries to read any .htaccess files before serving a page, which is another hook into the file system to degrade performance.

I forego Apache entirely, but we still need a way to handle PHP requests. Nginx does not have a PHP "module" like Apache does. Instead we use the FASTCGI protocol to send requests to a program called PHP-FPM.

#### But wait, what is FastCGI?

[FastCGI][4] is just a common protocol for web servers to talk to other programs.

### Is PHP-FPM just mod_php without Apache?

PHP-FPM is a daemon that listens on a port or socket for php requests and it uses a similar worker setup to nginx, in that a main daemon controls several worker threads when FastCGI requests come in. You have a fixed or variable number of "workers" that can execute the PHP code and return a response back upstream to whatever requested it, typically the webserver. People often compare PHP-FPM to mod\_php without Apache. That's not really fair because Apache can also use PHP-FPM instead of mod\_php via the FASTCGI protocol.

One advantage of PHP-FPM is that it can run as a seperate user than the web server  This is a win for security since exploits that affect the webserver or php itself are isolated from eachother. Another nice thing about PHP-FPM is that you can reload and restart it without having to touch the web server  and vice-versa. That means that if you upgrade or reconfigure your web server or PHP you don't have to restart the whole shebang.

At the end of the day, PHP-FPM is just another way to use the same language you know and love.

### Memcached

To increase our performance even more, we want to start caching expensive things that our application server needs to do. At work we use a master-slave MySQL setup and generally try to pull reads from our read-only database. This is great for spreading the load, but we also want to be caching database query results whenever possible. In fact, most of the bottlenecks you will run into when trying to scale is the database itself. Tuning queries and indexing are very important, but caching is also something that you should look into. Even lot's of small queries can add up and it is much harder to scale out a MySQL database than to add extra memcached instances.

Memcached is a proven piece of software often used to deal with this problem and you can get great results by running an instance that reserves as little as 25mb of ram for storage. Memcached is two things, a client and server. The server is an event-driven simple key-value store that runs as a daemon. The OS talks to memcached via a client library. PHP uses an extension, either php\_memcache or php\_memcached to attach to the native memcached client library, the latter understanding how to communicate with the server daemon.

#### For connecting to memcached with PHP, you have two options:

#### [php_memcache][5] or [php_memcached][6]

php_memcache(d) with a "d" is the one you really want to be on since it is more actively developed. Unfortunately if you are on windows, you are out of luck and you will have to use [php_memcache with no "d"][7]. For a really awesome talk about memcached (and APC) I recommend listening to [DPCRadio: APC & Memcache the High Performance Duo][8] by [**Ilia Alshanetsky**][9] who is a core PHP developer and one of the maintainers of php_memcached.

### APC

When you run a php script, it is first tokenized, then turned into byte-code (opcode) that the php interpreter can run at a lower level on the machine. The idea behind APC is that often times we don't need to perform this step since the php opcode is mostly that same for many pages. APC will store this compiled byte code, which can dramatically speed up performance.

Running APC is a no-brainer, but APC may need to be tuned a bit to fit your specific situation. Just like any caching, staleness is a factor, so APC has to occasionally check for newer versions of a script. Most of the default settings for APC should be fine for lower loads, but if you have a non-trivial amount of traffic you will want to more closely examine the various settings. I have personally taken down servers with bad APC settings.

### Wiring it together

The setup we use at work involves a single front-end web server running nginx, and a number of commodity application servers only running php-fpm. All static assets are served from the web server machine so nginx can keep them in memory as often as possible. When a request comes in for a web page, we will send it off to an application server via round robin. (there are several server selection methods available). The application servers only have one job, serving PHP. This approach allows us to very easily add or remove extra servers as needed.

One challenge with this setup is synchronizing the code. We have a deploy process that will rsync our project code to the individual application servers. For user sessions, we store them in memcached so that we don't worry about users jumping to different servers on subsequent requests.

### Bonus Tips

  * <span style="font-size: 14px; line-height: 1.6em;">Don't forget to set up your expires headers. See this<a href="http://www.youtube.com/watch?v=HKNZ-tQQnSY&list=UUkQX1tChV7Z7l1LFF4L9j_g&index=13"> awesome google presentation on the subject</a> (cache is king).</span>
  * Set up some monitoring to measure your results. We use cacti, nagios, and newrelic at work.
  * Don't forget to test that your cache mechanisms are actually working.

### Not an Apache Bash

I don't want this post to come off as an Apache bash,  and I merely present to you these opinions based upon my current experience. I think Apache is a great piece of software and I use it in many contexts. Any piece of software you add to your stack has the potential to perform better with adequate tuning, including Apache. I don't want to present benchmarks because your situation is going to be different from mine. But the point here is that I have used these technologies successfully and you may achieve similar results, so don't be scared to explore them. Thanks for reading!

 [1]: http://www.php.net/manual/en/install.fpm.php
 [2]: http://www.linode.com/?r=8ab874aa33b39b129030c8e53132a9d5ce87a06f
 [3]: http://en.wikipedia.org/wiki/CPU_cache
 [4]: http://en.wikipedia.org/wiki/FastCGI
 [5]: http://pecl.php.net/memcache
 [6]: http://pecl.php.net/package/memcached
 [7]: https://github.com/php-memcached-dev/php-memcached
 [8]: http://techportal.inviqa.com/2010/08/03/apc-memcache-the-high-performance-duo/
 [9]: http://ilia.ws/
{% endblock %}
