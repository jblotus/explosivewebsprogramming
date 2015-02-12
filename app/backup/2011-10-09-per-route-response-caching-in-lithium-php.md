---
title: Per-Route Response Caching in Lithium PHP
author: jblotus
layout: post

dsq_thread_id:
  - 438547825
categories:
  - Caching
  - Lithium PHP
  - MVC
  - Web Development
  - Programming
tags:
  - lithium
  - lithium filters
  - lithium php
  - lithium router
  - page caching
---
Occasionally, you will want to cache entire responses from different areas of your Lithium app. [Micheal N already posted a nice tip about response caching][1] by applying a filter to the `Dispatcher::run()` method, but I wanted to be able to set my cache times differently per route. Now in keeping with the aspect-oriented style, I want to define my cache times right in my `routes.php` file. With a bit of closure magic, combined with Lithium's filter system, this was a piece of cake. <!--more--> I figure that the best way to do this is by adding a bit of extra data to the

`Request` object before it hits `Dispatcher::run()` method, but just after a route is matched.

`in app/config/bootstrap/routes.php`

<pre class="brush:php">Router::connect('/podcasts/{:slug:[\w&#45;]+}', 'Episodes::index', function($request) {
  $request-&gt;cache = array(
    'key' => md5($request-&gt;url),
    'time' => '+10 minutes',
  );
  return $request;
});</pre> So in my application, I am trying to match a url that looks like:

`/podcasts/the-javascript-show`. This will normally take me to the Episodes controller's index action, passing `:slug` as an attribute of the request object. You will notice that for the third argument of `Router::connect()`, I have passed a closure to intercept the request object to modify the request object and add some caching info to a new attribute. Finally and importantly, I return the `$request` object. If you don't return that the Dispatcher won't know how to process your route.

The second step of this process involves adding a filter to `Dispatcher::run()` to handle the caching aspect.

`in app/config/bootstrap/action.php`

<pre class="brush:php">//make sure to use the Cache class namespace so Lithium will autoload it
use lithium\storage\Cache;</p>



<p>
  //now add a filter
  Dispatcher::applyFilter('run', function($self, $params, $chain) {
</p>



<p>
  //need to parse request object for route & cache settings
    $request = Router::process($params['request']);
</p>



<p>
  if (empty($request->cache)) {
      //no cache required, return as normal
      $response = $chain->next($self, $params, $chain);
</p>



<p>
  } else {
      //figure out the key we are going to use
      $key  = !empty($request->cache['key']) ? $request->cache['key'] : $request->url;
      $time = !empty($request->cache['time']) ? $request->cache['time'] : '+1 hour';
</p>



<pre><code>//read the response from cache
$response = Cache::read('default', $key);

if (!$response) {
  $response = $chain-&gt;next($self, $params, $chain);
  Cache::write('default', $key, $response, $time);
}
</code></pre>



<p>
  }
</p>



<p>
  return $response;
  });</pre>
</p>



<p>
  So what we are doing here is adding a filter to <code>Dispatcher::run()</code>. The first thing we have to do is process the request object by passing it off to the router. This is important because at this phase of the application request cycle, the request object has not yet been parsed. <code>Dispatcher::run()</code> will eventually do this before it returns a response, but we won't be able to intercept our custom cache attribute if we don't do it here.
</p>



<p>
  Next, we check to see if a cache attribute is set on the <code>$request</code> object. If not, then we just want to return the normal <code>$response</code> that the dispatcher would generate by capturing the next link in the filter chain. If we do happen to have a cache attribute on the <code>$request</code> object, we want to determine the <code>$key</code>, and <code>$time</code> to store the response. Normally we will specify the key in the <code>Router::connect()</code> method as shown previously, but I figure we can set some defaults here in case we forgot.
</p>



<p>
  We then try to read the response from cache based upon the given key. If we find it then that becomes the <code>$response</code> object. If we don't, the <code>$response</code> object is captured from the filter chain and stored in cache. Finally you must return the <code>$response</code> object.
</p>



<p>
  <strong>Benefits</strong>
  This technique bypasses the controller and renderer entirely once the response is cached which can have significant performance benefits.
</p>



<p>
  <strong>Caveats</strong>
  Since this caches the entire response, it is not really suitable for pages that are highly dynamic on the server side. A perfect example is a login menu that only shows up once logged in. A better use might be for json responses for api calls, or pages that are not contained in a layout that changes based upon user action. For that, you are better off using something like element caching, or query result caching.
</p>

 [1]: http://nitschinger.at/Caching-responses-in-Lithium
