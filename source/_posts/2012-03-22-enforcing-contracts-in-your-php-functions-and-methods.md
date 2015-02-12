---
title: Enforcing contracts in your PHP functions and methods
author: jblotus
layout: post
dsq_thread_id:
  - 620044769
categories:
  - ContractLib
  - Design by contract
  - PHP
  - PHPUnit
  - Web Development
  - Programming
tags:
  - assert()
  - contractlib
  - design by contract
  - phix
  - php
  - phpunit
  - scalar type hinting
  - stuart herbert
---
{% block excerpt %}
[Design by contract][1] is an important concept for controlling what type of input your methods or functions can receive. One of the most dangerous features of PHP is that functions will still execute even when they are missing required arguments, by emitting a warning instead of an error. In this post, I am going to walk through some of the solutions available to deal with this problem.
{% endblock %}

{% block content %}
First, we create our function. We are defining it so there is no default for the first argument.

```
<?php
function foo($arg) {
  echo 'it ran';
}

foo();
```

We then invoke our function, and you will get an <code>E_WARNING</code>. Notice that the function will still run and "<em>it ran</em>" will be output.


```
PHP Warning: Missing argument 1 for foo(), called in C:\Sites\temp\func.php on line 6 and defined in C:\Sites\temp\func.php on line 2

Warning: Missing argument 1 for foo(), called in C:\Sites\temp\func.php on line 6 and defined in C:\Sites\temp\func.php on line 2

it ran
```

I would consider this behavior undesirable, since further checks must be made to ensure that execution halts. <a title="php.net type hinting" href="http://php.net/manual/en/language.oop5.typehinting.php">Type-hinted arguments</a> on the other hand do not suffer from this problem. They throw something called a <a title="how to handle a catchable fatal error" href="http://stackoverflow.com/questions/2468487/how-can-i-catch-a-catchable-fatal-error-on-php-type-hinting">Catchable Fatal Error</a>.

```
<?php
function foo(array $arg, stdClass $arg1) {
  echo 'it should not run';
}

foo();
```

When you call a function without arguments and those arguments are type hinted, PHP generates a <strong>Catchable Fatal Error</strong>.

```
PHP Catchable fatal error: Argument 1 passed to foo() must be an array, none given, called in C:\Sites\temp\func.php on line 6 and defined in C:\Sites\temp\func.php on line 2

Catchable fatal error: Argument 1 passed to foo() must be an array, none given, called in C:\Sites\temp\func.php on line 6 and defined in C:\Sites\temp\func.php on line 2
```

You will notice that execution halts before the function even gets to the second argument. These <strong>Catchable Fatal Errors</strong> are not <a title="php.net exceptions" href="http://php.net/manual/en/language.exceptions.php">Exceptions </a>per se. In order to deal with them, you have to create a custom error handler using <a title="php.net set_error_handler()" href="http://php.net/manual/en/function.set-error-handler.php">set_error_handler()</a> and throw an Exception from there if the error is of the type <code>E_RECOVERABLE_ERROR</code>.

This is all pretty annoying, but serviceable – unless you consider that you cannot type hint scalar types in php (int, string, etc). BTW, Here is a good example of why<a title="php scalar type hinting discussion" href="http://nikic.github.com/2012/03/06/Scalar-type-hinting-is-harder-than-you-think"> implementing scalar type hints in php is hard</a> and some proposed solutions.

The stock solution I typically rely upon involves setting default values for all arguments, and throwing exceptions where the input data does not match.

```
<?php
function foo($string = '', $int = 0, array $array = array()) {

  if (!is_string($string) || !$string) {
      throw new InvalidArgumentException('$string was not a string or empty');
  }
  if (!is_int($int) || !$int) {
      throw new InvalidArgumentException('$int was not an int or empty');
  }
  //it's ok if array is empty
  }
```

This is probably the simplest form of design-by-contract for php that you can achieve. I have also recently begun exploring some other options for design by contract in PHP.

PHP's <a title="php.net assert()" href="http://php.net/manual/en/function.assert.php">assert()</a> is a native PHP function for asserting conditions, but it is probably much unsuitable for production due to the overhead involved. It also seems fairly cryptic and complicated to me, without adding much value or clarity. To be fair I really don't know much about assert() so I might be misinformed.

<a title="PHPUnit assertions for design-by-contract enforcement" href="http://www.phpunit.de/manual/3.4/en/api.html#api.assert.examples.Sample.php">PHPUnit also offers a wide variety of assertions suitable for design by contract programming</a>, which can be added to directly to your functions, independent of any test code.

```
<?php
require_once 'PHPUnit\autoload.php';

function foo($string = '', $int = 0, array $array = array()) {
  PHPUnit_Framework_Assert::assertInternalType(
    'string', $string, '$string should be a string'
  );
  echo 'This will not run';
}
foo(123);
```

When an assertion fails, a <code>PHPUnit_Framework_ExpectationFailedException</code> will be thrown.

```
PHP Fatal error: Uncaught exception 'PHPUnit_Framework_ExpectationFailedException' with message '$string should be a string
Failed asserting that 123 is of type "string".' in C:\wamp\bin\php\php5.3.8\pear\PHPUnit\Framework\Constraint.php:145
```

The benefits of using PHPUnit as an assertion library are obvious. Well tested and curated assertions exist for nearly any type of situation. These assertions should also be quite familiar if you are already using PHPUnit to test your code. A potential drawback however is adding a static dependency to PHPUnit to your production code. I haven't actually tried this in a production setting, but it seems quite servicable.

Finally we come upon <a title="ContractLib github" href="https://github.com/stuartherbert/ContractLib">ContractLib</a> by <a title="Stuart Herbert github" href="https://github.com/stuartherbert">Stuart Herbert</a> of <a title="Phix homepage" href="http://phix-project.org/">Phix</a> fame. ContractLib is a PHP library that enforces contracts using a closure based style.

```
<?php
//ContractLib requires use of a PSR-0 autoloader right now
use Phix_Project\ContractLib\Contract;</p>

function autoload($className) {
  $className = ltrim($className, '\');
  $fileName = '';
  $namespace = '';

  if ($lastNsPos = strripos($className, '\')) {
    $namespace = substr($className, 0, $lastNsPos);
    $className = substr($className, $lastNsPos + 1);
    $fileName = str_replace('\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
  }
  $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';

  require $fileName;
}
spl_autoload_register('autoLoad');

function foo($string) {
  //we must tell ContractLib to enforce contracts
  Contract::EnforceWrappedContracts();

  Contract::Preconditions(function() use ($string) {
      Contract::RequiresValue($string, is_string($string), '$string must be a string');
      Contract::RequiresValue($string, !empty($string), '$string must not be empty');
  });

  echo "this will not run\n";
}

foo();
foo('');
```

First of all, you will notice that you need a <a title="PSR-0 Standard gist" href="https://gist.github.com/1234504">PSR-0 compatible autoloader</a> in order to use ContractLib. Not really a big deal but if you already use one ignore that block of code. Secondly, you also have to explicitly tell ContractLib to enforce contracts. This would allow you to only enforce contracts in a development environment for example. Apparently <a title="PHP assert() vs ContractLib" href="http://blog.stuartherbert.com/php/2012/01/17/comparing-contractlib-to-phps-built-in-assert/">ContractLib is pretty fast vs PHP's assert()</a>, and I would personally argue that contracts should be enforced at all times. You can always turn it off and benchmark your code to see if there is a big difference.

When you call <code>Contract::Preconditions()</code> with a closure, you would pass in your method arguments via use(). For each contract you want to assert, you simple make a call to <code>Contract::RequiresValue()</code>. The syntax is pretty straightforward. The first arg is the value you are checking. The second argument is interesting because it takes an expression, so you can come up with your own assertions. The only problem I have with that is the potential for errors when writing your assertions, as opposed to using defined asserts à la PHPUnit. When a contract fails, ContractLib will throw a <code>Phix_Project\ContractLib\E5xx_ContractFailedException</code>.

```
Notice: Undefined variable: string in C:\dev3\clibtest\clib.php on line 28 PHP Fatal error:
Uncaught exception 'Phix_Project\ContractLib\E5xx_ContractFailedException' with message
'Internal server error: Contract::RequiresValue() failed; value tested was '';
reason for failure was: $string must be a string' in ...
```
I think <a title="ContractLib github" href="https://github.com/stuartherbert/ContractLib">ContractLib</a> looks pretty exciting, although I am not entirely sold about using it in production yet. I am probably leaning towards some sort of DI wrapping for PHPUnit's assertions to work into my projects, since static dependencies scare me and make testing difficult.

**Summary**
Hopefully this post has helped you understand a bit more about design-by-contract in PHP. There are definitely some options out there for achieving this in your projects today. Please free to share any tips or tricks in the comments area.

[1]: http://en.wikipedia.org/wiki/Design_by_contract
{% endblock %}
