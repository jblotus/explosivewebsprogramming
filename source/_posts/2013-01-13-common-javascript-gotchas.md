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

```
function globalFunction() {
  console.log('I am the original global function');
}

  function overWriteTheGlobal() {
    globalFunction = function() {
      console.log('I am the new global function');
    }
  }
  globalFunction(); //outputs "I am the original global function"
  overWriteTheGlobal(); //this will overwrite the original global function
  globalFunction(); //outputs "I am the new global function"
```

One technique that is useful in JavaScript to ensure that your variables and functions are self contained is to use a **immediately-invoked function expression**, commonly known as a **self-executing anonymous function**. I typically expose things to the outside world by passing in a carrier object to the function. This is a variation of the **module pattern**.

```
var module = {};

(function(exports){

  exports.notGlobalFunction = function() {
    console.log('I am not global');
  };
}(module));


function notGlobalFunction() {
  console.log('I am global');
}

notGlobalFunction(); //outputs "I am global"
module.notGlobalFunction(); //outputs "I am not global"
```
Inside the **self-executing anonymous function**, all of the global scope is enclosed and we finish by attaching it the the `module` variable. Technically you could just append properties directly to the `module` variable, but the reason we are passing it in to the function is to make it explicitly clear what we are attaching our function to. It also allows us to alias the passed in object inside the function. The critical thing here is that we are declaring our dependencies upfront and not relying on global variables, other than the module variable.

You also might have noticed the `var` keyword. If you aren't sure of how it is used, a basic explanation is that by preceding a variable declaration with `var` creates a property on the nearest containing function. If you omit the `var` keyword than you are saying that you want to assign a new value to an existing variable higher up the scope chain, which may or may not be the global scope.

```
var imAGlobal = true;

function globalGrabber() {
  imAGlobal = false;
  return imAGlobal;
}

console.log(imAGlobal); //outputs "true"
console.log(globalGrabber()); //outputs "false"
console.log(imAGlobal); //outputs "false"
```

As you can see, it is quite dangerous to rely on globals in your functions, due to possible side effects and collisions that are bound to occur. Now what happens when we use the `var` keyword?

```
var imAGlobal = true;

function globalGrabber() {
  var imAGlobal = false;
  return imAGlobal;
}


console.log(imAGlobal); //outputs "true"
console.log(globalGrabber()); //outputs "false"
console.log(imAGlobal); //outputs "true"
```

JavaScript hoists the `var` declaration to the top of the function block, then initializes the variable. This is called **variable hoisting**.

**To summarize:** all variables are scoped to a function (which is itself an object), and where you declare those variables with `var` determines the function they are scoped to. Excluding var will imply global scope for a variable.

Let's look at how variable hoisting happens:

```
function variableHoist() {
  console.log(hoisty);
  hoisty = 1;
  console.log(hoisty);
  var hoisty = 2;
  console.log(hoisty);
}

variableHoist();
//outputs undefined (would get a ReferenceError if no var declaration existed in scope)
//outputs "1"
//outputs "2"

try {
  console.log(hoisty); //outputs ReferenceError (no global var "hoisty")
} catch (e) {
  console.log(e);
}
```

So as you can see, it doesn't actually matter where you put the `var` declaration in the function, because the property gets created before the function executes any code. Now in practice, generally you want to put your `var` declarations at the top of the function, since that is where they end up anyway. It is also totally acceptable to initialize you variables at the top of the function, just be aware of the order of events here.

Functions declared with the `function` keyword in JavaScript (not variable assignment) also get hoisted. Behind the scenes, the entire function gets hoisted up and is made available for execution.

```
myFunction(); //outputs "i exist"

function myFunction() {
  console.log('i exist');
}
```

**This wholesale function hoisting does not occur when you use the var form of function declaration:**

```
try {
  myFunction();
} catch (e) {
  console.log(e); //throws "Uncaught TypeError: undefined is not a function"
}
var myFunction = function() {
  console.log('i exist');
}

myFunction();  //outputs "i exist"
```

###Understanding "this";

Since JavaScript uses function scope, the meaning of `this` is quite different than what you get in **PHP**, and causes a lot of confusion. Consider the following:

```
console.log(this); // outputs window object

var myFunction = function() {
  console.log(this);
}

myFunction(); //outputs window object

var newObject = {
  myFunction: myFunction
}

newObject.myFunction(); //outputs newObject
```

`this` by default refers to the object a function is contained in. Since `myFunction()` is a property of the global object, `this` is a reference to the global object, which is `window`. Now when we mix `myFunction()` into a `newObject`, `this` now refers to `newObject`. In **PHP** and other similar languages, `this` always refers to the the instance of a class containing the method. You could argue that JavaScript is doing something stupid here, but truthfully much of the power of the JavaScript language comes from this feature. In fact, we can even replace the value of `this` when invoking our JavaScript functions by using the `call()` or `apply()` methods.

```
var myFunction = function(arg1, arg2) {
  console.log(this, arg1, arg2);
};

var newObject = {};

myFunction.call(newObject, 'foo', 'bar'); //outputs newObject "foo" "bar"
myFunction.apply(newObject, ['foo', 'bar']); //outputs newObject "foo" "bar"
```

But let's not get ahead of ourselves. All we are doing here is invoking the function `myFunction` by substituting an alternative value for `this` inside the function by placing the value of the object we want to use a substitute as the first argument. The fundamental difference between `call()` and `apply()` is the way you pass arguments to the function. `call()` will take an unlimited amount of arguments after the first argument and `apply()` expects and array of arguments as it's second argument.

Libraries like jQuery perform a lot of magic by invoking things this way. **Let's look at the $.each() method in jQuery:**

```
var $els = [$('div'), $('span')];
var handler = function() {
  console.log(this);
};

$.each($els, handler);

//iteration 1 outputs wrapped jquery dom element for "div" tag
//iteration 2 outputs wrapped jquery dom element for "span" tag

handler.apply({}); //outputs object
```
jQuery will often rewrite the value of `this`, so you should always try to be aware of what `this` means in the context of a jQuery event handler, or other such constructs.

###Know the difference between ECMAScript 3 and ECMAScript 5

For many years, **ECMAScript 3** has been the standard in most browsers, but more recently **ECMAScript 5** has made it's way into most modern browsers (IE is still lagging behind). ECMAScript 5 introduced a lot of common sense features to JavaScript and some native methods that you previously relied upon a library for, such as `String.trim()` and `Array.forEach()`. The problem is you still can't rely on these methods being available in browser environments if you have users that are using Internet Explorer.

**Take a look at what happens when we try to use `String.trim` in IE 8:**

```
var fatString = "   my string   ";

//in modern browsers
console.log(fatString); //outputs "   my string   "
console.log(fatString.trim()); //outputs "my string"

//in IE 8
console.log(fatString.trim()); //error: Object doesn't support property or method 'trim'
```
So in the interim, we can use methods like `jQuery.trim` to do this for us, which I believe will fallback to `String.trim` if it is available in your browser for increased performance (native browser implementations are faster).

You might not care or even need to know about all of the differences between **ECMAScript 3** and **ECMAScript 5**, but it is generally a good idea to check out the <a href="https://developer.mozilla.org/en-US/docs/JavaScript">Mozilla Developer Network (MDN)</a> for function reference to see what versions of the language a function is available in first. Generally speaking, you should be fine if you are using a library like **jQuery** or **underscore** to handle this for you.

If you are interested in using a polyfill of **ECMAScript 5** for older browser, please check out <a href="https://github.com/kriskowal/es5-shim">https://github.com/kriskowal/es5-shim</a>

###Understanding Async

One of the things that tripped me up the most when beginning to work with JavaScript code, jQuery in particular is the fact that some operations are asynchronous. There were many times that I wrote code in a procedural manner expecting a result to be returned immediately without realizing it.

**Take a look at this broken code:**

```
var remoteValue = false;
$.ajax({
  url: 'http://google.com',
  success: function() {
    remoteValue = true;
  }
});


console.log(remoteValue); //outputs "false"
```

It took me a while to realize that you need to program around asynchronous calls using callbacks to deal with the outcome of my ajax calls.

```
var remoteValue = false;

var doSomethingWithRemoteValue = function() {
  console.log(remoteValue); //outputs true on success
}
$.ajax({
  url: 'https://google.com',
  complete: function() {
    remoteValue = true;
    doSomethingWithRemoteValue();
  }
});
```

Another cool thing is deferred objects (sometimes called promises), which you can use to program in a more procedural style:

```
var remoteValue = false;

var doSomethingWithRemoteValue = function() {
  console.log(remoteValue);
}

var promise = $.ajax({
  url: 'https://google.com'
});


//outputs "true"
promise.always(function() {
  remoteValue = true;
  doSomethingWithRemoteValue();
});

//outputs "foobar"
promise.always(function() {
  remoteValue = 'foobar';
  doSomethingWithRemoteValue();
});
```
You can use promises to chain callbacks in a style that is in my opinion a bit easier to work with than nested callbacks in addition to a host of other benefits these objects offer.

**Animations in the browser are also asyncronous**, so this is also a common source of confusion. I'm not going to go into detail here, but you need to treat animations much like ajax requests in the way you handle them via callbacks. I'm not really an expert on the subject though so please take a look at the <a href="http://api.jquery.com/animate/">jQuery .animate() method</a>.

###Simple Inheritance in JavaScript

Grossly simplified, JavaScript clones objects to extend them, while PHP, Ruby, Python and Java use and extend classes. In JavaScript you have something called a `prototype`, and every object has one. In fact, all functions, strings, numbers and objects have a common ancestor, `Object`. There are two things about prototype to remember:  blueprints and chains.

Each `prototype` is basically an object in itself that describes properties available when creating an instance of an object. The prototype chain is what allows prototypes to extend other prototypes. In fact, prototypes themselves can have prototypes. When a method or attribute does not exist on an object instance, then it is looked for in that object's `prototype`, and the prototypes's `prototype`, and so on until it finally reaches `undefined` if no such property exists.

Thankfully, beginners generally don't need to mess with this stuff at all, since it is easy enough to create an object literal and append properties to it at runtime.

```
var obj = {};

obj.newFunction = function() {
  console.log('I am a dynamic function');
};

obj.newFunction();
```

An easy way to extend objects that I use all the time is `jQuery.extend()`

```
var obj = {
  a: 'i am a lonely property'
};

var newObj = {
  b: function() {
    return 'i am a lonely function';
  }
};


var finalObj = $.extend({}, obj, newObj);

console.log(finalObj.a); //outputs "i am a lonely property"
console.log(finalObj.b()); //outputs "i am a lonely function"
```

**ECMAScript 5** offers us `Object.create()`, which you can use to extend from an existing object but you probably need to avoid using this if you need to support older browsers. It does offer distinct advantages to property creation and setting attributes of properties (yes, <a href="https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/Object/defineProperty">properties also have properties</a>).

```
var obj = {
  a: 'i am a lonely property'
};

var finalObj = Object.create(obj, {
  b: {
    get: function() {
      return "i am a lonely function";
    }
  }
});

console.log(finalObj.a); //outputs "i am a lonely property"
console.log(finalObj.b); //outputs "i am a lonely function"
```

You can get pretty deep into the subject of <a href="http://www.crockford.com/javascript/inheritance.html">inheritance in JavaScript</a> but the beautiful thing here again is that you really don't have to due to the immense power and flexibility of the language.

###Bonus Gotcha: Forgetting to use var in for loops

```
var i = 0;


function iteratorHandler() {
  i = 10;
}


function iterate() {
 //this iteration will only run once
  for (i = 0; i < 10; i++) {
    console.log(i); //outputs 0
    iteratorHandler();
    console.log(i); //outputs 10
  }
}



iterate();
```
The example is contrived, but you can see the danger here. The solution is to declare you iterator variables with `var`.

```
var i = 0;

function iteratorHandler() {
  i = 10;
}

function iterate() {
  //this iteration will run 10 times
  for (var i = 0; i < 10; i++) {
    iteratorHandler();
    console.log(i);
  }
}


iterate();
```
This all goes back to our scope rules. Remember to use `var` properly.

##Summary
JavaScript may be the only language people don't need to learn before using it, but eventually you are going to run in to some unexplained trouble. Other than avoiding your own bugs, learning JavaScript makes a lot of sense these days considering it's rebirth and widespread availability. This blog by no means attempts to be a complete panacea, but hopefully it will help a few people understand some of the fundamentals before being forced into writing more awful JavaScript code, secretly hoping to get reassigned to a backend project buried in database queries in happy PHP land.

[1]: http://jquery.com/
[2]: http://php.net/manual/en/language.namespaces.definitionmultiple.php
