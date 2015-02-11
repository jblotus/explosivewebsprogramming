---
title: Diving in to Git
author: jblotus
excerpt: If the only thing stopping you from using git is the lack of tools, do yourself a favor and learn how to do it with the command line.
layout: post
dsq_thread_id:
  - 312027411
categories:
  - Version Control
  - Web Development
  - Programming
tags:
  - beanstalk
  - beanstalkapp
  - cakephp
  - git
  - github
  - mysysgit
  - netbeans
  - subversion
  - svn
---
It seems like [git ][1]is all the rage these days, but I have been steadfastly sticking to [subversion][2] (SVN) due to it's mature IDE integration and the sheer breadth of tools available for it. But today I finally took the plunge and set up my first repo on [GitHub][3], which seems to be the dominating force in public code hosting these days. I think SVN has lost the open-source versioning war against Git for a few reasons:

  * No permission needed to fork a project
  * Everyone gets their own copy of the repo locally
  * [GitHub][3] is sexy

I didn't get my git account going until I needed to [submit a patch for a simple bug in the cakephp HtmlHelper documentation][4]. I actually forked the project, and edited the file directly on GitHub since it was a small change. I then submitted a ticket over at the [CakePHP issue tracker][5] and [Mark Story][6], the Lead Developer,  pulled in my change almost instantly! If this were subversion I would have to first get access to the repo, and then create a new branch, change the file, ask for a merge, etc, etc.

I determined that my next move should be to create a project on git and set up my box to edit locally. I followed [GitHub's guide to getting git installed on my windows machine][7] and the installation was somewhat trival. I did have to wrap my head around the whole flow of working with git.

My project is a resume written in [PHP ][8]and using [less.js][9], which I wanted to experiment with since it sounds so damn cool (which it is). After creating my project on [GitHub][10], I followed the simple instruction they printed out. It worked kind of like this:

## Global setup:

<pre>Download and install <a href="http://git-scm.com/download" target="_blank">Git</a>
  git config --global user.name "James"
  git config --global user.email jblotus@gmail.com
</pre>

## Next steps:

<pre>mkdir myrepodir
  cd myrepodir
  git init
  touch README
  git add README
  git commit -m 'first commit'
  git remote add origin git@github.com:jblotus/myrepodir.git
  git push origin master</pre> Let me try and explain what's going on here:

<pre>git config --global user.name "James"
git config --global user.email jblotus@gmail.com
</pre> Obviously installing git was a prerequisite, which I did using

[mysysgit][11]

<pre>git init
</pre> This is how you create the repo

<pre>touch README
</pre> Touch is a nix command that creates an empty file with the name you specify.

[GitHub][3] uses README as a description o the project on the project landing page.

<pre>git add README</pre> This tells git to start tracking changes for a specfic file.

<pre>git commit -m 'first commit'
</pre> This will commit the file to your repo(your local repo SVN users). The

* -m * argument simply adds a one line commit message. Optionally you can omit it and a VIM editor will pop up and you can edit your message that way.

<pre>git remote add origin git@github.com:jblotus/myrepodir.git
</pre> I assume that this command tells your local git repo that it is a mere child of the remote git master overlord located at the url provided by

[GitHub][3].

<pre>git push origin master
</pre> This pushes (commits your local repo) to github. At this point it will probably ask for a password which you will have, because you set up your ssh keys when you followed the github instructions.

## Your first edits

Changing my workflow to git seems a tad confusing. In my IDE, I am normally used to seeing that a file is changed without having to do anything special since SVN is integrated and I have a handly little tab which tracks local modifications. In git the equivilant is:

<pre>git status</pre> Diffing files is also something that is a joy in NetBeans. I simply did not enjoy using git to diff my files because I don't like reading my code via the bash shell. I am sure that there are tools to handle this but I really hope that the netbeans git module that is under development gets rolling quickly. For the record, to diff in git, you type:

<pre>git diff</pre> I made a few changes to my index.php file and had to do the following:

<pre>git add . //this adds all the files in the working directory recursively
git commit -m 'my changes for this commit' //make sure to use quotes
git push //this uploaded my changes to github
</pre>

## When to stick with Subversion

For private projects or client work, I think I may still stick to versioning with SVN, which I do currently via [Beanstalk][12]. Why? The most important thing for me is IDE integration in NetBeans. I don't really care for hacking around the command line to commit and diff files one at a time. If someone comes out with an awesome set of Git tools for NetBeans I may be more willing to make the switch. Currently both [GitHub][3] and [Netbeans][13] support either Git or Subversion repo's.

## Summary

The real draw of using git is [GitHub][3]. This site is just great for browsing code, tracking changes, and after a while it becomes obvious why everyone is using it. I can't imagine why anyone would host their public project code anywhere else at this point. If you have an open source project and you want to spur participation, Git is the wise choice due to its distributed nature. If the only thing stopping you from using git is the lack of tools, do yourself a favor and learn how to do it with the command line. The tools will be here eventually!

 [1]: http://git-scm.com/
 [2]: http://subversion.tigris.org/
 [3]: http://github.com
 [4]: http://cakephp.lighthouseapp.com/projects/42648/tickets/1350-incorrect-docblock-for-__nestedlist-in-htmlhelper
 [5]: http://cakephp.lighthouseapp.com/projects/42648-cakephp
 [6]: http://mark-story.com/
 [7]: http://help.github.com/
 [8]: http://php.net/
 [9]: https://github.com/cloudhead/less.js
 [10]: https://github.com/
 [11]: http://code.google.com/p/msysgit/
 [12]: http://www.beanstalkapp.com
 [13]: http://netbeans.org/
