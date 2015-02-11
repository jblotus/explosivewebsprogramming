---
title: 'Why can&#8217;t someone just make a good IDE for PHP?'
author: jblotus
layout: post

dsq_thread_id:
  - 744178104
categories:
  - Career
  - IDE Love
  - IDE Hate
  - PHP
  - Web Development
  - Programming
tags:
  - dreamweaver
  - ide
  - integrated development environment
  - netbeans
  - php
  - php ide
  - phpstorm
  - software development
  - software philosophy
  - zend studio
---
I think the first &#8220;[IDE][1]&#8221; I ever used for web development would have to be [Adobe Dreamweaver][2] (née Macromedia). Ok, so maybe it was [Microsoft FrontPage][3], but that shouldn&#8217;t really count. Dreamweaver for the novice held the promise of being able to easily create [DHTML][4] animations and offered [WYSIWYG][5] perfection for creating web interactive pages. All of the deployment you needed was built in (FTP) and you did get some sense that it was the best product on the market for doing web work (in fact it still might be for your average designer). This post is about my experience moving away from the oft-maligned program and some lessons learned in my quest for the perfect IDE. <!--more-->

### Ignorance is Bliss

I wrote a ton of [PHP][6] code in Dreamweaver. In fact, I learned PHP while using Dreamweaver to edit [joomla][7] templates back when I was a &#8220;website manager&#8221;. I went on to write thousands of lines of code in Dreamweaver for about a year into my programming journey. Eventually though, I started to feel like I was missing out on something. I decided to search for dedicated PHP development software and realized were at least 10 IDE programs available for PHP and clearly Dreamweaver was not among them. I decided then and there that I needed to move forward and ditch my old reliable Dreamweaver.

As a novice, it was an [overwhelming experience even trying to pick an IDE][8]. I was writing a large website with [CakePHP][9] at the time and figured that I would look around and see what others in that community were using. For the record, candidates included: [Komodo][10], [NetBeans][11], [Eclipse PDT][12], [Zend Studio][13], [PHPStorm][14] and a few more. I downloaded a few different programs and had a difficult time evaluating them because after all, how would a beginner know what made for a good IDE? Additionally, some of these programs cost money and I did not have a penny to spare in those days. So it had to be free.

### NoobBeans

I settled on NetBeans initially. I had heard of this nifty feature called &#8220;[code-completion][15]&#8221; and some people said that you could use it to [auto-complete model class relationships and methods for CakePHP][16]. I had mixed results with that, but in time code-completion in general was quickly becoming something that I could not live without. NetBeans also had built-in FTP and [SVN][17] integration. That was neat, especially since I had begun using version control with [TortoiseSVN ][18]recently (it seemed important to learn). Eventually I found the auto-format code button, and realized my coding style was pretty sloppy compared to the corrections it was making.

I gave NetBeans a daily workout for a good year or two, but then I decided wanted to make mobile apps for a living. After learning that I needed a mac to make iPhone applications, I decided that I would have to [start out with android][19]. Of course this required me to learn Java, and also introduced me to Eclipse. Once I started using Eclipse in a Java context, I was starting to feel that net beans was lacking a ton of configuration options that eclipse offered. So I downloaded [Eclipse PDT][12], which is basically a free version of Zend Studio.** Now I felt powerful**.

### The 7th Circle of Hell is called Eclipse

When I started at a new company, I was assigned a pretty low spec machine to do PHP development work on. The codebase I was working on was also massive. Eclipse couldn&#8217;t keep up. Things were always building, indexing or validating! I could count on Eclipse crashing at least five times a day. Eventually I began fighting back by disabling validators and automatic project building. I was turning off feature after feature and started to long for the good old days of Dreamweaver.

Once my workstation was upgraded to an HP Intel i7 with 12 gigs of ram, most of my crashing problems started to go away. I also tried several favors of eclipse like Aptana, and finally ended up using Zend Studio on a trial basis. Zend Studio seemed at least a little bit better than Eclipse PDT, because it had refactoring features and a more advanced formatter amongst other things not included with PDT. I found zend studio to be a bit more stable, but ultimately the refactoring featurs never seemed to work for me, and the code formatter was just crappy. Now I found myself spending more time using the backspace, enter and delete keys to correct sends formatting wonkyness than actually coding. That is hyperbole, but I was starting to see why all the Ruby guys used Textmate or vim.

### It&#8217;s just a glorified text editor

I hate [vim][20], and I&#8217;m not on a mac so [textmate][21] is out. So along comes [Sublime Text 2][22]. I started using Sublime Text casually on some of my hobby projects and I started to understand why people preferred a lighter text editor. Sometimes I just felt like the editor was reading mind and I never had performance issues. Still as much as I enjoy Sublime Text, I didn&#8217;t find that it gave me a productivity advantage overall. I personally don&#8217;t want to hop around my computer for different tools to get my code tested and deployed, which I don&#8217;t do as often with an IDE. Additionally, the speed of searching for code in my IDE is lightning fast and I could never achieve those results with a text editor.

So I was stuck with Zend Studio and i was afraid. I knew I was one corrupt workspace away from disaster. But it wasn&#8217;t all bad. I had a few macros, a few useful plugins and my workflow was sufficiently fast. I could live with this. Things went along fine for months, until I went to a meetup of the [South Florida Php Users Group][23]. There I was introduced to [PHPStorm][14]. Just another IDE I thought to myself, as I debated the merits of switching from Eclipse. I was so damn wrong.

### Salvation is a (PHP)Storm

I tried out PHPStorm for a free 30 day trial. Several ex-eclipse users can be wrong right? Besides I had some side projects to hack on and eclipse was borked on my laptop. Once up and running, PHPStorm made me a quick convert. Everything just worked better. The code completion was faster and more accurate, the settings easier to manage. Unlike Eclipse PDT, Zend Studio or [Aptana][24], it didn&#8217;t feel like someone had grafted to php support onto a IDE written for programming in Java – although I&#8217;m sure PHPStorm is entirely based on [IntelliJ IDEA][25].

Features starting popping up all the time that would pleasantly surprise me: [Zen Coding support][26], [inline string language injection][27], doc block validation and more. PHPStorm had all of the stuff Zend Studio gave me, but is mostly better implemented. Stability is also vey good, as I haven&#8217;t seen a crash yet in two months. My i5 laptop did have a bit of sluggishness on a really large project, but I tweaked a few settings and everything seemed reasonably fast again. I still run in to some annoyances with the code formatter and some incorrect code syntax warnings, but therein lies another lesson. I don&#8217;t think an IDE or any tool for that matter is free of blemishes.

### Always end on a Cliché

Overall I think I will stick it out with PHPStorm for awhile. Looking back I realize that IDE&#8217;s have been a constant annoyance and yet have prompted so much growth and development in my skill level. I just plain work faster and write better code with an IDE. I also think that the problems I have ran into with IDE&#8217;s are true about software in general. We search for better solutions and always fall short.

**I am reminded of a famous quote:**

> Perfect is the enemy of good* –[Voltaire][28]*
The reality is that I could probably still get a lot done in Dreamweaver, but it is the journey on the quest for improvement that can help us truly master our craft.

 [1]: http://en.wikipedia.org/wiki/Integrated_development_environment
 [2]: http://www.adobe.com/devnet/dreamweaver.html
 [3]: http://en.wikipedia.org/wiki/Microsoft_FrontPage
 [4]: http://en.wikipedia.org/wiki/Dynamic_HTML
 [5]: http://en.wikipedia.org/wiki/WYSIWYG
 [6]: http://www.php.net/
 [7]: http://www.joomla.org/
 [8]: http://coding.smashingmagazine.com/2009/02/11/the-big-php-ides-test-why-use-oneand-which-to-choose/
 [9]: http://cakephp.org/
 [10]: http://www.activestate.com/komodo-ide
 [11]: http://netbeans.org/features/php/
 [12]: http://www.eclipse.org/projects/project.php?id=tools.pdt
 [13]: http://www.zend.com/en/products/studio/
 [14]: http://www.jetbrains.com/phpstorm/
 [15]: http://en.wikipedia.org/wiki/Autocomplete
 [16]: http://bakery.cakephp.org/articles/SymenTimmermans/2009/01/21/model-based-code-insight-and-completion-in-netbeans
 [17]: http://subversion.tigris.org/
 [18]: http://tortoisesvn.net/
 [19]: http://developer.android.com/sdk/index.html
 [20]: http://www.vim.org/ "VIM"
 [21]: http://macromates.com/ "TextMate"
 [22]: http://www.sublimetext.com/2
 [23]: http://www.soflophp.org/
 [24]: http://aptana.com/
 [25]: http://www.jetbrains.com/idea/
 [26]: http://code.google.com/p/zen-coding/ "Zen Coding Project Homepage"
 [27]: http://www.jetbrains.com/phpstorm/webhelp/language-injections.html "PHPStorm language injection"
 [28]: http://www.famous-quotes.net/Quote.aspx?The_perfect_is_the_enemy_of_the_good "Voltaire's famous quote about perfection"
