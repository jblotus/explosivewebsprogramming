---
title: 'Tutorial: Issue tracking with Lighthouse and Beanstalk/GitHub'
author: jblotus
excerpt: "How many bugs can you keep track of in your head at once? If you are anything like me, that number is somewhere in the single digit range. This is why issue tracking is a very important tool that every programmer should be using now. There are tons of free issue tracking programs like Mantis, Bugzilla, or even web based applications like Lighthouseapp. There is no excuse to put it off any longer if you aren't already doing it. "
layout: post
dsq_thread_id:
  - 311540297
categories:
  - Issue/Bug Tracking
  - Version Control
  - Web Development
  - Programming
tags:
  - beanstalk
  - bug tracker
  - bug tracking
  - issue tracker
  - issue tracking
  - lighthouse
  - ticket bins
  - tickets
  - versioning
---
How many bugs can you keep track of in your head at once? If you are anything like me, that number is somewhere in the single digit range. This is why issue tracking is a very important tool that every programmer should be using now. There are tons of free issue tracking programs like [Mantis][1], [Bugzilla][2], or even web based applications like [Lighthouse][3]. There is no excuse to put it off any longer if you aren't already doing it. The reason it is important to use issue tracking on every project is that you will be more likely to produce a better product by fixing bugs before you add new features. Tracking your issues also allows your clients to see what you are doing on their projects and allows them to have a better understanding of  what their deadline and progress expectations should be.

Frankly I think simplicity is best for issue tracking and [Lighthouse ][4]is the tool I use to handle all of my issue tracking for every project I work on. It is a very simple, powerful and beautiful issue tracker that allows for third party integration via its API. This is great because you can commit files and the changes will be attached to your tickets, like commits from [GitHub ][5]or [Beanstalk][6].

## Sample Project

In this post I will walk though the basics of setting up a simple issue tracker for a fictional new client, "Inmate Penpals, Inc." The company in this example bought their website as a script for $999 and it is simply broken. The company described in their job posting a very long list of features that were broken, or bugs that needed to be fixed. **Sounds great right? ***Why bother setting up an issue tracker?The client already made a list!*

> 90% of clients will not know the full extent of their issues beforehand, so treat any project description as a synopsis of their issues
The first thing that you need to do is convince the client that they need an issue tracker, as it will help keep the project on budget and allow them the opportunity to see you making progress on their projects. It won't cost them a dime to set up a free account over at [Lighthouseapp][3], so that argument should be pretty easy to win!

## Step 1: Set Up a Issue Tracker

<div>
  <img class="img-thumbnail" title="signup" src="http://www.jblotus.com/wp-content/uploads/2010/12/signup-1024x705.jpg" alt="Signup for a Lighthouse Account" width="550" height="378" />

  <p>
    Signup for a Lighthouse Account
  </p>
</div>

First, [sign up for a free lighthouseapp account][7]. You have to choose your project url, which in this case will be [inmatepenpals.lighthouseapp.com][8].

<div>
  <img class="img-thumbnail" title="confirmation" src="http://www.jblotus.com/wp-content/uploads/2010/12/confirmation-1024x313.jpg" alt="Confirm your Lighthouse account" width="550" height="168" />

  <p>
    Confirm your Email address
  </p>
</div>

<div>
  <img class="img-thumbnail" title="dashboard" src="http://www.jblotus.com/wp-content/uploads/2010/12/dashboard-1024x470.jpg" alt="Your empty Lighhouse dashboard" width="550" height="252" />

  <p>
    Your empty project dashboard
  </p>
</div>

Once you have confirmed your email address you can click the link and be taken directly to your project dashboard, which is totally empty right now. Next, click "**Create a New Project**" to set up your first project.

<div>
  <img class="size-large wp-image-122" title="createnewproject" src="http://www.jblotus.com/wp-content/uploads/2010/12/createnewproject2-550x415.jpg" alt="Create a new Lighthouse project" width="550" height="415" />

  <p>
    Create a new Lighthouse project
  </p>
</div>

You have to fill out some details about your project, mainly the project name and description. The important thing on this page are the options for "**Public Project**", "**Private Project**", and "**Open Source Project**". For most client work, you are going to want to choose "**Private Project**" because you don't want to share your troubles with the world, especially considering that sensitive information may be stored on the issue tracker.

<div>
  <img class="size-large wp-image-123" title="Invite users to Project" src="http://www.jblotus.com/wp-content/uploads/2010/12/invite1-550x306.jpg" alt="Invite users to Project" width="550" height="306" />

  <p>
    Invite users to Project
  </p>
</div>

Next you will want to invite your client to the tracker, or have them invite you if they set it up. On this screen you can set read & write permissions for other users or invite people via email. Since I don't want to add another user right now I will move on to the next section, defining milestones.

## Step 2: Define Project Milestones

Milestones are important because they allow you to set a date for certain bugs or features to be fixed or implemented by. For this project I will create two milestones, one for the expected due date of the project, and one for any future features or enhancements that are beyond the scope of the project deadline.  Every ticket represents an issue, and every issue belongs to a single milestone. This way you can easily prioritize the due dates of your tickets and track their progress.

<div>
  <img class="size-large wp-image-124" title="New Projects appear on the right hand side of the screen" src="http://www.jblotus.com/wp-content/uploads/2010/12/projects1-550x276.jpg" alt="New Projects appear on the right hand side of the screen" width="550" height="276" />

  <p>
    New Projects appear on the right hand side of the screenProjects
  </p>
</div>

<div class="mceTemp mceIEcenter">
  <div>
    <img class="size-large wp-image-125" title="milestones" src="http://www.jblotus.com/wp-content/uploads/2010/12/milestones1-550x276.jpg" alt="The milestones tab is located on the top menu" width="550" height="276" />

    <p>
      The milestones tab is located on the top menu
    </p>
  </div>
</div>

<p style="text-align: left;">
  To create a milestone, we first have to make sure we are in the project screen for our new project. From the dashboard, you can see all of the projects listed on the right hand side of the interface. Since we only have one project, it should be pretty easy to spot. Click the project name and you will be taken to the project overview page.From the project overview screen, you will see a menu that says "<strong>Milestones</strong>". Click on this to go to the milestone edit screen.
</p>

<div>
  <img class="size-large wp-image-126" title="newmilestone" src="http://www.jblotus.com/wp-content/uploads/2010/12/newmilestone1-550x276.jpg" alt="Create a New Milestone" width="550" height="276" />

  <p>
    Create a New Milestone
  </p>
</div>

From the "**Milestone**" screen you can view, add, edit or delete your project milestones.  To create a milestone, you simply click the "**Create Milestone**" menu link.

<div>
  <img class="img-thumbnail" title="alphatest" src="http://www.jblotus.com/wp-content/uploads/2010/12/alphatest1-550x477.jpg" alt="Set the milestone title, due date, and description" width="550" height="477" />

  <p>
    Set the milestone title, due date, and description
  </p>
</div>

We want to add **milestones **for our first due date, which I have set to Dec 30, 2010. The focus and goals for this milestone are to fix all of the major bugs. Once your save the milestone, I want to click "**New Milestone**" again to create a second milestone.

<div>
  <img class="img-thumbnail" title="noduedate" src="http://www.jblotus.com/wp-content/uploads/2010/12/noduedate1-550x390.jpg" alt="Set a Future milestone, with no due date" width="550" height="390" />

  <p>
    Set a Future milestone, with no due date
  </p>
</div>

<p style="text-align: left;">
  In most projects, there are features that would be nice to have but really aren't important enough to get done on your current milestone. In order to avoid scope creep, it is important to put these tickets into a separate milestone. You can always move tickets in or out of the milestone, in this case called "<strong>Future</strong>", into a milestone with a due date.
</p>

## Step 3: Working with tickets

Milestone's are all well and good, but "**Tickets**" are the real meat of the issue tracking experience. A ticket can represent anything from a large feature request, a small database bug, or something even as small as a typo. The ticket system in Lighthouseapp is really exceptional. You can add attachments, assign tickets to specific users, and even integrate 3rd party changesets (i.e. commits) to update directly on the tickets! When combined with versioning from GitHub or Beanstalkapp, this is a powerful way to track the code as it changes over time.

The client has submitted a job description, which describes what the client wants accomplished overall. Our job is to parse this out into separate tickets. This might be a bit difficult at first as we come to terms with the scope of the project, but it will git much easier to refine after we enter everything in.

> Our site is completely malfunctioned. The email doesn't work when the user sends a note to an inmate. There is a display bug on the homepage when you hover your mouse over the "contact us" 3d button that causes the button to go away. Our server seems really slow and I want all of this fixed.  On the homepage only 10 inmates pictures are supposed to show but instead the whole database of prisoners loads. We would also like to have a rotating slideshow that makes the entire page change colors during the holidays. Thanks! -InmatePenpals CEO
Wow, that wasn't the worst job post I ever saw, but it certainly needs to be broken down a bit further. We are going to start creating "**tickets**" for each issue.

<div>
  <img class="img-thumbnail" title="newticket" src="http://www.jblotus.com/wp-content/uploads/2010/12/newticket1-550x307.jpg" alt="The new ticket button is available almost anywhere in the project overview" width="550" height="307" />

  <p>
    The new ticket button is available almost anywhere in the project overview
  </p>
</div>

Make sure that you are somewhere in the "**Project Overview**" section of the tracker. From almost anywhere in here you can click the "**Create a New Ticket**" button.

I usually start out making the tickets rather quickly. I will simply copy and paste the problem in the clients own words to describe the ticket.

<div>
  <img class="img-thumbnail" title="titledescription" src="http://www.jblotus.com/wp-content/uploads/2010/12/titledescription1-550x428.jpg" alt="Enter the ticket title and description" width="550" height="428" />

  <p>
    Enter the ticket title and description
  </p>
</div>

Enter a descriptive title for the ticket. I try an use my best judgement to sum up in my own words the issue at-hand. For description I will usually paste the words of the client and any other info the client has given about the problem. In some cases I may also attach a screenshot of the problem if it is visually demonstrable.

<div>
  <img class="img-thumbnail" title="whofixmilestone" src="http://www.jblotus.com/wp-content/uploads/2010/12/whofixmilestone1-550x428.jpg" alt="Assign the ticket to a user and milestone" width="550" height="428" />

  <p>
    Assign the ticket to a user and milestone
  </p>
</div>

<p style="text-align: left;">
  You should also set the ticket to be assigned to yourself, since you will be fixing it. As for milestone, that really depends upon the request the client has made. If it is something from the original job description, I would probably consider that to be part of the project due date, in our case the Alpha Test milestone. On the other hand, if it were a feature request that the client put in two weeks after the project starts, I would probably put that into <strong>Future</strong>.
</p>

<div>
  <img class="img-thumbnail" title="tags" src="http://www.jblotus.com/wp-content/uploads/2010/12/tags1-550x428.jpg" alt="Set your tags consistently" width="550" height="428" /><>

  <p>
    Set your tags consistently
  </p>
</div>

<p style="text-align: left;">
  Each ticket also has a line for "<strong>Tags</strong>". Tags are something that need to be fairly consistent throughout your project and I will tell you why when we create ticket bins to categorize our tickets. Two tags that are very important are "<strong>defect</strong>" and "<strong>enhancement</strong>". I tend to categorize every ticket into one of those two categories. Most bugs are defects and most features that don't exist or need to be improved are called <strong>enhancements</strong>. Since the ticket in question relates to email not working, we can assume the it is a "<strong>defect</strong>". We can always change this later to "<strong>enhancement</strong>" if it turns out that email functionality was never implemented in the first place.  I also added the tag <strong>"email</strong>", but I don't want to go tag crazy so two tags should be fine in most cases.
</p>

<div>
  <img class="img-thumbnail" title="ticketview" src="http://www.jblotus.com/wp-content/uploads/2010/12/ticketview1-550x392.jpg" alt="The ticket view" width="550" height="392" />

  <p>
    The ticket view
  </p>
</div>

<div>
  <img class="img-thumbnail" title="tickets" src="http://www.jblotus.com/wp-content/uploads/2010/12/tickets1-550x309.jpg" alt="The tickets index" width="550" height="309" />

  <p>
    The tickets index
  </p>
</div>

<p style="text-align: left;">
  I went ahead and created the other tickets while you were reading the previous section. Make sure to tag your bugs as <strong>defect </strong>and your feature requests as <strong>enhancement. </strong>Make sure to go eover each ticket with your client to make sure you are both on the same page, and also to further clarify the ticket. Posting an update to a ticket is a simple as adding a comment.  You can change the ticket title, upload screenshots, change tags, or change anything else about the ticket from the "<strong>Ticket Detail</strong>" screen. Once you have entered all the initial tickets, you can start organizing them into "<strong>Ticket Bins</strong>", which is a fancy way of saying a group of tickets.
</p>

## Step 4: Grouping and sorting tickets with Ticket Bins

<p style="text-align: left;">
  I like to use Ticket Bins to group my tickets into separate categories. I make one ticket bind for <strong>defects</strong>, and another one for <strong>enhancements</strong>. Lighthouseapp already provides some ticket bins by default, under "<strong>Shared Ticket Bins</strong>" on the right hand side of the screen. These are "<strong>Open Tickets</strong>", "<strong>Resolved Tickets</strong>", and "<strong>This week's tickets</strong>". Each ticket bin has a number next to it showing how many tickets are in that bin. A ticket can be in an unlimited number of bins.
</p>

<p style="text-align: left;">
  I always create two new ticket bins with every new project. These bins will be called "<strong>Open Defects</strong>", and "<strong>Open Enhancements</strong>".
</p>

<div>
  <img class="img-thumbnail" title="ticketsscreen" src="http://www.jblotus.com/wp-content/uploads/2010/12/ticketsscreen1-550x309.jpg" alt="Click the &quot;Tickets&quot; menu link" width="550" height="309" />

  <p>
    Click the "Tickets" menu link
  </p>
</div>

<p style="text-align: left;">
  To create a custom search bin, aka ticket bin, you must first be on the main tickets index screen. To get here quickly you can click the "<strong>Tickets</strong>" menu item.
</p>

<div>
  <img class="img-thumbnail" title="findtickets" src="http://www.jblotus.com/wp-content/uploads/2010/12/findtickets1-550x309.jpg" alt="We have to enter our criteria under &quot;Find Tickets&quot;" width="550" height="309" />

  <p>
    We have to enter our criteria under "Find Tickets"
  </p>
</div>

We have to set the search criteria for our ticket bins. In fact we can save any search as a ticket bin. [Click here for a list of all searchable attributes][9].

<div>
  <img class="img-thumbnail" title="taggeddefect" src="http://www.jblotus.com/wp-content/uploads/2010/12/taggeddefect1-550x190.jpg" alt="Add in tagged:defecttagged" width="550" height="190" />

  <p>
    Add in tagged:defecttagged
  </p>
</div>

To get a list of all open defect tickets that I am responsible for, I need to enter the following onto the "**Find Tickets**" line.

```
responsible:me state:open tagged:defect
```

<div>
  <img class="img-thumbnail" title="onlydefects" src="http://www.jblotus.com/wp-content/uploads/2010/12/onlydefects1-550x361.jpg" alt="The ticket search shows only tickets tagged with defect" width="550" height="361" />

  <p>
    The ticket search shows only tickets tagged with defect
  </p>
</div>

Now only tickets that I previously tagged as "**defect**" will appear in the list. To save the bin you must click on "**Save as Search Bin**".

Give the search bin a name "**Open Defects**". I also repeated these steps for our "**Open Enhancements**" ticket bin.

<div>
  <img class="img-thumbnail" title="newbins" src="http://www.jblotus.com/wp-content/uploads/2010/12/newbins1-550x443.jpg" alt="New bins will now appear on the right hand side of the screen" width="550" height="443" />

  <p>
    New bins will now appear on the right hand side of the screen
  </p>
</div>

The new search bins will now appear in the list of "**Shared Ticket Bins**". This is a very convenient way to organize your tickets by almost any criteria you can think of!

## Step 5: Integrate Version Control From GitHub or Beanstalkapp

The best part of all of this is the fact that you can integrate your commit messages from GitHub, Beanstalkapp, or several other providers. It just so happens that I set up a project for this over at [Beanstalkapp][6], but setting it up via [GitHub ][5]is quite similar.

<div class="mceTemp mceIEcenter">
  <div>
    <img class="img-thumbnail" title="apitoken" src="http://www.jblotus.com/wp-content/uploads/2010/12/apitoken1-550x443.jpg" alt="You create an API token for your username" width="550" height="443" />

    <p>
      You create an API token for your username
    </p>
  </div>
</div>

To create an "**API token**", you must click on your username, and you will see on the right hand side of the screen a select list. Choose the current account to create an API token for this specific tracker account, or select "**All Accounts**" to generate an API token that can be used on all tracker accounts associated with your the logged in user.

<div>
  <img class="img-thumbnail" title="currentproject" src="http://www.jblotus.com/wp-content/uploads/2010/12/currentproject1-550x443.jpg" alt="Set API key permissions" width="550" height="443" />

  <p>
    Set API key permissions
  </p>
</div>

For the purposes of our example, I chose to create an "**API Token**" for the current account, labeled Beanstalkapp since that is who I am giving the key to. I also chose to gave the key "**Full Access**" instead of "**Read only access**".

<div>
  <img class="img-thumbnail" title="apitoken" src="http://www.jblotus.com/wp-content/uploads/2010/12/apitoken2-550x443.jpg" alt="Your new API token will appear on the right hand side" width="550" height="443" />

  <p>
    Your new API token will appear on the right hand side
  </p>
</div>

Once in Beanstalk or GitHub, you will need to connect your repository to Lighthouse. Here is a[ complete screencast for integrating Lighthouse with GitHub][12].

<div>
  <img class="img-thumbnail" title="integration" src="http://www.jblotus.com/wp-content/uploads/2010/12/integration1-550x304.jpg" alt="Integration in Beanstalkapp" width="550" height="304" />

  <p>
    Integration in Beanstalkapp
  </p>
</div>

<div>
  <img class="img-thumbnail" title="activate" src="http://www.jblotus.com/wp-content/uploads/2010/12/activate1-550x304.jpg" alt="Activate Lighthouse Integration" width="550" height="304" />

  <p>
    Activate Lighthouse Integration
  </p>
</div>

<div>
  <img class="img-thumbnail" title="accountdetails" src="http://www.jblotus.com/wp-content/uploads/2010/12/accountdetails1-550x394.jpg" alt="Add Lighthouse Tracker URL" width="550" height="394" />

  <p>
    Add Lighthouse Tracker URL
  </p>
</div>

<div>
  <img class="img-thumbnail" title="apitokens" src="http://www.jblotus.com/wp-content/uploads/2010/12/apitokens1-550x394.jpg" alt="Paste your API token into the box" width="550" height="394" />

  <p>
    Paste your API token into the box
  </p>
</div>

<div>
  <img class="img-thumbnail" title="selectproject" src="http://www.jblotus.com/wp-content/uploads/2010/12/selectproject1-550x506.jpg" alt="Select the project from the list" width="550" height="506" />

  <p>
    Select the project from the list
  </p>
</div>

<div>
  <img class="img-thumbnail" title="activateconfirmation" src="http://www.jblotus.com/wp-content/uploads/2010/12/activateconfirmation1-550x506.jpg" alt="Activate the Integration" width="550" height="506" />

  <p>
    Activate the Integration
  </p>
</div>

Once inside your Beanstalk repository dashboard interface, click the "Integration" menu link. You will see a list of services on the left that you can hook into with Beanstalk. Choose "Lighthouse" and click the "Activate" button. All you have to do in the next few forms is paste in your Lighthouse URL, paste in your API, select your active project and you are ready to Activate the integration.

Once we are hooked in via Beanstalk or GitHub, you can add things in your commit messages that will post directly to tickets you create in Lighthouse.

<div>
  <img class="img-thumbnail" title="commitmessage" src="http://www.jblotus.com/wp-content/uploads/2010/12/commitmessage1-550x328.jpg" alt="Make some changes and commit your files" width="550" height="328" />

  <p>
    Make some changes and commit your files
  </p>
</div>

Now when you commit your files, you can add in special tags to tell Beanstalk to push your changesets to Lighthouse.

<div>
  "]<img class="img-thumbnail" title="ticketnumber" src="http://www.jblotus.com/wp-content/uploads/2010/12/ticketnumber1-550x327.jpg" alt="Add the ticket number number in brackets ex [#1]" width="550" height="327" />

  <p>
    Add the ticket number number in brackets ex [#1
  </p>
</div>

To assign a commit to a specific ticket do this:

```
this is my commit message [#1] //#1 is the ticket number
```
To close a ticket do this:

```
this is my commit message [#1 state:resolved] //#1 is the ticket number
```
All of the keywords go inside square brackets []. I usually don't close tickets directly from a commit, and I find that assigning a commit to a specific ticket is usually good enough. You can do some pretty crazy things with Beanstalk such as automatic deployment, just from a simple commit message.

[Here is a complete list of Lighthouse keywords][14].

<div>
  <img class="img-thumbnail" title="newchangeset" src="http://www.jblotus.com/wp-content/uploads/2010/12/newchangeset1-550x216.jpg" alt="New Changesets show up in Lighthouse" width="550" height="216" />

  <p>
    New Changesets show up in Lighthouse
  </p>
</div>

Now if you look at the project overview in Lighthouse, you will see our newest changeset and the associated commit message.

<div>
  <img class="img-thumbnail" title="updatedticket" src="http://www.jblotus.com/wp-content/uploads/2010/12/updatedticket1-550x216.jpg" alt="The changeset and update details as a commentchangeset " width="550" height="216" />

  <p>
    The changeset and update details as a commentchangeset
  </p>
</div>

You will see the newest changeset and commit message applied as a comment to the ticket. You can easily view the changes made by clicking on the corresponding link.

## Step 6: Ticket Management

At this point you might want to mark a ticket as "Resolved" if the issue described has been solved. The different ways you can mark a ticket's status are:

  * **New **- New ticket that has not been worked on. Use this when you first create a ticket.
  * **Open **- A ticket is currently being worked on
  * **Resolved **- A issue has been resolved successfully.
  * **Hold **- Valid issues that can't be resolved or that aren't worth resolving.
  * **Invalid** - Issues that are poorly formed, already working, or generally not bugs or feature requests.

How you use these states will be largely up to you, but try not to use "**Resolved**" in cases where "**Hold**" or "**Invalid**" might be more appropriate, such as when you can't reproduce a bug, or the ticket did not describe a bug but rather incorrect usage.

## Summary

Issue tracking should be part of the flow of every programmer for any serious project. [Lighthouse ][3]is a great choice for the job and they offer a free account, so you have no excuse not to give them a try! So what are you waiting for, get organized today!

 [1]: http://www.mantisbt.org/
 [2]: http://www.bugzilla.org/
 [3]: http://lighthouseapp.com/
 [4]: http://www.fogcreek.com/FogBugz/
 [5]: https://github.com/
 [6]: http://beanstalkapp.com/
 [7]: http://lighthouseapp.com/signup
 [8]: http://inmatepenpals.lighthouseapp.com/
 [9]: http://help.lighthouseapp.com/faqs/getting-started/how-do-i-search-for-tickets
 [12]: http://bigbangtechnology.com/blog/post/setting_up_communication_between_github_and_lighthouse_via_a_secure_token
 [14]: http://help.lighthouseapp.com/faqs/ticket-workflow/ticket-keyword-updates
