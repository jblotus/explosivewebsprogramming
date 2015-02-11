---
title: Combine serialized form ajax post data with an arbitrary object using jQuery
author: jblotus
layout: post

dsq_thread_id:
  - 356654730
categories:
  - Javascript
  - jQuery
  - Web Development
  - Programming
tags:
  - $.extend
  - $.param
  - ajax
  - javascript
  - jquery
  - serialize
---
Sometimes when you do an ajax form post in jQuery, you need to merge another object into your post data. The idea behind this solution is to serialize the existing form data using jQuery's **$().serialize** method. Then we use the** $.param()** utility method to create a serialized version of any javascript object or array. Simply concatenate the two and you are off to the races.

<pre class="brush:js">var $form = $(form);</p>



<pre><code>    var data = {
      'foo' : 'bar'
    };

    data = $form.serialize() + '&amp;' + $.param(data);

    $.ajax({
      'type' : 'POST',
      'dataType' : 'json',
      'url' : $form.attr('action'),
      'data': data,

      'success': function(data) {
        //do something
      }
    });&lt;/pre&gt;
</code></pre>



<h3>
  Warning
</h3>



<p>
  A downside to this approach is the possibility of duplicated key names in your serialized string. Concatenating the two serialize strings won't catch that. In that case you may be better off turning the form elements into an object and merging them using <strong>$.extend()</strong>. <a href="http://stevenbenner.com/2010/03/javascript-regex-trick-parse-a-query-string-into-an-object/">Check out this post on converting a query string into a javascript object</a>.
</p>
