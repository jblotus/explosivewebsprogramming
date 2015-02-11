---
title: 'First Impressions: Cloud9 IDE + PHP'
author: jblotus
layout: post

dsq_thread_id:
  - 992361849
categories:
  - IDE Love/Hate
  - PHP
  - PHPUnit
  - Web Development
  - Programming
tags:
  - ace editor
  - cloud9
  - cloud9 ide
  - development environments
  - online ide
  - pair programming
  - php
  - php ide
  - puppet
  - vagrant
---
I love programming, but admittedly I absolutely hate setting up local development environments. I have been using tools like [Vagrant][1] and [Puppet][2] to help ease the pain, but even then I find it tedious to get a quick project off the ground. This inevitably leads to me making some master development VM, but this stuff takes time and can kill an idea before I even lay down a single line of code. Well, what if I didn't need to bother with that boring stuff anymore? I have been hearing about the [Cloud9 online code editor][3] for a long time (based on the [ACE Editor][4]) and thought maybe it would be suitable for some [node.js][5] development. Little did I know how many features they are packing now for free into this awesome web applicaiton.

<!--more-->

### Your code anywhere

At first I though **Cloud9** would be a simple editor, perhaps a nice showcase of what you can do with HTML5 and some well-written javascript but it quickly became evident that this app was packing a lot of powerful features.

#### <span style="text-decoration: underline;" data-mce-mark="1">Features at a glance:</span>

  * Integrated linux sandbox environment with pre-installed PHP, Node, MySQL, etc
  * Inline Shell access (no sudo and some other restrictions)
  * Easy connection to GitHub and BitBucket
  * Remote, multi-collaborator real-time editing (Pair programming anyone?)
  * 512 MB of storage in the sandbox
  * Did I mention you can hop on to any computer and start coding right away?

Sounds pretty awesome if you think about it. You can even connect you own server via SSH instead of using the build in sandbox (this is a paid feature).

### Is this the Holy-Grail?

**Not Really, but it's close. **This is a very exciting product that almost reaches holy-grail status, but it falls short in a few key areas. Occasional stuttering, delays in the terminal and editor tabs are mood killers. Having to wait 5-10 seconds to save a file while in a cycle of test-driven-development is painful, which is something that happened to me quite frequently. You also will miss CTRL-Clicking into a method and other modern conveniences you get for free in editors like [PHPStorm, NetBeans, etc][6].

If you are a **VIM** person you probably won't be 100% happy with the responsiveness and let's face it, [you guys hate IDE's][7]. To be fair you can pull up VI in a terminal window, but there are also some problems there. If you are used to a native terminal client or [PuTTY][8], the provided HTML5 terminal has some quirks, CTRL-C doesn't work as expected and I also ran in to some issues with my local aliases disappearing.

### Let's use PHP 5.5

Some other complaints are that the built in versions for PHP are a bit low (5.3.3), an I'm not sure there is an obvious way to change this other than building from source. You do get **PEAR** and **PECL** pre-installed but I haven't tried to do anything serious with that yet. It looks like you do have the ability to compile from source, but it takes awhile. I was unable to successfully build memcached but I was able to build [PHP 5.5 alpha][9] (in about two hours) although due to permissions issues I had to do <span style="text-decoration: underline;" data-mce-mark="1">something like this</span>:

<pre class="brush:shell">$ ./configure prefix=$HOME
$ make
$ make install</p>



<p>
  $ php -v<br />
  PHP 5.5.0alpha1 (cli) (built: Dec 25 2012 15:17:46)
  Copyright (c) 1997-2012 The PHP Group<br />
  Zend Engine v2.5.0-dev, Copyright (c) 1998-2012 Zend Technologies</pre>
  If you know your way around linux, you will probably have more luck than I did, but your mileage may vary.
</p>



<h3>
  Is it FREE?
</h3>



<p>
  Well it seems that just like GitHub  you can create<strong> unlimited public workspaces, with unlimited collaborators.</strong> If you need more than one private project and only want to give your team access, or you want to connect your own VM instead of using the built in sandbox &#8211; you will probably want to cough up the<em> $12/month</em>. If you are strictly experimenting or working on open-source there isn't a lot to lose here.
</p>



<h3>
  The Verdict
</h3>



<p>
  The <strong>Cloud9</strong> team has done a great job removing the barriers to getting a simple idea out the door. I implemented <a title="FizzBuzz In PHP - A Test Drive of the Cloud9 IDE" href="https://github.com/jblotus/cloud9-php-fizzbuzz">FizzBuzz in PHP</a> using <a title="Composer PHP Dependency Management" href="http://getcomposer.org/">Composer</a>, <a title="PHPUnit Manual" href="http://www.phpunit.de/manual/current/en/index.html">PHPUnit</a>, and some<a title="PHP Standard PHP Library" href="http://php.net/manual/en/book.spl.php"> SPL interfaces</a> and got it all done , and posted to GitHub in about an hour. Since Apache is also installed, this is a really brain-dead simple way to prototype, or even work on non-trivial projects. I haven't tried the multi-user editing yet, but it actually sounds like a lot of fun. Also, the whole thing is available independently of the online service so you can install Cloud9 locally (<a title="Cloud9 IDE GitHub Source" href="https://github.com/ajaxorg/cloud9/">check out the GitHub repo</a>).  What are you waiting for? Go <a title="Cloud9 IDE Signup" href="https://c9.io/">sign up</a> and/or check it out locally and let me know what you think in the comments section!
</p>

 [1]: http://vagrantup.com/ "Go to the Vagrant Homepage"
 [2]: http://puppetlabs.com/ "Go to the PuppetLabs Homepage"
 [3]: https://c9.io/ "Go to the Cloud9 IDE Homepage"
 [4]: http://ace.ajax.org/#nav=about "Go to the ACE Editor Homepage"
 [5]: http://nodejs.org/
 [6]: http://www.jblotus.com/2012/06/29/why-cant-someone-just-make-a-good-ide-for-php/ "Why can’t someone just make a good IDE for PHP?"
 [7]: http://stackoverflow.com/questions/136056/ide-or-text-editor "IDE or Text Editor"
 [8]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html "PuTTY Terminal Client"
 [9]: http://downloads.php.net/dsp/ "PHP 5.5 Alpha"
