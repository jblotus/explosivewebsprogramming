---
title: Understanding Filters in Lithium PHP
author: jblotus
layout: post
dsq_thread_id:
  - 397695866
categories:
  - Lithium PHP
  - PHP
  - PHP Frameworks
  - Web Development
  - Programming
tags:
  - closures
  - lithium
  - lithium filters
  - lithium php
  - lithium php filter system
  - php
---
One of the core features of the Lithium PHP 5.3 Framework is the[ innovative filter system][1]. I initially found the concept a bit hard to understand at first, but the idea is that you can modify a filterable method by applying a filter in some other part of the code. Many of the core methods that ship with Lithium's classes are already filterable, which simply means they return a closure containing their logic which checks for any applied filters at runtime. This powerful concept allows for near-total modification of a method, nearly anywhere in the application that calls it. In fact, you can make your own methods "filterable" to take advantage of this behavior.

###Anatomy of a Filterable method

Currently I think the documentation and examples for the Lithium PHP Filter system don't do a great job of explaining the basics of how Filters actually work. This was a stumbling block for me until I studied the well-commented core code.  Consider the following example code:

`in app/models/Users.php`

```
<?php
namespace app\models;

class Users extends \lithium\data\Model {

  public static function filterableFunction() {
    return static::_filter(__FUNCTION__, array(), function() {
      return 'foo';
    });
  }
}
```

As you can see, we are extending <code>lithium\data\Model</code> which extends from <code>lithium\core\StaticObject</code>. That is important because in order for this whole filter thing to work, your filterable methods and static functions will have to be in classes that extend either <code>lithium\core\Object</code> or <code>lithium\core\StaticObject</code>. These are the base objects for everything in the framework and any object we make in our app will likely extend for them in some way (unless it is an external library).

Taking a look at our model, you will see that there is a static function called <code>filterableFunction</code>. This is the method we want to apply filters to. They way this works is the method <em>returns a closure</em> produced by the <code>lithium\core\StaticObject::_filter()</code>. So what that means is the <code> lithium\core\StaticObject::_filter</code> method will execute any registered filters that were applied to the method before executing it. The _filter method takes a method name as it's first argument, and any passed arguments for the method as an array. The third argument is actually a closure which contains your method's logic.  Is your mind blown yet?

If we were to call this method right now, you would simply get the output <code>"foo"</code>.  This is because we haven't actually applied any filters to this method yet. Let's go ahead and do that now:

<code>in app/controllers/UsersController.php</code>
```
<?php
namespace app\controllers;

use app\models\Users;

class UsersController extends \lithium\action\Controller {

  public function filter_test() {
    Users::applyFilter('filterableFunction', function($self, $params, $chain) {
      $next = $chain->next($self, $params, $chain);
      var_dump(strtoupper($next));
      var_dump('After Execution');

      //we could also just return something else
      //but in this case we are returning the original output
      return $next;
    });

    $filtered = Users::filterableFunction();

    var_dump($filtered);
    exit;
  }  
}
```
<strong>And the output:</strong>

```
Before Execution
FOO
After Execution
foo
```

In the <code>filter_test()</code> method, we can apply the filter to our Users class. To do this we simply call the static method <code>applyFilter()</code> which is another bit that Users inherited from <code>lithium\core\StaticObject</code>. The first argument when dealing with a filter on a static method is just the string name of the method. The second argument of <code>applyFilter()</code> is a closure which is where we can define our intercepting logic.

You will notice that the closure takes three arguments, <code>$self</code>, <code>$params</code>, and <code>$chain</code>. This won't normally be changing so it would be a good idea to memorize the format. What it does is simply give us access to aspects of the original filterable method inside the context of this closure. In the example these arguments aren't utilized, but instead are just passed along to the next method in the filter chain.  By calling <code>$chain->next($self, $params, $chain)</code> I am collecting the output of the filterable method. This allows us to act on and manipulate that data before actually returning it.

It is usually a good idea to return something from the <code>applyFilter()</code> closure. In many cases you will simply return <code>$next</code> (the result of <code>$chain->next()</code>) unmolested. The idea is that you can create your own callbacks in different parts of your code to act upon these filterable. The value of <code>$next</code> is going to be the return value of the next registered filter for the filterable method, or the original method itself if the end of the chain has been reached.

<strong>Arguments and Filters</strong>

We also get access to our filterable methods agruments in the context of <code>applyFilter()</code>. In order to do this, let's modify our original example a bit.

<code>in app/models/Users.php</code>
```
<?php
namespace app\models;

class Users extends \lithium\data\Model {
  public static function filterableFunction($framework = 'Lithium') {
    //get late static binding class name
    $self = get_called_class();

    //collect params to pass to _filter()
    $params = compact('framework');

    return static::_filter(__FUNCTION__, $params, function($self, $params) {
      return $self::magicTimeBomb($params['framework']);
    });
  }

  public static function magicTimeBomb($framework) {
      return $framework . ' BOOOOOOOOOM!!!';
    }
  }
}
```

Now our filterable method get's interesting. I want to call another static method in the class and pass it an argument. If we couldn't do this, the whole filter system would be kind of pointless since our class we not be very <a href="http://en.wikipedia.org/wiki/Don't_repeat_yourself">DRY (don't repeat yourself)</a> if we had to stuff all our logic inside a closure. What we need to do is capture the true name of the called class using <a href="http://php.net/manual/en/function.get-called-class.php">get_called_class()</a>. If you didn't know already <code>get_called_class()</code> is similar to the static scope operator in the sense that it uses late static binding to determine the correct inheritance for static methods. We also collect any arguments into a <code>'key' => $value</code> array that need to be passed in to the closure.  The closure itself needs to be modified to accept <code>$self</code> and <code>$params</code> as arguments.

Notice that inside the closure the value of <code>$self::magicTimeBomb()</code> is passed a value from the <code>$params</code> array.

Now when we run our action again (<code>UsersController::filter_test()</code>) we get the following output:

```
Before Execution
LITHIUM BOOOOOOOOOM!!!
After Execution'
Lithium BOOOOOOOOOM!!!
```

Cool. Now let's modify it a little bit more in our <code>applyFilter()</code> closure in <code>UsersController::test_filter()</code>.

<code>in app/controllers/UsersController.php</code>

```
<?php
namespace app\controllers;

use app\models\Users;

class UsersController extends \lithium\action\Controller {

  public function filter_test() {
    Users::applyFilter('filterableFunction', function($self, $params, $chain) {
      var_dump('Before Execution, modifying params');
      $params['framework'] = 'Symfony2';

      $next = $chain->next($self, $params, $chain);

      var_dump('After Execution');
      return $next;
    });
  $filtered = Users::filterableFunction();

  var_dump($filtered);
  exit;
  }
}
```
<strong>Now the output:</strong>

```
Before Execution, modifying params
After Execution
Symfony2 BOOOOOOOOOM!!!
```
Wow. This is some cool stuff. Think about what we just did. We modified the arguments that get passed into our filterable method's closure. Think of the power and flexibility that gives you. This replaces the need to have the model level and controller level
<a title="Callbacks in CakePHP" href="http://book.cakephp.org/view/984/Callbacks">callbacks that we have in CakePHP</a> (which you can still create by the way, simply apply a filter in the classes  <em>_init()</em> method). Now we only define callbacks where we need them and we don't always have to open up our classes to modify them.

The Filter system for Lithium PHP is one of the key selling points for the framework in my opinion. It empowers you to change just about anything you need without opening up the core and hacking it up. The initial concept is a bit complicated, but once you understand how the core classes are constructed you will be amazed at how easy it is to make your app suck less!

Please let me know how you use Lithium's filter system in the comments.

 [1]: http://lithify.me/docs/manual/lithium-basics/filters.wiki "Filter System Manual for Lithium PHP"
