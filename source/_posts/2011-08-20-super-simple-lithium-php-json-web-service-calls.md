---
title: Super Simple Lithium PHP JSON Web Service Calls
author: jblotus
layout: post

dsq_thread_id:
  - 391177937
categories:
  - JSON
  - Lithium PHP
  - PHP
  - PHP Frameworks
  - Web Development
  - Programming
  - Web Services
tags:
  - json
  - lithium
  - lithium php
  - php
  - php steam functions
  - web scraping
  - web services
---
My new application written using the [Lithium PHP Framework][1] needed to make [API calls to Apple&#8217;s iTunes service][2]. The web service in question returns JSON which can be parsed and used to build an array or object on our server via php&#8217;s native[ json_decode()][3]. I couldn&#8217;t find any documentation about setting up a simple request like this but with some digging I found the[ lithium\net\http\Service class][4] ready to do the job. Instead of loading up Curl or some other method, I can pull a json object down from a remote web server (or any webpage) and parse the body in less than 3 lines of code.

**Looking at the lithium\net\http\Service class**

<pre class="brush:php">/**
     * Initializes a new <code>Service</code> instance with the default HTTP request settings and
     * transport- and format-handling classes.
     *
     * @param array $config
     */
    public function __construct(array $config = array()) {
        $defaults = array(
            'persistent' =&gt; false,
            'scheme'     =&gt; 'http',
            'host'       =&gt; 'localhost',
            'port'       =&gt; null,
            'timeout'    =&gt; 30,
            'auth'       =&gt; null,
            'username'   =&gt; null,
            'password'   =&gt; null,
            'encoding'   =&gt; 'UTF-8',
            'socket'     =&gt; 'Context'
        );
        parent::__construct($config + $defaults);
    }</pre> A cursory glance at the file revealed the the connection options need to be passed in via an $options config array in the class constructor. This is a lot like

[CakePHP&#8217;s HttpSocket class][5]. For the purposes of an example, lets say we want to pull down all the Jack Johnson items via his artist id in iTunes. If you click <http://itunes.apple.com/lookup?id=909253> you will get a nice json string representing the collection. Let&#8217;s go ahead and create a controller interface with the API.

Create **app/controllers/ItunesController.php**

<pre class="brush:php"><?php
namespace app\controllers;
use lithium\net\http\Service;</p>



<p>
  class ItunesController extends \lithium\action\Controller {
</p>



<p>
  public function get()
    {
      //instantiate the service connection
      $service = new Service(array('host' => 'itunes.apple.com'));
</p>



<pre><code>//capture the response
$data = $service-&gt;get('/lookup?id=909253');

//decode the json
$songs = json_decode($data);

//now we get a nice object to work with
var_dump(songs);

//object(stdClass)[28]
//  public 'resultCount' =&gt; int 1
//  public 'results' =&gt;
//    array
//      0 =&gt;
//        object(stdClass)[29]
//          public 'wrapperType' =&gt; string 'artist' (length=6)
//          public 'artistType' =&gt; string 'Artist' (length=6)
//          public 'artistName' =&gt; string 'Jack Johnson' (length=12)
//          public 'artistLinkUrl' =&gt; string 'http://itunes.apple.com/us/artist/jack-johnson/id909253?uo=4' (length=60)
//          public 'artistId' =&gt; int 909253
//          public 'amgArtistId' =&gt; int 468749
//          public 'amgVideoArtistId' =&gt; null
//          public 'primaryGenreName' =&gt; string 'Rock' (length=4)
//          public 'primaryGenreId' =&gt; int 21
}
</code></pre>



<p>
  }
</p>



<p>
  ?></pre>
  Dont forget to include the namespace to include the Service library at the top of your controller. You can of course play with the constructor options to work with nearly any web url. Best of all this doesn&#8217;t need CURL to be installed with your php since it relies upon php native streams functions. This is some really awesome stuff and I must say that I am finding it trivially easy to implement a web service using Lithium. I could easily dump the iTunes objects into MongoDB for later retrieval with very little conversion.
</p>



<p>
  As always please share your thoughts in the comments.
</p>

 [1]: http://lithify.me/ "Lithium PHP Framework"
 [2]: http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html "Applie iTunes API docs"
 [3]: http://php.net/manual/en/function.json-decode.php "php json_decode manual"
 [4]: http://lithify.me/docs/lithium/net/http/Service "Lithium PHP Basic Web Service class"
 [5]: http://book.cakephp.org/view/1517/HttpSocket "CakePHP HttpSocket"