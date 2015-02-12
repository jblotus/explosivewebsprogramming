---
title: Using an AppController in Lithium PHP to pass data to the layout
author: jblotus
layout: post

dsq_thread_id:
  - 400533613
categories:
  - Lithium PHP
  - PHP Frameworks
  - Web Development
  - Programming
tags:
  - AppController
  - auth
  - layout
  - lithium
  - lithium php
  - passing data to layout
  - php
  - view
---
**Note:** *This article may not be pushing best practices. Instead of setting view data via an AppController, we can create a helper for any arbitrary data we need. Due to Lithium's autoloading, this takes care of including our helper as needed and may make a bit more sense than injecting view variables via an AppController.*

A concept that has been somewhat glossed over in the [Lithium PHP community][1],  but is standard practice when coming from CakePHP is the [use of an AppController class][2]. The idea here is that all your app's controllers will extend from this custom controller instead of `\lithium\action\Controller`. This allows us to share some common controller functionality across our entire application. Obviously with [Lithium's Filter system][3], application wide callbacks are not as essential as they once were, nor as cumbersome, but I still think extending an `AppController` (and `AppModel` btw) is a good practice. One specific example is the passing of variables to a common layout that should be present on every page of your website.

<!--more-->

**Creating the AppController class is trivial.**

`in app/controllers/AppController.php`

<pre class="brush:php"><?php</p>



<p>
  namespace app\controllers;
</p>



<p>
  use lithium\security\Auth;
</p>



<p>
  class AppController extends \lithium\action\Controller {
</p>



<p>
  public function render($options = array()) {
</p>



<pre><code>$auth = Auth::check('default');
$this-&gt;set(compact('auth'));
parent::render($options);
</code></pre>



<p>
  }
  }
</p>



<p>
  ?></pre>
  Inside our AppController, we are extends the core Lithium controller class. Since we want to pass some auth data to the layout to be available on every page (via the layout) we need to override the render method of the controller. <code>lithium\action\Controller::render()</code> takes one argument, which is an array of options. We can leave this alone for now but we want to make sure to follow the same method signature as what the method we are extending.
</p>



<p>
  In order to make the data appear in every view (or layout), we need to use the <code>Controller::set()</code> method.<code> Controller::set()</code> simply takes a <code>'key' => $value </code>array of variables to set. This should already be a familiar way to send data to the view, only now we are setting the data for every controller action called from a controller that extends <code>AppController</code>.
</p>



<p>
  <strong>Here is an example of a controller extending AppController:</strong>
</p>



<p>
  <code>in app/controllers/UsersController.php</code>


  <pre class="brush:php"><?php</p>



<p>
  namespace app\controllers;
</p>



<p>
  use app\models\Users;
</p>



<p>
  class UsersController extends \app\controllers\AppController {
</p>



<p>
  public function index() {
</p>



<pre><code>$users = Users::all();
return compact('users');
</code></pre>



<p>
  }
  }
</p>



<p>
  ?></pre>
  In our <code>Users::index()</code> view, we should expect <code>$users</code> to be available to us, and since we are extending <code>AppController</code>, the <code>$auth</code> variable is populated with user Authentication data (or false if check failed). In fact <code>$auth</code> will be available in the view for any controller action extending <code>AppController</code>.
</p>



<p>
  <strong>Here is a snippet from my layout:</strong>
</p>



<p>
  <code>in app/views/layouts.default.html.php</code>


  <pre class="brush:php">&lt;!doctype html&gt;
&lt;head&gt;
&lt;?php
/* Head stuff <em>/
?&gt;
&lt;/head&gt;
&lt;body&gt;
      &lt;ul&gt;
        &lt;li class="active"&gt;&lt;?php echo $this-&gt;html-&gt;link('Home', '/'); ?&gt;&lt;/li&gt;
        &lt;li&gt;
          &lt;?php
          if ($auth) {
            echo $this-&gt;html-&gt;link('Logout', 'Users::logout');
          } else {
            echo $this-&gt;html-&gt;link('Login', 'Users::login');
          }
          ?&gt;
        &lt;/li&gt;
      &lt;/ul&gt;
&lt;?php
/</em> Some more layout goodness */
?&gt;
&lt;/body&gt;
&lt;/html&gt;</pre>
</p>

 [1]: http://union-of-rad.org/ "#li3 Lithium PHP Twitter feed"
 [2]: http://book.cakephp.org/view/957/The-App-Controller "AppController in CakePHP"
 [3]: http://www.jblotus.com/2011/08/27/understanding-filters-in-lithium-php/ "Understand Filters in Lithium PHP"
