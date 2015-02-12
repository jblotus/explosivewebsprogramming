---
title: 'Introducing the jQuery easyBars plugin - Simple Progress Meters'
author: jblotus
layout: post

dsq_thread_id:
  - 388846921
categories:
  - Javascript
  - jQuery
  - jQuery easyBars
  - jQuery Plugins
  - Web Development
  - Programming
tags:
  - easybars
  - javascript
  - jquery
  - jquery plugins
---
For part of a new project at work, I needed to be able to render a progress bar showing progress towards completion of an assignment. I had not yet made a proper jQuery plugin so I though it would be a good idea to release this so others could take advantage of it or possibly contribute back code if they have improvements.

The idea behind jQuery easyBars is that you can transform an element (probably a div) into a progress bar. The attributes of the bar are designed to be easily configured with intelligent defaults.

<a href="http://www.jblotus.com/wp-content/uploads/2011/08/jquery-easybars-mockup.jpg"><img class="img-thumbnail" title="jquery-easybars-mockup" src="http://www.jblotus.com/wp-content/uploads/2011/08/jquery-easybars-mockup-550x343.jpg" alt="jQuery easyBars plugin mockup" width="550" height="343" /></a>

For example, the progress bar will take on the width, height of the target element, but can be easily overridden in the easyBars options. I also allow for HTML5 data attributes to be used to control the appearance of the bars.

For a complete example please check out the [jQuery easyBars demo page][1].

You can also [download/fork the project on GitHub][2]

 [1]: http://jblotus.github.com/jQuery-easyBars/demo/demo.html "jQuery easyBars super simple progress meters demo page"
 [2]: https://github.com/jblotus/jQuery-easyBars "Download or Fork the jQuery easyBars project on GitHub"
