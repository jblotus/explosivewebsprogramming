---
title: Keeping your Handlebars.js templates organized
author: jblotus
layout: post
dsq_thread_id:
  - 312021309
categories:
  - Handlebars.js
  - Javascript
  - Web Development
  - Programming
tags:
  - async
  - handlebars
  - handlebarsjs
  - javascript
  - jquery
  - self executing anonymous functions
---
[Handlebars.js][1] is a great tool for client side templates, but one thing that is a bit of a nag about Handlebars.js templates is the fact that the docs recommend you insert the templates as inline script tags. This can be a bit of an issue if you have a lot of views, since you probably don't want to put them all on your page, or perhaps you have something more dynamic in mind.. Consider the implementation defined in the Handlebars.js docs:

<pre class="brush:js">&lt;script id="entry-template" type="text/x-handlebars-template"&gt;
{{content}}
&lt;/script&gt;</pre> This can get out of hand quickly if you have multiple templates and it gets even worse if you want to reuse your templates across multiple files. The solution I came up with involves loading your templates via ajax. This allows you to organize your templates however you see fit but I personally prefer to lay them out like this:

<pre class="brush:shell">/public_html/
/public_html/js/
/public_html/js/templates/
/public_html/js/templates/apple.handlebars
/public_html/js/templates/orange.handlebars
/public_html/js/templates/banana.handlebars</pre> You can use the following code to load a handlebars template via ajax. (example assumes you use jQuery):

<pre class="brush:js">(function getTemplateAjax(path) {
    var source;
    var template;</p>



<pre><code>$.ajax({
    url: path, //ex. js/templates/mytemplate.handlebars
        cache: true,
        success: function(data) {
            source    = data;
            template  = Handlebars.compile(source);
            $('#target').html(template);
    }
});
</code></pre>



<p>
  })()</pre>
  Since we are loading via ajax you might want to rewrite your template loading function to utilize a callback like this:


  <pre class="brush:js">(
    function getTemplateAjax(path, callback) {
        var source;
        var template;</p>



<pre><code>    $.ajax({
        url: path,
            success: function(data) {
                source    = data;
                template  = Handlebars.compile(source);

                //execute the callback if passed
                if (callback) callback(template);
        }
    });
}

//run our template loader with callback
(getTemplateAjax('js/templates/handlebarsdemo.handlebars', function(template) {&lt;/pre&gt;
</code></pre>



<p>
  <pre class="brush:js"><br />
        //do something with compiled template
        $('body').html(template);
    })()
)()</pre>
</p>

 [1]: http://www.handlebarsjs.com/
