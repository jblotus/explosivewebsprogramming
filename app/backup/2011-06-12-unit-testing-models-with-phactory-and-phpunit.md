---
title: Unit Testing Models with Phactory and PHPUnit
author: jblotus
layout: post
dsq_thread_id:
  - 329118867
categories:
  - Database Testing
  - MVC
  - MySQL
  - Phactory
  - PHP
  - PHPUnit
  - SQL
  - Testing
  - Unit Testing
  - Web Development
  - Programming
tags:
  - database testing
  - database unit testing
  - dbunit
  - phactory
  - php
  - phpunit
  - phpunit database testing
  - testing models
---
[<img class="size-full wp-image-530 alignleft" title="PHP Database Unit Testing with Phactory" src="http://www.jblotus.com/wp-content/uploads/2011/06/phactory-transparent.png" alt="PHP Database Unit Testing with Phactory" width="212" height="76" />][1]

[PHPUnit][2], although exceptional in so many aspects, is somewhat lacking when it comes to database testing. Testing models in with PHPUnit usually requires you to set up cumbersome XML files for fixtures and your are stuck with them for the entire testcase. I am going to show you how to unit test your models quickly and easily with an awesome PHP database testing tool called [Phactory][1]. <!--more-->

[ Phactory][1] is an alternative to using fixtures for testing code that interacts with a database. What this means is that instead of building a fixture with xml and using it for an entire test case (series of tests), we can manipulate our test database directly on a per-test level. This allows use to quickly create only the records we need for a test, and remove them from the database after each test automatically.

**Installing Phactory** Phactory is available as a PEAR package and is super easy to install:

<pre class="brush:shell">pear channel-discover pearhub.org
pear install pearhub/Phactory</pre> Once Phactory is installed, you want to integrate Phactory with your test runner. You should probably create a new base test case for models using Phactory. Let's do that now:

<pre class="brush:php">/*
 * /models/shoe.php
 */</p>



<p>
  require_once 'PHPUnit/Autoload.php';
  require_once 'Phactory/lib/Phactory.php';
</p>



<p>
  /**
   * Test Case Base Class for using Phactory *
   */
  abstract class PhactoryTestCase extends PHPUnit_Framework_TestCase
  {
    protected static $db;
</p>



<p>
  public static function setUpBeforeClass()
    {
      //set up Phactory db connection
      self::$db = self::getConnection();
      Phactory::reset();
    }
</p>



<p>
  public static function tearDownAfterClass()
    {
      Phactory::reset();
    }
</p>



<p>
  protected function setUp()
    {
      Phactory::reset();
    }
</p>



<p>
  protected function tearDown()
    {
      Phactory::reset();
    }
</p>



<p>
  /**
     * Sets up Phactory PDO connect
     */
    public static function getConnection()
    {
      $options = array (
        'driver'   => 'mysql',
        'host'     => '127.0.0.1',
        'user'     => 'root',
        'password' => '',
        'database' => 'project_test_db'
      );
</p>



<pre><code>$pdo = new PDO($options['driver'].':host='.$options['host'].';dbname='.$options['database'], $options['user'], $options['password']);
Phactory::setConnection($pdo);
</code></pre>



<p>
  }
  }</pre>
  This abstract base class, <strong>PhactoryTestCase</strong>will be extended by our model class. In <strong>PhactoryTestCase::setUpBeforeClass()</strong>, which runs before all the tests in a given test case, we create the connection to the test database. We also want to ensure that any records are truncated in between individual tests. By calling <strong>Phactory::reset()</strong> I have ensured that all records and Phactory table definitions (<em>blueprints and inflections</em>) will be cleared.
</p>



<p>
  <strong>Setting Up the Test Database
  </strong>Your test database should be a copy of your project's main database, minus any records. A quick way to get a SQL dump of a database without any records is:


  <pre class="brush:shell">mysqldump --host="localhost" --user="root" project_db --no-data >; "schema.sql";</pre>
  Substitute the values for your project of course. What I am doing here is dumping the schema for the main database

  <em>project_db</em> into a sql file which I will use to create my test database. Go ahead and execute that sql on your test database, but be aware that you will have to keep any schema changes between the two databases in sync.
</p>



<p>
  <strong>Setting Up a Model to Test</strong>
</p>



<p>
  The model I am going to test today is called <strong>Shoe</strong>.


  <pre class="brush:php"><?php
/*
 * /tests/PhactoryTestCase.php
 */
class Shoe
{
  public function __construct($pdo)
  {
    $this->db = $pdo;
  }</p>



<p>
  public function getShoesByBrandName($brand_name = '')
    {
      if ($brand_name) {
</p>



<pre><code>  $sql   = "SELECT * FROM shoe_table WHERE brand_name = '{$brand_name}'";

  $shoes = array();

  foreach ($this->db->query($sql) as $row) {
    $shoes[] = $row;
  }

  return $shoes;
}
</code></pre>



<p>
  }
  }</pre>
  In this simple model, we are going to use a constructor supplied PDO object to act as the database. In fact, we are going to use the same PDO object that Phactory is using and just pass it in during our tests. In production of course you would be talking to your main database, but we need to be able to swap in our test db in order to test it. In practice, you will probably already have a database abstraction layer. The trick in that case would be to switch to the test database via a bootstrap,  or inject the test database config to your abstraction layer via your test runner.
</p>



<p>
  The model has one method, <strong>Shoe::getShoesByBrandName() </strong>which takes a string, the brand name. It executes some SQL and returns an array of shoes. Let's go ahead and set up a test case for this model.


  <pre class="brush:php"><?php
/*
 * /tests/models/ShoeTest.php
 */</p>



<p>
  require_once '/models/shoe.php';
  require_once 'PhactoryTestCase.php';
</p>



<p>
  class ShoeTest extends PhactoryTestCase
  {
    protected $sut;
</p>



<p>
  protected function setUp()
    {
      parent::setUp();
      $this->sut = new Shoe(parent::$db);
</p>



<pre><code>//make sure Phactory doesn't try to pluralize table name
Phactory::setInflection('shoe_table', 'shoe_table');

//create empty Phactory blueprint
Phactory::define('shoe_table', array());
</code></pre>



<p>
  }
  }</pre>
  Here is a basic test case for our Shoe model. In the <strong>ShoeTest::setUp()</strong> we call the methods hanging out in <strong>parent::setUp()</strong>, which includes setting up the connection to the database. Then I populate<strong> ShoeTest::sut</strong> with a new instance of our shoe model object, passing it the pdo object stored in <strong>PhactoryTestCase::$db</strong>.
</p>



<p>
  We also sometimes need to tell Phactory not to pluralize our table names when it does it's magic by calling <strong>Phactory::setInflection()</strong>.
</p>



<p>
  A blueprint must be defined for any tables we want to use in our test. We do this by calling <strong>Phactory::define().</strong> The first argument is the name of the table, the second argument is an array of default values that will be populated upon an insert. In this array you specify keys matching column names in your table with a default value. For now I am passing in an empty array because I don't want to set up any default values.
</p>



<p>
  Next, we create a method to test Shoe::getShoesByBrandName() w/ no args.


  <pre class="brush:php">  public function testGetShoesByBrandName()
  {
    $actual = $this->sut->getShoesByBrandName();
    $this->assertNull($actual);
  }</pre>
  This is usually always the first test I write since we should get a null value when passing no arguments to our method. Now for a real test.


  <pre class="brush:php">  public function testGetShoesByBrandName1()
  {
  //should only pull up Nike Shoes
    Phactory::create('shoe_table', array('id' => 123, 'color' => 'blue', 'brand_name' => 'Nike'));
    Phactory::create('shoe_table', array('id' => 234, 'color' => 'red',  'brand_name' => 'Nike'));</p>



<pre><code>//should not pull up
Phactory::create('shoe_table', array('id' => 345, 'color' => 'blue', 'brand_name' => 'Vans'));
Phactory::create('shoe_table', array('id' => 456, 'color' => 'blue', 'brand_name' => 'Reebok'));
Phactory::create('shoe_table', array('id' => 567, 'color' => 'blue', 'brand_name' => 'New Balance'));

$actual = $this->sut->getShoesByBrandName('Nike');

//should pull up first two rows
$this->assertEquals(2, count($actual));
</code></pre>



<p>
  }</pre>
  Here I use <strong>Phactory::create()</strong> to build 5 records for my test. When this test runs, Phactory will insert all of these rows into the test database. I then call <strong>Shoe::getShoesByBrandName() </strong>and expect to get back the two records that are Nike shoes. I assert that two rows have been pulled up by the method.
</p>



<p>
  <strong>That Was Easy</strong>
  You can also do more complicated multi-table relationships and joins in your Model code, and all you have to do in your test case is create a corresponding table blueprint. You will also need to create any records you need in all of the tables you are testing. This offers you incredible flexibility as opposed to developing with fixtures since you can control what records you need when you need them. You also don't need to specify every column when defining a table blueprint so you will save a ton of time over using a<a href="http://www.phpunit.de/manual/current/en/fixtures.html"> PHPUnit XML dataset</a>.
</p>



<p>
  Hopefully this post has given you incentive to take a look at Phactory. They have a very well written usage guide that goes into quite a bit more depth than the code I have presented. Make sure you check out the <a href="http://phactory.org/guide/">Phactory Homepage</a> and<a href="https://github.com/chriskite/phactory"> Phactory on GitHub</a>. I also want you to watch <a href="http://www.phparch.com/2011/03/webcast-phactory/">this video overview of Phactory</a> that features the author walking through several features of the component. Thanks for reading!
</p>

 [1]: http://phactory.org/guide/
 [2]: https://github.com/sebastianbergmann/phpunit/
