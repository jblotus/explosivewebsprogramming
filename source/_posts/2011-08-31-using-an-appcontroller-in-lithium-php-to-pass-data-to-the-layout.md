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

**Creating the AppController class is trivial.**

`in app/controllers/AppController.php`

```
<?php
namespace app\controllers;

use lithium\security\Auth;

class AppController extends \lithium\action\Controller {

  public function render($options = array()) {
    $auth = Auth::check('default');
    $this->set(compact('auth'));
    parent::render($options);
  }
}
```

Inside our AppController, we are extends the core Lithium controller class. Since we want to pass some auth data to the layout to be available on every page (via the layout) we need to override the render method of the controller. <code>lithium\action\Controller::render()</code> takes one argument, which is an array of options. We can leave this alone for now but we want to make sure to follow the same method signature as what the method we are extending.

In order to make the data appear in every view (or layout), we need to use the <code>Controller::set()</code> method.<code> Controller::set()</code> simply takes a <code>'key' => $value </code>array of variables to set. This should already be a familiar way to send data to the view, only now we are setting the data for every controller action called from a controller that extends <code>AppController</code>.

<strong>Here is an example of a controller extending AppController:</strong>

<code>in app/controllers/UsersController.php</code>
```
<?php
namespace app\controllers;

use app\models\Users;

class UsersController extends \app\controllers\AppController {

  public function index() {
    $users = Users::all();
    return compact('users');
  }
}
```
In our <code>Users::index()</code> view, we should expect <code>$users</code> to be available to us, and since we are extending <code>AppController</code>, the <code>$auth</code> variable is populated with user Authentication data (or false if check failed). In fact <code>$auth</code> will be available in the view for any controller action extending <code>AppController</code>.

<strong>Here is a snippet from my layout:</strong>

<code>in app/views/layouts.default.html.php</code>
```
<!doctype html>
<head>
  <?php
  /* Head stuff <em>/
  ?>
</head>
<body>
  <ul>
    <li class="active"><?php echo $this->html->link('Home', '/'); ?></li>
      <li>
      <?php
      if ($auth) {
        echo $this->html->link('Logout', 'Users::logout');
      } else {
        echo $this->html->link('Login', 'Users::login');
      }
      ?>
    </li>
  </ul>
</body>
</html>
```

 [1]: http://union-of-rad.org/ "#li3 Lithium PHP Twitter feed"
 [2]: http://book.cakephp.org/view/957/The-App-Controller "AppController in CakePHP"
 [3]: http://www.jblotus.com/2011/08/27/understanding-filters-in-lithium-php/ "Understand Filters in Lithium PHP"
