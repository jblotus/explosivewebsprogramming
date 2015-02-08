---
title: Use @dataProvider to reduce duplication and improve the maintainability of your tests
tags:
    - @dataProvider
    - data providers
    - php
    - phpunit
    - test duplication
    - multiple datasets in PHPUnit
categories:
    - progamming

---
[PHPUnit](http://phpunit.de/) offers a [handy annotation called @dataProvider](http://phpunit.de/manual/current/en/phpunit-book.html#appendixes.annotations.dataProvider) which can be 
used for all sorts of testing situations. Typically you will use this to feed in a wide variety of data into the same test to ensure that the **system-under-test** can handle a variety of inputs. 
Over time I have discovered a few other neat uses for `dataProvider` methods that I want to share with you today.

<!--more-->

Here is an example of a simple `dataProvider` that feeds data into our test method. The first thing to notice is the annotation in the DocBlock for our test method. Simply put the name of the dataProvider method here. The dataProvider itself must be a `public static` method that must return an `array`. Each element in the outer dataProvider array must also be an array. Each element of the inner array will be passed as an argument to the test method. That sounds rather complicated, so this example might help:

~~~
class WordFilterTest extends PHPUnit_Framework_TestCase
{
  /**
   * @param $validWord
   *
   * @dataProvider validWordsProvider
   */
  public function testAllowsValidWords($validWord)
  {
    $wordFilter = new WordFilter();
    $actual = $wordFilter->isWordAllowed($validWord);
    $this->assertTrue($actual);
  }

  public static function validWordsProvider()
  {
    return array(
      array('foo'),
      array('bar'),
      array('Baz')
    );
  }
}
~~~

There are also situations where you have tests that do essentially the same thing in terms of **setUp**, but have different outputs depending upon their inputs. Instead of duplicating our test conditions to test each input and output separately we can leverage the `@dataProvider` annotation to feed our test method a combination of values.

The example here takes the name of two primary colors are returns the name of the third color they produce when mixed.

~~~
class ColorMixerTest extends PHPUnit_Framework_TestCase
{
  /**
   * @param $colorOne
   * @param $colorTwo
   * @param $expectedColor
   *
   * @dataProvider colorMixProvider
   */
  public function testMixTwoColorsReturnsExpectedColor($colorOne, $colorTwo, $expectedColor)
  {
    $colorMixer = new ColorMixer();
    $actual = $colorMixer->mixColors($colorOne, $colorTwo);
    $this->assertEquals($expectedColor, $actual);
  }

  public static function colorMixProvider()
  {
    return array(
      array('red', 'yellow', 'orange'),
      array('red', 'blue', 'purple'),
      array('blue', 'yellow', 'green')
    );
  }
}
~~~
Another case for the `@dataProvider` annotation could be testing a variety of properties on an object were set or modified. This example will convert all _camelCase_ properties of an object to _under_score_.

~~~
class CamelCaseToUnderscoreConverterTest extends PHPUnit_Framework_TestCase
{
  /**
   * @param $inputProperty
   * @param $translatedPropertyName
   *
   * @dataProvider propertyProvider
   */
  public function testConvertsPropertyKeysToUnderscores($inputProperty, $translatedPropertyName)
  {
    $testObj= (object) array($inputProperty => 'anyValue');

    $camelCaseToUnderscoreConverter = new CamelCaseToUnderscoreConverter();
    $camelCaseToUnderscoreConverter->convertKeysToUnderscore($testObj); //by ref

    $this->assertObjectNotHasAttribute($inputProperty, $testObj);
    $this->assertObjectHasAttribute($translatedPropertyName, $testObj);
  }

  public static function propertyProvider()
  {
    return array(
      array('sourceId', 'source_id'),
      array('someImportantLongThing', 'some_important_long_thing'),
      array('somethingthatshouldnotchange', 'somethingthatshouldnotchange')
    );
  }
}
~~~

One thing to keep in mind is that dataProvider methods run before your **TestCase** can run it's **setUp**, so you won't have access to anything that gets configured there. 
In this example, we need to create a multiple database rows for different use cases to run our expensive business process. 
The dataProvider has no access to our database connection so we instead pass an array of "instructions" to our test method to be created when the test gets executed.

~~~
class ExpensiveBusinessProcessTest extends PHPUnit_Framework_TestCase
{
  /**
   * @param array $rowsToCreate
   *
   * @dataProvider rowsToCreateProvider
   */
  public function testExpensiveProcessDoesIt(array $rowsToCreate)
  {
    foreach ($rowsToCreate as $rowToCreate) {
      RowCreator::createRow($rowsToCreate['table'], $rowsToCreate['data']);
    }

    $expensiveBusinessProcess = new ExpensiveBusinessProcess();
    $actual = $expensiveBusinessProcess->execute();

    $this->assertEquals('it worked with this data set, feed me another', $actual);
  }

  public static function rowsToCreateProvider()
  {
    return array(

      //bob from accounting should have no issues completing this process since he is an admin
      array(
        array('table' => 'users', 'data' => array('id' => 555, 'username' => 'bobfromaccounting', 'type' => 'admin'))
      ),

      //sue from the sales team should also be able to do it since she has an acl entry
      array(
        array('table' => 'users', 'data' => array('id' => 899, 'username' => 'suefromsales', 'type' => 'user')),
        array('table' => 'users_permissions', 'data' => array('user_id' => 899, 'enabled' => 1))
      )
    );
  }
}
~~~
So the next time you find yourself doing the copy/paste dance ask yourself if a dataProvider would be more appropriate. As with anything, it is possible to go overboard with them and sacrifice clarity for the sake of reduced duplication. I look forward to hearing about how YOU use them in your tests, so please leave a comment!
