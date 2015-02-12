---
title: Your form fields are all broken on iOS / Mobile Safari
author: jblotus
layout: post

dsq_thread_id:
  - 313932300
categories:
  - HTML5
  - iOS
  - iPhone
  - iPad Development
  - Web Development
  - Programming
tags:
  - ios web development
  - mobile safari
  - mobile web
---
If your site has a contact form, login form, or just about any other type of form element, there is a huge chance that you are annoying your iPhone, iPad and iPod touch audience. By default, these devices all offer a few normally helpful features like "autocapitalize" and "autocorrect". The problem lies is the fact that some input fields expect case-sensitive data like username. On [Spelling City][1] we test input fields for correct word spelling, which critically relies upon proper case.  Asking your users to turn off autocorrect and autocapitalize is not really an adequate solution. Luckily the fix is trivially easy:

As stated in the [Safari Web Content Guide for Designing Forms][2]:

To turn off auto correct and autocapitalize:

<pre class="brush:plain"><input type="text" name="field1" autocorrect="off" autocapitalize="off" />;
</pre>

Please put this on your case-sensitive form fields asap. It doesn't break anything (except maybe W3C validation, which doesn't matter atm with HTML5).

NRY9V5EK22P9

 [1]: http://www.spellingcity.com
 [2]: http://developer.apple.com/library/safari/#documentation/appleapplications/reference/safariwebcontent/DesigningForms/DesigningForms.html
