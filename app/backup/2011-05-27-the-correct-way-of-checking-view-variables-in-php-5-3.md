---
title: The correct way of checking view variables in PHP 5.3
author: jblotus
layout: post

dsq_thread_id:
  - 314863431
categories:
  - PHP
  - Web Development
  - Programming
tags:
  - error checking
  - error handling
  - error supression
  - mvc
  - php
  - php 5.3
  - shorthand ternary
  - views
---
### **<span style="color: #ff0000;">NOTE: Don't use the information in this article, consider it a discussion point at best. Suppressing errors like this leads to bad code.</span> - 09/05/2012**

If you are using an MVC framework, or any other type of template, you do a lot of error checking for the presence of variables, properties and array keys in your views. This is a pain point when trying to write clean and readable templates. I am recommending that instead of using isset() everywhere, we leverage the [@ (error suppression)][1] and [?: (shorthand ternary) operators][2] to test for variables.

Consider the following view:

<pre class="brush:php">&lt;?php
&lt;div&gt;
    &lt;p&gt;
      My name is &lt;?= $name ?&gt;.
       I am very &lt;?= $emotions['current'] ?&gt;
       when I &lt;?= $actions-&gt;enjoyMost ?&gt;
    &lt;/p&gt;
&lt;/div&gt;</pre> We have a few potential problems with this code. Does $name exist? How about the &#8216;current' key in $emotions? If it does exist, is it a string? Maybe $actions->enjoyMost is actually being called on a non-object?

Phew. That's a lot of potential error checking. I have come to the conclusion that you usually don't need to be particularly concerned with these situations when dealing with views. At worst, these variable can fall back to being an empty string and you don't have to deal with polluted error logs and E\_NOTICE, or E\_WARNING errors.

Here is the correct way to do it:

<pre class="brush:php">?php
&lt;div&gt;
    &lt;p&gt;
     My name is &lt;?= @$name ?: '' ?&gt;.
      I am very &lt;?= @$emotions['current'] ?: '' ?&gt;
      when I &lt;?= @$actions-&gt;enjoyMost ?: '' ?&gt;
    &lt;/p&gt;
&lt;/div&gt;</pre> E\_FATAL errors will still stop your program, and suppressing E\_NOTICE and E_WARNING won't create any issues with your template that wouldn't have existed anyway, minus the annoying error message (especially with xdebug!).

&nbsp;

 [1]: http://php.net/manual/en/language.operators.errorcontrol.php
 [2]: http://davidwalsh.name/php-shorthand-if-else-ternary-operators
