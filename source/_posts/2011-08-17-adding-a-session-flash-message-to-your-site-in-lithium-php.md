---
title: Adding a Session Flash Message to your site in Lithium PHP
author: jblotus
layout: post
dsq_thread_id:
  - 389022217
categories:
  - CakePHP
  - Lithium PHP
  - PHP Frameworks
  - Sessions
  - Web Development
  - Programming
tags:
  - cakephp
  - lithium
  - lithium php
  - php
  - php frameworks
  - session flash message
  - sessions
---
I've finally had a chance to start hacking on the[ Lithium PHP Framework][1] and I must say it's really cool. I have been working with [CakePHP ][2]for a few years and most of the concepts familiar to CakePHP developers will feel natural for those who choose to try Lithium. Unfortunately for those of us who expect the framework to come with everything you need out of the box, you will be somewhat disappointed or confused at first. In my case, I needed to display to the user a "flash message" after redirecting them to another page. CakePHP has this as part of the framework by default but Lithium will ask you to do a bit more work. Luckily,[ Lithium framework lead developer Nate Abele][3] has created a[ handy extension to the Lithium session helper ][4]will will give us exactly what we need.

**The Problem**

The idea behind a session flash message is that you set a message in the session, usually before a redirect that contains useful information about the operation performed on the previous page. The message will be output in the main layout of the website, usually near the top of the content. The key is deleted from session as soon as it is read to prevent the message from following the user around the site.

Here is how to get it working in Lithium:

**1. Enable sessions in ***app/config/bootstrap.php*

```
require <strong>DIR</strong> . '/bootstrap/session.php';
```

**2. Download the latest version of [Nate Abele's Session Helper Extension][5] and save it to ***app/extensions/helpers/Session.php***. I have pasted the code here for convenience.**

```
<?php
namespace app\extensions\helper;

class Session extends \lithium\template\Helper {
  /**
   * Classes used by this helper.
   *
   * @var array Key/value pair of classes.
   */
  protected $_classes = array('session' => '\lithium\storage\Session');

  /**
   * Session flash message.
   *
   * @param string $key Key of flash message. Defaults to 'message' if not set.
   * @return string Flash message, or null if not set.
   */
  public function message($key = null) {
    $class = $this->_classes['session'];
    $key = ($key === null) ? 'message' : $key;

    if ($message = $class::read($key)) {
      $class::delete($key);
      return $message;
    }
    return null;
  }

  /**
  * Read a value from the session.
  *
  * @param string $key The key to be fetched.
  * @return string The value stored in the session, or null if it does not exist.
  */
  public function read($key = null) {
    $class = $this->_classes['session'];
    var_dump($class);
    return $class::read($key);
  }
}
```
<strong>3. Add the session flash message check to your layout. <span style="font-family: mceinline;">(</span></strong><span style="font-family: mceinline;"><em>actual layout truncated</em></span><strong><span style="font-family: mceinline;">)</span></strong>

```
<?php
$session_flash_message = $this->session->message();

if ($session_flash_message) {
  ?>
  <p id="session-flash-message"><?= $session_flash_message ?></p>
  <?php
}
?>
<div id="content">
  <?php echo $this->content(); ?>
</div>
```

<strong>4. Add some css to your flash message</strong>

```
#session-flash-message {
  border: 1px solid #ff0000;
  padding: 20px;
  font-size: 16px;
  background: #CCC;
}
```

<strong>5. Set the session flash message in your controller before a redirect action</strong>


```
<?php
namespace app\controllers;

use app\models\Widgets;

//important: needed to write to session
use lithium\storage\Session;

class WidgetsController extends \lithium\action\Controller {

  public function index() {
    $widgets = Widgets::all();
    return compact('widgets');
  }

  public function add() {
    $data = $this->request->data;

    if ($this->request->data) {
      $widget = Widgets::create($data);
      $saved = $widget->save();

      if ($saved) {
        Session::write('message', 'Saved the widget');
        return $this->redirect(array('controller' => 'widgets', 'action' => 'index'), $options);
      } else {
        Session::write('message', 'Failed to save the widget, do it right this time');
        return $this->redirect(array('controller' => 'widgets', 'action' => 'add'), $options);
      }
    }
  }
}

```
<strong>Conclusion</strong>

This should be all you need to get started with session-based flash messages for your Lithium PHP Framework powered website. You can of course customize the appearance and behavior of your messages to suit your specific app. Let me know in the comments if I missed anything! Remember to do are return when calling any redirects since it Lithium won't stop the method on it's own.


 [1]: http://lithify.me/ "Lithium PHP Framework Homepage"
 [2]: http://cakephp.org/ "CakePHP homepage"
 [3]: https://twitter.com/#!/nateabele "Nate Abele Lithium PHP Twitter Account"
 [4]: http://lab.lithify.me/lab/extensions/view/8b25810f8f6fe936b874e0c3820c2e79 "Lithium Session Helper Extension"
 [5]: http://lab.lithify.me/lab/extensions/view/8b25810f8f6fe936b874e0c3820c2e79 "Nate Abele Lithium PHP Session Helper Extension"
