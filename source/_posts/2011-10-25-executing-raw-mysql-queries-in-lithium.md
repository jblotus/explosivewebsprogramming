---
title: Executing raw MySQL queries in Lithium
author: jblotus
layout: post

dsq_thread_id:
  - 452390981
categories:
  - Lithium PHP
  - MySQL
  - PHP
  - PHP Frameworks
  - SQL
  - Web Development
  - Programming
tags:
  - lithium php
  - lithium sql
  - mysql adapter
  - php
  - raw sql in lithium
  - sql queries in lithium
---
Note: see comments below for a better solution

I&#8217;ve been working on a project that is using the [MySql adapter for Lithium ][1]and I keep running into roadblocks when it comes to using some MySQL specific features. Unfortunately the adapter for MySQL that comes with Lithium doesn&#8217;t quite handle all of the features I need and I was breaking my head trying to find an easy way to simply execute a direct query against the database. The `MySql::_execute()` method is the actual method that performs the MySQL queries, and it&#8217;s visibility is protected. The `\lithium\data\source\Database` class actually provides an API for queries via `Database::create()`, `Database::read()`, `Database::update()` and `Database::insert()` but those methods don&#8217;t actually work for all situations.

**Operators in SET clause** `Model::connection()` returns an instance of `\lithium\data\source\database\adapter\MySql` (which is a subclass of `\lithium\data\source\Database`) and you could use this to do an update, but operators are not supported in the SET clause of the generated sql.

Here is my solution for simply executing a query:

<pre class="brush:php">$sql = "UPDATE list_items SET completed = NOT completed WHERE id = $id";
Model::connection()-&gt;invokeMethod('_execute', array($sql));</pre> This is not pretty, and certainly feels like a violation of

[The Law Of Demeter][2], but it works. In an update, this should simply return a boolean true, but in the case of records being returned it will actually return a `lithium\data\source\database\adapter\my_sql\Result` object that can be operated on. A Result object in the case of the MySQL adapter is simply an iterable object wrapper for **[mysql\_fetch\_row()][3]**.

**A word of warning: **you should probably try to avoid using this method if you can. Try using Model methods or even `Model::connection()` to access native functions. The best option is really to create your own subclass of the MySQL adapter and drop it in to *app/extensions/data/source/database/adapter*.

Here is what the adapter might look like:

<pre class="brush:php"><?php</p>



<p>
  namespace app\extensions\data\source\adapter;
</p>



<p>
  class CustomMySql extends \lithium\data\source\adapter\MySql {
</p>



<p>
  public function query($sql) {
      return self::invokeMethod('_execute', array($sql));
    }
</p>



<p>
  }
</p>



<p>
  ?></pre>
  You would the define your custom adapter in your <em>app/config/connections.php</em>


  <pre class="brush:php">Connections::add('default', array(
  'type' =&gt; 'database',
  'adapter' =&gt; 'app\extensions\data\source\adapter\CustomMySql',
  'host' =&gt; 'localhost',
  'login' =&gt; 'root',
  'password' =&gt; '',
  'database' =&gt; 'my_blog'
));</pre>
  You should then be able to do the following from within other sections of code that interact with the database:


  <pre class="brush:php">
//disable all posts
$sql = "UPDATE posts SET active = NOT active";</p>



<p>
  //returns boolean true on success
  return Posts::connection()->query($sql);
  </pre>
</p>



<p>
  You could of course expand upon the query function to be more flexible, but I have written it to be quite simple for demonstration purposes. In reality you might want to allow for options or even make the query method filterable. Hopefully one day the Lithium team will get around to including a native <code>query()</code> method in the adapter, but until then you are left to roll your own. Luckily Lithium makes substituting classes a breeze.
</p>

 [1]: http://rad-dev.org/lithium/source/data/source/database/adapter/MySql.php
 [2]: http://en.wikipedia.org/wiki/Law_of_Demeter
 [3]: http://php.net/manual/en/function.mysql-fetch-row.php
