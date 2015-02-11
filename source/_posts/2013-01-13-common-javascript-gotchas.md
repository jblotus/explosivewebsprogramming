---
title: 'Common JavaScript "Gotchas"'
author: jblotus
layout: post
dsq_thread_id:
  - 1022678866
categories:
  - Javascript
  - jQuery
  - PHP
  - Tutorial
  - Web Development
  - Programming
tags:
  - async
  - closures
  - ecmascript
  - ecmascript 5
  - es3
  - es5
  - globals
  - hoisting
  - javascript
  - jquery
  - php
  - this
  - tips
  - var keyword
---
PHP was my first programming language, and my initial exposure to JavaScript was through libraries like [jQuery][1]. There were things about JavaScript that always seemed to trip me up in the beginning due to how they worked differently than PHP. Heck there are still some things today that are confusing. I want to share some of the things that I struggled when I started working with JavaScript. I am going to cover the global namespace, `this`, knowing the difference between **ECMAScript 3** and **ECMAScript 5**, asynchronous operations, prototypes, and simple JavaScript inheritance.

<!--more-->

### The Global Namespace

In PHP specifically, when you declare a variable function outside of a class ([or namespace block][2]) you are essentially adding a function to the global namespace. In JavaScript, there is no such thing as a namespace per se, rather everything is attached to the global object. In the case of the web browser, that is the `window` object. The other key difference is that in JavaScript, functions and variables are attributes of the global object, which we typically refer to as properties.

This can be troublesome because you won't get a warning in JavaScript if you overwrite a global function or property and it can actually be quite dangerous.

<pre class="brush:js">function globalFunction() {
  console.log('I am the original global function');
}</p>



<p>
  function overWriteTheGlobal() {
    globalFunction = function() {
      console.log('I am the new global function');
    }
  }
  globalFunction(); //outputs "I am the original global function"
  overWriteTheGlobal(); //this will overwrite the original global function
  globalFunction(); //outputs "I am the new global function"</pre>
  One technique that is useful in JavaScript to ensure that your variables and functions are self contained is to use a <strong>immediately-invoked function expression</strong>, commonly known as a <strong>self-executing anonymous function</strong>. I typically expose things to the outside world by passing in a carrier object to the function. This is a variation of the <strong>module pattern</strong>.


  <pre class="brush:js">var module = {};</p>



<p>
  (function(exports){
</p>



<p>
  exports.notGlobalFunction = function() {
      console.log('I am not global');
    };
</p>



<p>
  }(module));
</p>



<p>
  function notGlobalFunction() {
    console.log('I am global');
  }
</p>



<p>
  notGlobalFunction(); //outputs "I am global"
  module.notGlobalFunction(); //outputs "I am not global"</pre>
  Inside the <strong>self-executing anonymous function</strong>, all of the global scope is enclosed and we finish by attaching it the the <code>module</code> variable. Technically you could just append properties directly to the <code>module</code> variable, but the reason we are passing it in to the function is to make it explicitly clear what we are attaching our function to. It also allows us to alias the passed in object inside the function. The critical thing here is that we are declaring our dependencies upfront and not relying on global variables, other than the module variable.
</p>



<p>
  You also might have noticed the <code>var</code> keyword. If you aren't sure of how it is used, a basic explanation is that by preceding a variable declaration with <code>var</code> creates a property on the nearest containing function. If you omit the <code>var</code> keyword than you are saying that you want to assign a new value to an existing variable higher up the scope chain, which may or may not be the global scope.


  <pre class="brush:js">var imAGlobal = true;</p>



<p>
  function globalGrabber() {
    imAGlobal = false;
    return imAGlobal;
  }
</p>



<p>
  console.log(imAGlobal); //outputs "true"
  console.log(globalGrabber()); //outputs "false"
  console.log(imAGlobal); //outputs "false"</pre>
  As you can see, it is quite dangerous to rely on globals in your functions, due to possible side effects and collisions that are bound to occur. Now what happens when we use the <code>var</code> keyword?


  <pre class="brush:js">var imAGlobal = true;</p>



<p>
  function globalGrabber() {
    var imAGlobal = false;
    return imAGlobal;
  }
</p>



<p>
  console.log(imAGlobal); //outputs "true"
  console.log(globalGrabber()); //outputs "false"
  console.log(imAGlobal); //outputs "true"</pre>
  JavaScript hoists the <code>var</code> declaration to the top of the function block, then initializes the variable. This is called <strong>variable hoisting</strong>.
</p>



<p>
  <strong>To summarize:</strong> all variables are scoped to a function (which is itself an object), and where you declare those variables with <code>var</code> determines the function they are scoped to. Excluding var will imply global scope for a variable.
</p>



<p>
  Let's look at how variable hoisting happens:


  <pre class="brush:js">function variableHoist() {
  console.log(hoisty);
  hoisty = 1;<br />
  console.log(hoisty);
  var hoisty = 2;
  console.log(hoisty);
}</p>



<p>
  variableHoist();
  //outputs undefined (would get a ReferenceError if no var declaration existed in scope)
  //outputs "1"
  //outputs "2"
</p>



<p>
  try {
    console.log(hoisty); //outputs ReferenceError (no global var "hoisty")
  } catch (e) {
    console.log(e);
  }</pre>
  So as you can see, it doesn't actually matter where you put the <code>var</code> declaration in the function, because the property gets created before the function executes any code. Now in practice, generally you want to put your <code>var</code> declarations at the top of the function, since that is where they end up anyway. It is also totally acceptable to initialize you variables at the top of the function, just be aware of the order of events here.
</p>



<p>
  Functions declared with the <code>function</code> keyword in JavaScript (not variable assignment) also get hoisted. Behind the scenes, the entire function gets hoisted up and is made available for execution.


  <pre class="brush:js">myFunction(); //outputs "i exist"</p>



<p>
  function myFunction() {
    console.log('i exist');
  }</pre>
  <strong>
  </strong>This wholesale function hoisting does not occur when you use the var form of function declaration:


  <pre class="brush:js">try {
  myFunction();
} catch (e) {
  console.log(e); //throws "Uncaught TypeError: undefined is not a function"
}
var myFunction = function() {
  console.log('i exist');
}</p>



<p>
  myFunction();  //outputs "i exist"</pre>
  &nbsp;
</p>



<h3>
  Understanding &#8220;this&#8221;
</h3>



<p>
  Since JavaScript uses function scope, the meaning of <code>this</code> is quite different than what you get in <strong>PHP</strong>, and causes a lot of confusion. Consider the following:


  <pre class="brush:js">console.log(this); // outputs window object</p>



<p>
  var myFunction = function() {
    console.log(this);
  }
</p>



<p>
  myFunction(); //outputs window object
</p>



<p>
  var newObject = {
    myFunction: myFunction
  }
</p>



<p>
  newObject.myFunction(); //outputs newObject</pre>
  <code>this</code> by default refers to the object a function is contained in. Since <code>myFunction()</code> is a property of the global object, <code>this</code> is a reference to the global object, which is <code>window</code>. Now when we mix <code>myFunction()</code> into a <code>newObject</code>, <code>this</code> now refers to <code>newObject</code>. In <strong>PHP</strong> and other similar languages, <code>this</code> always refers to the the instance of a class containing the method. You could argue that JavaScript is doing something stupid here, but truthfully much of the power of the JavaScript language comes from this feature. In fact, we can even replace the value of <code>this</code> when invoking our JavaScript functions by using the <code>call()</code> or <code>apply()</code> methods.


  <pre class="brush:js">var myFunction = function(arg1, arg2) {
  console.log(this, arg1, arg2);
};</p>



<p>
  var newObject = {};
</p>



<p>
  myFunction.call(newObject, 'foo', 'bar'); //outputs newObject "foo" "bar"
  myFunction.apply(newObject, ['foo', 'bar']); //outputs newObject "foo" "bar"</pre>
  But let's not get ahead of ourselves. All we are doing here is invoking the function <code>myFunction</code> by substituting an alternative value for <code>this</code> inside the function by placing the value of the object we want to use a substitute as the first argument. The fundamental difference between <code>call()</code> and <code>apply()</code> is the way you pass arguments to the function. <code>call()</code> will take an unlimited amount of arguments after the first argument and <code>apply()</code> expects and array of arguments as it's second argument.
</p>



<p>
  Libraries like jQuery perform a lot of magic by invoking things this way. <strong>Let's look at the $.each() method in jQuery:</strong>


  <pre class="brush:js">var $els = [$('div'), $('span')];
var handler = function() {
  console.log(this);
};</p>



<p>
  $.each($els, handler);
</p>



<p>
  //iteration 1 outputs wrapped jquery dom element for "div" tag
  //iteration 2 outputs wrapped jquery dom element for "span" tag
</p>



<p>
  handler.apply({}); //outputs object</pre>
  jQuery will often rewrite the value of <code>this</code>, so you should always try to be aware of what <code>this</code> means in the context of a jQuery event handler, or other such constructs.
</p>



<p>
  &nbsp;
</p>



<h3>
  Know the difference between ECMAScript 3 and ECMAScript 5
</h3>



<p>
  For many years, <strong>ECMAScript 3</strong> has been the standard in most browsers, but more recently <strong>ECMAScript 5</strong> has made it's way into most modern browsers (IE is still lagging behind). ECMAScript 5 introduced a lot of common sense features to JavaScript and some native methods that you previously relied upon a library for, such as <code>String.trim()</code> and <code>Array.forEach()</code>. The problem is you still can't rely on these methods being available in browser environments if you have users that are using Internet Explorer.
</p>



<p>
  <strong>Take a look at what happens when we try to use <code>String.trim</code> in IE 8:</strong>


  <pre class="brush:js">var fatString = "   my string   ";</p>



<p>
  //in modern browsers
  console.log(fatString); //outputs "   my string   "
  console.log(fatString.trim()); //outputs "my string"
</p>



<p>
  //in IE 8
  console.log(fatString.trim()); //error: Object doesn't support property or method 'trim'</pre>
  So in the interim, we can use methods like <code>jQuery.trim</code> to do this for us, which I believe will fallback to <code>String.trim</code> if it is available in your browser for increased performance (native browser implementations are faster).
</p>



<p>
  You might not care or even need to know about all of the differences between <strong>ECMAScript 3</strong> and <strong>ECMAScript 5</strong>, but it is generally a good idea to check out the <a href="https://developer.mozilla.org/en-US/docs/JavaScript">Mozilla Developer Network (MDN)</a> for function reference to see what versions of the language a function is available in first. Generally speaking, you should be fine if you are using a library like <strong>jQuery</strong> or <strong>underscore</strong> to handle this for you.
</p>



<p>
  If you are interested in using a polyfill of <strong>ECMAScript 5</strong> for older browser, please check out <a href="https://github.com/kriskowal/es5-shim">https://github.com/kriskowal/es5-shim</a>
</p>



<h3>
  Understanding Async
</h3>



<p>
  One of the things that tripped me up the most when beginning to work with JavaScript code, jQuery in particular is the fact that some operations are asynchronous. There were many times that I wrote code in a procedural manner expecting a result to be returned immediately without realizing it.
</p>



<p>
  <strong>Take a look at this broken code:</strong>


  <pre class="brush:js">var remoteValue = false;
$.ajax({
  url: 'http://google.com',
  success: function() {
    remoteValue = true;
  }
});</p>



<p>
  console.log(remoteValue); //outputs "false"</pre>
  It took me a while to realize that you need to program around asynchronous calls using callbacks to deal with the outcome of my ajax calls.


  <pre class="brush:js">var remoteValue = false;</p>



<p>
  var doSomethingWithRemoteValue = function() {
    console.log(remoteValue); //outputs true on success
  }
</p>



<p>
  $.ajax({
    url: 'https://google.com',
    complete: function() {
      remoteValue = true;
      doSomethingWithRemoteValue();<br />
    }
  });</pre>
  Another cool thing is deferred objects (sometimes called promises), which you can use to program in a more procedural style:


  <pre class="brush:js">var remoteValue = false;</p>



<p>
  var doSomethingWithRemoteValue = function() {
    console.log(remoteValue);
  }
</p>



<p>
  var promise = $.ajax({
    url: 'https://google.com'
  });
</p>



<p>
  //outputs "true"
  promise.always(function() {
    remoteValue = true;
    doSomethingWithRemoteValue();<br />
  });
</p>



<p>
  //outputs "foobar"
  promise.always(function() {
    remoteValue = 'foobar';
    doSomethingWithRemoteValue();<br />
  });</pre>
  You can use promises to chain callbacks in a style that is in my opinion a bit easier to work with than nested callbacks in addition to a host of other benefits these objects offer.
</p>



<p>
  <strong>Animations in the browser are also asyncronous</strong>, so this is also a common source of confusion. I'm not going to go into detail here, but you need to treat animations much like ajax requests in the way you handle them via callbacks. I'm not really an expert on the subject though so please take a look at the <a href="http://api.jquery.com/animate/">jQuery .animate() method</a>.
</p>



<h3>
  Simple Inheritance in JavaScript
</h3>



<p>
  Grossly simplified, JavaScript clones objects to extend them, while PHP, Ruby, Python and Java use and extend classes. In JavaScript you have something called a <code>prototype</code>, and every object has one. In fact, all functions, strings, numbers and objects have a common ancestor, <code>Object</code>. There are two things about prototype to remember:  blueprints and chains.
</p>



<p>
  Each <code>prototype</code> is basically an object in itself that describes properties available when creating an instance of an object. The prototype chain is what allows prototypes to extend other prototypes. In fact, prototypes themselves can have prototypes. When a method or attribute does not exist on an object instance, then it is looked for in that object's <code>prototype</code>, and the prototypes's <code>prototype</code>, and so on until it finally reaches <code>undefined</code> if no such property exists.
</p>



<p>
  Thankfully, beginners generally don't need to mess with this stuff at all, since it is easy enough to create an object literal and append properties to it at runtime.


  <pre class="brush:js">var obj = {};</p>



<p>
  obj.newFunction = function() {
    console.log('I am a dynamic function');
  };
</p>



<p>
  obj.newFunction();</pre>
  An easy way to extend objects that I use all the time is <code>jQuery.extend()</code>


  <pre class="brush:js">var obj = {
  a: 'i am a lonely property'
};</p>



<p>
  var newObj = {
    b: function() {
      return 'i am a lonely function';
    }
  };
</p>



<p>
  var finalObj = $.extend({}, obj, newObj);
</p>



<p>
  console.log(finalObj.a); //outputs "i am a lonely property"
  console.log(finalObj.b()); //outputs "i am a lonely function"</pre>
  <strong>ECMAScript 5</strong> offers us <code>Object.create()</code>, which you can use to extend from an existing object but you probably need to avoid using this if you need to support older browsers. It does offer distinct advantages to property creation and setting attributes of properties (yes, <a href="https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/Object/defineProperty">properties also have properties</a>).


  <pre class="brush:js">var obj = {
  a: 'i am a lonely property'
};</p>



<p>
  var finalObj = Object.create(obj, {
    b: {
      get: function() {
        return "i am a lonely function";
      }
    }
  });
</p>



<p>
  console.log(finalObj.a); //outputs "i am a lonely property"
  console.log(finalObj.b); //outputs "i am a lonely function"</pre>
  You can get pretty deep into the subject of <a href="http://www.crockford.com/javascript/inheritance.html">inheritance in JavaScript</a> but the beautiful thing here again is that you really don't have to due to the immense power and flexibility of the language.
</p>



<h3>
  Bonus Gotcha: Forgetting to use var in for loops
</h3>



<p>
  <pre class="brush:js">var i = 0;</p>



<p>
  function iteratorHandler() {<br />
    i = 10;
  }
</p>



<p>
  function iterate() {
   //this iteration will only run once
    for (i = 0; i < 10; i++) {
      console.log(i); //outputs 0
      iteratorHandler();
      console.log(i); //outputs 10
    }
  }
</p>



<p>
  iterate();</pre>
  The example is contrived, but you can see the danger here. The solution is to declare you iterator variables with <code>var</code>.


  <pre class="brush:js">var i = 0;</p>



<p>
  function iteratorHandler() {<br />
    i = 10;
  }
</p>



<p>
  function iterate() {
    //this iteration will run 10 times
    for (var i = 0; i < 10; i++) {<br />
      iteratorHandler();
      console.log(i);
    }
  }
</p>



<p>
  iterate();</pre>
  This all goes back to our scope rules. Remember to use <code>var</code> properly.
</p>



<h2>
  Summary
</h2>



<p>
  JavaScript may be the only language people don't need to learn before using it, but eventually you are going to run in to some unexplained trouble. Other than avoiding your own bugs, learning JavaScript makes a lot of sense these days considering it's rebirth and widespread availability. This blog by no means attempts to be a complete panacea, but hopefully it will help a few people understand some of the fundamentals before being forced into writing more awful JavaScript code, secretly hoping to get reassigned to a backend project buried in database queries in happy PHP land.
</p>

 [1]: http://jquery.com/
 [2]: http://php.net/manual/en/language.namespaces.definitionmultiple.php
