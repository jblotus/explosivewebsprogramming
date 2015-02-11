---
title: 'My Programming Heroes: Douglas Crockford'
author: jblotus
layout: post

dsq_thread_id:
  - 1010187022
categories:
  - Concurrent Programming
  - Design Patterns
  - HTML5
  - Javascript
  - jQuery
  - JSON
  - node.js
  - Progamming Heroes
  - Web Development
  - Programming
tags:
  - ajax
  - computer history
  - douglas crockford
  - grace hopper
  - haskell
  - javascript
  - json
  - monads
  - programming
  - programming heroes
  - robert martin
  - xss
  - yui
---
> JavaScript is a language that people would use without learning it first, which is by mistake that you would not make in other language &#8211; Douglas Crockford
I'm a huge fan of [Douglas Crockford][1], and that has a lot to do with how I learned [JavaScript][2]. Like many web developers, I generally loathed JavaScript until [jQuery][3] came along, and then I suddenly thought I understood the language. It wasn't until I had to write non-trivial applications in JavaScript that I truly began to notice the huge gaps in my education. With the rise of server-side JavaScript and the constant deluge of new interesting things happening in the JavaScript community, I felt that I needed to catch up, quickly. I needed a master to learn from, and Douglas Crockford was the master I chose.

<!--more-->

## Who is this Douglas Crockford?

[Douglas Crockford discovered JSON][4], you know, the language that [relegated XML to second-class status][5]? Everywhere I looked all you heard was [Crockford this,][6] [Crockford that][7]. JSLint will make you cry, they said. Let's not even get started on the [debate about semicolons][8]. So clearly [Crockford is an important character in the JavaScript world][9]: serves on the ECMAScript board, former-chief JavaScript architect at Yahoo [and now PayPal][10]. Oh yeah, he also wrote a pretty popular book, [JavaScript: The Good Parts][11]. Douglas is also a [master of hyperbole][12].

[There is a ton of stuff to cover with Crockford][13], but his recorded talks are probably one of my favorite resources and frankly I fall asleep with these playing on my iPad quite regularly. If you are looking for some free JavaScript education, do yourself a favor and **WATCH ALL OF HIS VIDEOS**.

##

## YUI Library Lectures

This is an awesome[ **8-part series** of lectures given by Crockford while he was still Chief JavaScript Architect at Yahoo][14]. This is a great starting point for anyone looking to learn more about JavaScript.

### Volume 1 &#8211; The Early Years

In this video, Douglas Crockford covers pretty much the entire history of our industry! This video taught me about [Grace Hopper and her contributions to programming][15], such as [creating the first compiler][16] and [coining the term &#8220;bug&#8221;][17]. He also covers CPU architecture, mainframes, and the first software. **Too many programmers today don't bother learning anything about the origin of programming** and the fact that he was able to sum it up in just under two hours is quite impressive.

http://www.youtube.com/watch?v=JxAXlJEmNMg

### Chapter 2 &#8211; And Then There Was JavaScript

Here, Douglas Crockford discusses the origins and history of JavaScript, specifically it's good bad parts and the dangerous bad parts. This is a great fundamental introduction to how this language actually works and frankly most people who watch this should pick up a lot of good information. **This is a fairly technical talk.**

http://www.youtube.com/watch?v=RO1Wnu-xKoY

### Act III &#8211; Function the Ultimate

In this video, Crockford describes the very best part of JavaScript, functions. This is a critically important talk because Crockford covers often-misunderstood topics like hoisting, function hoisting, scope, closure, and recursion. He also covers JavaScript's version of inheritance, both pseudo-classical and prototypal, a common source of confusion for JavaScript developers. **This talk is very code heavy, so grab some Red Bull or coffee.**

http://www.youtube.com/watch?v=ya4UHuXNygM

### Episode IV &#8211; The Metamorphosis of Ajax

This video focuses on the origins of Ajax, which was born out of [work by Microsoft][18] and applied by [Jesse James Garret][19] to create the version of ajax we use today. Crockford also gives us a bit of history about HTML and how it evolved in the beginning, went on to evolve during the browser wars, and how it lead to the awful DOM we have today. There is a lot of talk about web standards and how browsers actually work, including the DOM. This is great video for users of modern libraries like jQuery to understand why those libraries exist in the first place.

http://www.youtube.com/watch?v=Fv9qT9joc0M

### Part V &#8211; The End of All Things

> If you don't have these vulnerabilities in your browser (XSS), it is not standards compliant. So there is something deeply, deeply wrong with the standards of the web.. -Douglas Crockford
This talk covers important topics like [cross-site scripting attacks (XSS)][20], browser vulnerabilities, [including why HTML5 actually makes the situation worse][21]. He also covers some interesting ideas about creating secure JavaScript code, such a [programming by capability][22]. Other topics covered are JavaScript performance and bench-marking  and more best practices, specifically discussing some of the rules in[ JSLint][23] and how they came about.

http://www.youtube.com/watch?v=47Ceot8yqeI

### Scene 6 &#8211; Loopage[ ][24]

This video talks about the Event Loop, one of the most important features of the JavaScript language. Crockford covers the origin of I/O, from the time-sharing mainframe systems of old, the user-friendly [HyperCard][25] for the Mac, and the danger of threads. He also discusses server and client architecture for modern web applications, using node and YUI to build fast, performant programs.

http://www.youtube.com/watch?v=QgwSUtYSUqA

### Level 7 &#8211; ECMAScript 5: The New Parts

This video has Douglas Talking about [ECMAScript 5][26], and some of the challenges in designing the standard to replace ECMAScript 3. One of the interesting things I learned in this video is how difficult it is to actually move the standard forward (ECMAScript 3 has been out since 1999) and why it takes so long to make these changes (HINT: It's about not breaking the web&#8230;and Microsoft). This talk gives a nice tour of some of the new features of the language and even offers some insights about where [ES6/ESNext][27] is going, which he hopes will fix some of the problems the ECMAScript 5 was unable to address.

http://www.youtube.com/watch?v=UTEqr0IlFKY

### Section 8 &#8211; Programming Style & Your Brain

> JavaScript and brains? What's the connection? -Douglas Crockford
This talk is mostly about how consistent programming style affects good habits and reduces errors in our code. This is probably one of the more controversial things Douglas Crockford is known for, considering how &#8220;subjective&#8221; most programmers are about coding style. Douglas tries to make the argument that these things are actually less subjective than we think  and that by following a consistent and rigorous style, we actually produce better code.

http://www.youtube.com/watch?v=taaEzHI9xyY

&nbsp;

## More Awesome Crockford Videos

Did you finish the YUI Lectures? Don't worry, there are still [a ton of great Crockford videos][28] out there. I haven't posted all of the ones I know about, just some of my favorites that I think are the most useful and entertaining.

### Monads & Gonads

Douglas is always good with titles, but this talk doesn't require balls to watch. Douglas talks about [Haskell][29], and the [concept of a Monad][30] and how you can achieve this programming style in JavaScript. The topic of promises, concurrency and parallelism are also discussed in detail. I still haven't gotten my head around this topic, so **I've been watching this video frequently**.

http://www.youtube.com/watch?v=dkZFtimgAcM

### What Would Crockford Do?

Well, [Yahoo had a big talent drain awhile ago][31], including one of their biggest assets, as [Doug jumped over to PayPal][10]. Never one to shy away from controversy, he gave a talk about how he would have run Yahoo had he been in charge of the company. I think there is an element of voyeurism here that makes it hard to stop watching and ultimately even though this isn't really a technical talk, it is one of my **favorite Crockford talks of all time**.

http://www.youtube.com/watch?v=8HzclYKz4yQ

### JavaScript: Your New Overlord

For context, this talk was given at [JAX, which is a Java and Android conference][32]. In this talk Douglas pretty much talks smack about XML, the enterprise, JavaScript &#8211; and why JavaScript is now the language of the future in spite of how maligned it is by  developers in other languages. You get a nice overview of the history of the language and the good parts. Douglas also covers [JavaScript as the ultimate VM][33], including languages that compile to JavaScript like [CoffeeScript][34], and even old languages that are being compiled to JavaScript.

http://www.youtube.com/watch?v=Trurfqh_6fQ

## What else?

You can probably accuse me of being a Douglas Crockford fan by now, and you would be right. What I appreciate most about him is how he presents his opinions. His arguments are always backed up by some reasoning, no matter how opinionated. Some people do not agree with Crockford, and to those people I offer this except from [Clean Code][35], a book by another on of my programming heroes, [Robert Martin][36]:

<img class="wp-image-1063 alignnone" alt="cleancodecover" src="http://www.jblotus.com/wp-content/uploads/2013/01/cleancodecover.jpg" width="192" height="254" />

> Students of these approaches immerse themselves in the teachings of the founder. They dedicate themselves to learn what that particular master teaches, often to the exclusion of any other master’s teaching. Later, as the students grow in their art, they may become the student of a different master so they can broaden their knowledge and practice. Some eventually go on to reﬁne their skills, discovering new techniques and founding their own schools. None of these different schools is absolutely right. Yet within a particular school we act as though the teachings and techniques are right. After all, there is a right way to practice Hakkoryu Jiu Jitsu, or Jeet Kune Do. But this rightness within a school does not invalidate the teachings of a different school -Robert Martin (Clean Code)
### Follow a Master

Essentially, I immersed myself in Douglas Crockford's teachings because he is a **worthy master**. Without these videos, I'm pretty sure my interest in JavaScript or my knowledge wouldn't be at the level it is today. There are many other masters of the JavaScript language you might also want to follow: [John Resig][37], [Jeremy Ashkenas][38], [Addy Osmani][39], [Yehuda Katz][40], [Thomas Fuchs][41], [Paul Irish][42], [TJ Holowaychuk][43], [James Halliday][44], [Isaac Schlueter][45], etc. So my final advice today would be:** pick a master and immerse yourself!**

 [1]: http://www.crockford.com/
 [2]: https://developer.mozilla.org/en-US/docs/JavaScript
 [3]: http://jquery.com/
 [4]: http://www.youtube.com/watch?v=-C-JoyNuQJs
 [5]: http://blog.programmableweb.com/2011/05/25/1-in-5-apis-say-bye-xml/
 [6]: https://github.com/twitter/bootstrap/issues/3057#issuecomment-5135512
 [7]: http://ajaxian.com/archives/doug-crockford-and-the-online-booty-call-saga
 [8]: http://dailyjs.com/2012/04/19/semicolons/
 [9]: http://yuiblog.com/crockford/
 [10]: http://techcrunch.com/2012/05/13/paypal-gets-its-own-share-of-the-yahoo-diaspora-hires-java-icon-douglas-crockford/
 [11]: http://shop.oreilly.com/product/9780596517748.do
 [12]: http://commonquote.com/author/15734/douglas-crockford
 [13]: http://javascript.crockford.com/
 [14]: http://developer.yahoo.com/yui//theater/
 [15]: http://en.wikipedia.org/wiki/Grace_Hopper
 [16]: http://en.wikipedia.org/wiki/A-0_System
 [17]: http://www.waterholes.com/~dennette/1996/hopper/bug.htm
 [18]: http://www.w3.org/TR/XMLHttpRequest/
 [19]: http://en.wikipedia.org/wiki/Jesse_James_Garrett
 [20]: http://en.wikipedia.org/wiki/Cross-site_scripting
 [21]: http://thechangelog.com/post/676820023/episode-0-2-6-douglas-crockford-on-json-and-javascript-f
 [22]: http://en.wikipedia.org/wiki/Object-capability_model
 [23]: http://www.jslint.com/
 [24]: https://github.com/zephyrfalcon/magicripper2
 [25]: http://en.wikipedia.org/wiki/HyperCard
 [26]: http://en.wikipedia.org/wiki/ECMAScript
 [27]: https://wiki.mozilla.org/ES6_plans
 [28]: http://www.youtube.com/playlist?list=PLwDFnUJNt1B0MOseiYYN95Cht60zqwplH
 [29]: http://www.haskell.org/haskellwiki/Haskell
 [30]: http://www.haskell.org/tutorial/monads.html
 [31]: http://techcrunch.com/2012/04/04/yahoo-cuts-14-percent-of-workforce-2000-given-pink-slips/
 [32]: http://jaxconf.com/
 [33]: http://www.hanselman.com/blog/JavaScriptIsAssemblyLanguageForTheWebSematicMarkupIsDeadCleanVsMachinecodedHTML.aspx
 [34]: http://coffeescript.org/
 [35]: http://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882
 [36]: https://twitter.com/unclebobmartin
 [37]: http://ejohn.org/
 [38]: https://github.com/jashkenas
 [39]: http://addyosmani.com/blog/
 [40]: http://yehudakatz.com/
 [41]: http://mir.aculo.us/
 [42]: http://paulirish.com/
 [43]: http://tjholowaychuk.com/
 [44]: https://twitter.com/substack
 [45]: https://github.com/isaacs
