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
My new application written using the [Lithium PHP Framework][1] needed to make [API calls to Apple's iTunes service][2]. The web service in question returns JSON which can be parsed and used to build an array or object on our server via php's native[ json_decode()][3]. I couldn't find any documentation about setting up a simple request like this but with some digging I found the[ lithium\net\http\Service class][4] ready to do the job. Instead of loading up Curl or some other method, I can pull a json object down from a remote web server (or any webpage) and parse the body in less than 3 lines of code.

**Looking at the lithium\net\http\Service class**


```
<?php
/**
 * Initializes a new Service instance with the default HTTP request settings and
 * transport- and format-handling classes.
 *
 * @param array $config
 */
  public function __construct(array $config = array()) {
    $defaults = array(
        'persistent' => false,
        'scheme'     => 'http',
        'host'       => 'localhost',
        'port'       => null,
        'timeout'    => 30,
        'auth'       => null,
        'username'   => null,
        'password'   => null,
        'encoding'   => 'UTF-8',
        'socket'     => 'Context'
    );
    parent::__construct($config + $defaults);
  }
```

A cursory glance at the file revealed the the connection options need to be passed in via an $options config array in the class constructor. This is a lot like [CakePHP's HttpSocket class][5]. For the purposes of an example, lets say we want to pull down all the Jack Johnson items via his artist id in iTunes. If you click <http://itunes.apple.com/lookup?id=909253> you will get a nice json string representing the collection. Let's go ahead and create a controller interface with the API.

Create `app/controllers/ItunesController.php`

```
<?php
namespace app\controllers;
use lithium\net\http\Service;

class ItunesController extends \lithium\action\Controller {

  public function get()
    {
      //instantiate the service connection
      $service = new Service(array('host' => 'itunes.apple.com'));

      //capture the response
      $data = $service->get('/lookup?id=909253');

      //decode the json
      $songs = json_decode($data);

      //now we get a nice object to work with
      var_dump(songs);

      //object(stdClass)[28]
      //  public 'resultCount' => int 1
      //  public 'results' =>
      //    array
      //      0 =>
      //        object(stdClass)[29]
      //          public 'wrapperType' => string 'artist' (length=6)
      //          public 'artistType' => string 'Artist' (length=6)
      //          public 'artistName' => string 'Jack Johnson' (length=12)
      //          public 'artistLinkUrl' => string 'http://itunes.apple.com/us/artist/jack-johnson/id909253?uo=4' (length=60)
      //          public 'artistId' => int 909253
      //          public 'amgArtistId' => int 468749
      //          public 'amgVideoArtistId' => null
      //          public 'primaryGenreName' => string 'Rock' (length=4)
      //          public 'primaryGenreId' => int 21
  }
}
```

Dont forget to include the namespace to include the Service library at the top of your controller. You can of course play with the constructor options to work with nearly any web url. Best of all this doesn't need CURL to be installed with your php since it relies upon php native streams functions. This is some really awesome stuff and I must say that I am finding it trivially easy to implement a web service using Lithium. I could easily dump the iTunes objects into MongoDB for later retrieval with very little conversion.

As always please share your thoughts in the comments.

 [1]: http://lithify.me/ "Lithium PHP Framework"
 [2]: http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html "Applie iTunes API docs"
 [3]: http://php.net/manual/en/function.json-decode.php "php json_decode manual"
 [4]: http://lithify.me/docs/lithium/net/http/Service "Lithium PHP Basic Web Service class"
 [5]: http://book.cakephp.org/view/1517/HttpSocket "CakePHP HttpSocket"
