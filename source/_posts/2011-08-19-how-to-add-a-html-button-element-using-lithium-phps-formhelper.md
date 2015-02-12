---
title: 'How to add a HTML button element using Lithium PHPs FormHelper'
author: jblotus
layout: post

dsq_thread_id:
  - 390111627
categories:
  - Lithium PHP
  - PHP Frameworks
  - Web Development
  - Programming
tags:
  - element
  - formhelper
  - lithium
  - lithium php
  - php
---
Lithium Day 2. This time just a simple quick tip about adding different input types using the[ FormHelper in Lithium][1]. I want to create a button element in my form but there is no native method to do it. The trick is to use** FormHelper::field(&#8216;Button Text', array(&#8216;type' => &#8216;button', &#8216;label' => false));** This will output a button element. Setting the &#8216;label' to false is also important since by default all** FormHelper::field()** elements will generate a label. A quick check of the [docs for FormHelper::field()][2] gives a full breakdown of options.

**Here is some sample code:**

```
<?=$this->form->create($object); ?>
<?=$this->form->field('title');?>
<?=$this->form->field('url', array('type' => 'text'));?>
<?=$this->form->field('Cancel', array('type' => 'button', 'label' => false)); ?>
<?=$this->form->submit('Submit'); ?>
<?=$this->form->end(); ?>
```

The markup we get from this will tell us a few things:

```
<form action="/my_controller/some_action" method="post">
  <div>
    <label for="Title">Title</label>
    <input type="text" name="title" id="Title">
  </div>
  <div>
      <label for="Url">Url</label>
      <input type="text" name="url" id="Url">
  </div>

  <div>
      <button id="Cancel">Cancel</button>
  </div>

  <input type="submit" value="Submit">
</form>
```

You can see that the `FormHelper::field()` method also generates elements with a surrounding div. You can stop prevent this by passing `('div' => false)` in the options array.

 [1]: http://lithify.me/docs/lithium/template/helper/Form "Lithium FormHelper"
 [2]: http://lithify.me/docs/lithium/template/helper/Form::field() "Lithium FormHelper::field() docs"
