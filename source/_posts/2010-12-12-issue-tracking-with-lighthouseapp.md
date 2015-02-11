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

In this post I will walk though the basics of setting up a simple issue tracker for a fictional new client, &#8220;Inmate Penpals, Inc.&#8221; The company in this example bought their website as a script for $999 and it is simply broken. The company described in their job posting a very long list of features that were broken, or bugs that needed to be fixed. **Sounds great right? ***Why bother setting up an issue tracker?The client already made a list!*

> 90% of clients will not know the full extent of their issues beforehand, so treat any project description as a synopsis of their issues
The first thing that you need to do is convince the client that they need an issue tracker, as it will help keep the project on budget and allow them the opportunity to see you making progress on their projects. It won't cost them a dime to set up a free account over at [Lighthouseapp][3], so that argument should be pretty easy to win!

## Step 1: Set Up a Issue Tracker

<div id="attachment_53" style="width: 560px" class="wp-caption aligncenter">
  <a href="http://www.jblotus.com/wp-content/uploads/2010/12/signup.jpg"></a><a rel="attachment wp-att-53" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/signup/"><img class="aligncenter size-large wp-image-53" title="signup" src="http://www.jblotus.com/wp-content/uploads/2010/12/signup-1024x705.jpg" alt="Signup for a Lighthouse Account" width="550" height="378" /></a>

  <p class="wp-caption-text">
    Signup for a Lighthouse Account
  </p>
</div>

First, [sign up for a free lighthouseapp account][7]. You have to choose your project url, which in this case will be [inmatepenpals.lighthouseapp.com][8].

<div id="attachment_55" style="width: 560px" class="wp-caption aligncenter">
  <a href="http://www.jblotus.com/wp-content/uploads/2010/12/confirmation.jpg"></a><a rel="attachment wp-att-55" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/confirmation/"><img class="aligncenter size-large wp-image-55" title="confirmation" src="http://www.jblotus.com/wp-content/uploads/2010/12/confirmation-1024x313.jpg" alt="Confirm your Lighthouse account" width="550" height="168" /></a>

  <p class="wp-caption-text">
    Confirm your Email address
  </p>
</div>

<div id="attachment_56" style="width: 560px" class="wp-caption aligncenter">
  <a href="http://www.jblotus.com/wp-content/uploads/2010/12/dashboard.jpg"></a><a rel="attachment wp-att-56" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/dashboard/"><img class="aligncenter size-large wp-image-56" title="dashboard" src="http://www.jblotus.com/wp-content/uploads/2010/12/dashboard-1024x470.jpg" alt="Your empty Lighhouse dashboard" width="550" height="252" /></a>

  <p class="wp-caption-text">
    Your empty project dashboard
  </p>
</div>

Once you have confirmed your email address you can click the link and be taken directly to your project dashboard, which is totally empty right now. Next, click &#8220;**Create a New Project**&#8221; to set up your first project.

<div id="attachment_122" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-122" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/createnewproject-3/"><img class="size-large wp-image-122" title="createnewproject" src="http://www.jblotus.com/wp-content/uploads/2010/12/createnewproject2-550x415.jpg" alt="Create a new Lighthouse project" width="550" height="415" /></a>

  <p class="wp-caption-text">
    Create a new Lighthouse project
  </p>
</div>

You have to fill out some details about your project, mainly the project name and description. The important thing on this page are the options for &#8220;**Public Project**&#8220;, &#8220;**Private Project**&#8220;, and &#8220;**Open Source Project**&#8220;. For most client work, you are going to want to choose &#8220;**Private Project**&#8221; because you don't want to share your troubles with the world, especially considering that sensitive information may be stored on the issue tracker.

<div id="attachment_123" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-123" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/invite-2/"><img class="size-large wp-image-123" title="Invite users to Project" src="http://www.jblotus.com/wp-content/uploads/2010/12/invite1-550x306.jpg" alt="Invite users to Project" width="550" height="306" /></a>

  <p class="wp-caption-text">
    Invite users to Project
  </p>
</div>

Next you will want to invite your client to the tracker, or have them invite you if they set it up. On this screen you can set read & write permissions for other users or invite people via email. Since I don't want to add another user right now I will move on to the next section, defining milestones.

## Step 2: Define Project Milestones

Milestones are important because they allow you to set a date for certain bugs or features to be fixed or implemented by. For this project I will create two milestones, one for the expected due date of the project, and one for any future features or enhancements that are beyond the scope of the project deadline.  Every ticket represents an issue, and every issue belongs to a single milestone. This way you can easily prioritize the due dates of your tickets and track their progress.

<div id="attachment_124" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-124" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/projects-2/"><img class="size-large wp-image-124" title="New Projects appear on the right hand side of the screen" src="http://www.jblotus.com/wp-content/uploads/2010/12/projects1-550x276.jpg" alt="New Projects appear on the right hand side of the screen" width="550" height="276" /></a>

  <p class="wp-caption-text">
    New Projects appear on the right hand side of the screenProjects
  </p>
</div>

<div class="mceTemp mceIEcenter">
  <div id="attachment_125" style="width: 560px" class="wp-caption aligncenter">
    <a rel="attachment wp-att-125" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/milestones-2/"><img class="size-large wp-image-125" title="milestones" src="http://www.jblotus.com/wp-content/uploads/2010/12/milestones1-550x276.jpg" alt="The milestones tab is located on the top menu" width="550" height="276" /></a>

    <p class="wp-caption-text">
      The milestones tab is located on the top menu
    </p>
  </div>
</div>

<p style="text-align: left;">
  To create a milestone, we first have to make sure we are in the project screen for our new project. From the dashboard, you can see all of the projects listed on the right hand side of the interface. Since we only have one project, it should be pretty easy to spot. Click the project name and you will be taken to the project overview page.From the project overview screen, you will see a menu that says &#8220;<strong>Milestones</strong>&#8220;. Click on this to go to the milestone edit screen.
</p>

<div id="attachment_126" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-126" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/newmilestone-2/"><img class="size-large wp-image-126" title="newmilestone" src="http://www.jblotus.com/wp-content/uploads/2010/12/newmilestone1-550x276.jpg" alt="Create a New Milestone" width="550" height="276" /></a>

  <p class="wp-caption-text">
    Create a New Milestone
  </p>
</div>

From the &#8220;**Milestone**&#8221; screen you can view, add, edit or delete your project milestones.  To create a milestone, you simply click the &#8220;**Create Milestone**&#8221; menu link.

<div id="attachment_127" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-127" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/alphatest-2/"><img class="size-large wp-image-127" title="alphatest" src="http://www.jblotus.com/wp-content/uploads/2010/12/alphatest1-550x477.jpg" alt="Set the milestone title, due date, and description" width="550" height="477" /></a>

  <p class="wp-caption-text">
    Set the milestone title, due date, and description
  </p>
</div>

We want to add **milestones **for our first due date, which I have set to Dec 30, 2010. The focus and goals for this milestone are to fix all of the major bugs. Once your save the milestone, I want to click &#8220;**New Milestone**&#8221; again to create a second milestone.

<div id="attachment_128" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-128" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/noduedate-2/"><img class="size-large wp-image-128" title="noduedate" src="http://www.jblotus.com/wp-content/uploads/2010/12/noduedate1-550x390.jpg" alt="Set a Future milestone, with no due date" width="550" height="390" /></a>

  <p class="wp-caption-text">
    Set a Future milestone, with no due date
  </p>
</div>

<p style="text-align: left;">
  In most projects, there are features that would be nice to have but really aren't important enough to get done on your current milestone. In order to avoid scope creep, it is important to put these tickets into a separate milestone. You can always move tickets in or out of the milestone, in this case called &#8220;<strong>Future</strong>&#8220;, into a milestone with a due date.
</p>

## Step 3: Working with tickets

Milestone's are all well and good, but &#8220;**Tickets**&#8221; are the real meat of the issue tracking experience. A ticket can represent anything from a large feature request, a small database bug, or something even as small as a typo. The ticket system in Lighthouseapp is really exceptional. You can add attachments, assign tickets to specific users, and even integrate 3rd party changesets (i.e. commits) to update directly on the tickets! When combined with versioning from GitHub or Beanstalkapp, this is a powerful way to track the code as it changes over time.

The client has submitted a job description, which describes what the client wants accomplished overall. Our job is to parse this out into separate tickets. This might be a bit difficult at first as we come to terms with the scope of the project, but it will git much easier to refine after we enter everything in.

> Our site is completely malfunctioned. The email doesn't work when the user sends a note to an inmate. There is a display bug on the homepage when you hover your mouse over the &#8220;contact us&#8221; 3d button that causes the button to go away. Our server seems really slow and I want all of this fixed.  On the homepage only 10 inmates pictures are supposed to show but instead the whole database of prisoners loads. We would also like to have a rotating slideshow that makes the entire page change colors during the holidays. Thanks! -InmatePenpals CEO
Wow, that wasn't the worst job post I ever saw, but it certainly needs to be broken down a bit further. We are going to start creating &#8220;**tickets**&#8221; for each issue.

<div id="attachment_129" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-129" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/newticket-2/"><img class="size-large wp-image-129" title="newticket" src="http://www.jblotus.com/wp-content/uploads/2010/12/newticket1-550x307.jpg" alt="The new ticket button is available almost anywhere in the project overview" width="550" height="307" /></a>

  <p class="wp-caption-text">
    The new ticket button is available almost anywhere in the project overview
  </p>
</div>

Make sure that you are somewhere in the &#8220;**Project Overview**&#8221; section of the tracker. From almost anywhere in here you can click the &#8220;**Create a New Ticket**&#8221; button.

I usually start out making the tickets rather quickly. I will simply copy and paste the problem in the clients own words to describe the ticket.

<div id="attachment_130" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-130" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/titledescription-2/"><img class="size-large wp-image-130" title="titledescription" src="http://www.jblotus.com/wp-content/uploads/2010/12/titledescription1-550x428.jpg" alt="Enter the ticket title and description" width="550" height="428" /></a>

  <p class="wp-caption-text">
    Enter the ticket title and description
  </p>
</div>

Enter a descriptive title for the ticket. I try an use my best judgement to sum up in my own words the issue at-hand. For description I will usually paste the words of the client and any other info the client has given about the problem. In some cases I may also attach a screenshot of the problem if it is visually demonstrable.

<div id="attachment_131" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-131" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/whofixmilestone-2/"><img class="size-large wp-image-131" title="whofixmilestone" src="http://www.jblotus.com/wp-content/uploads/2010/12/whofixmilestone1-550x428.jpg" alt="Assign the ticket to a user and milestone" width="550" height="428" /></a>

  <p class="wp-caption-text">
    Assign the ticket to a user and milestone
  </p>
</div>

<p style="text-align: left;">
  You should also set the ticket to be assigned to yourself, since you will be fixing it. As for milestone, that really depends upon the request the client has made. If it is something from the original job description, I would probably consider that to be part of the project due date, in our case the Alpha Test milestone. On the other hand, if it were a feature request that the client put in two weeks after the project starts, I would probably put that into <strong>Future</strong>.
</p>

<div id="attachment_132" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-132" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/tags-2/"><img class="size-large wp-image-132" title="tags" src="http://www.jblotus.com/wp-content/uploads/2010/12/tags1-550x428.jpg" alt="Set your tags consistently" width="550" height="428" /></a>

  <p class="wp-caption-text">
    Set your tags consistently
  </p>
</div>

<p style="text-align: left;">
  Each ticket also has a line for &#8220;<strong>Tags</strong>&#8220;. Tags are something that need to be fairly consistent throughout your project and I will tell you why when we create ticket bins to categorize our tickets. Two tags that are very important are &#8220;<strong>defect</strong>&#8221; and &#8220;<strong>enhancement</strong>&#8220;. I tend to categorize every ticket into one of those two categories. Most bugs are defects and most features that don't exist or need to be improved are called <strong>enhancements</strong>. Since the ticket in question relates to email not working, we can assume the it is a &#8220;<strong>defect</strong>&#8220;. We can always change this later to &#8220;<strong>enhancement</strong>&#8221; if it turns out that email functionality was never implemented in the first place.  I also added the tag <strong>&#8220;email</strong>&#8220;, but I don't want to go tag crazy so two tags should be fine in most cases.
</p>

<div id="attachment_133" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-133" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/ticketview-2/"><img class="size-large wp-image-133" title="ticketview" src="http://www.jblotus.com/wp-content/uploads/2010/12/ticketview1-550x392.jpg" alt="The ticket view" width="550" height="392" /></a>

  <p class="wp-caption-text">
    The ticket view
  </p>
</div>

<div id="attachment_134" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-134" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/tickets-2/"><img class="size-large wp-image-134" title="tickets" src="http://www.jblotus.com/wp-content/uploads/2010/12/tickets1-550x309.jpg" alt="The tickets index" width="550" height="309" /></a>

  <p class="wp-caption-text">
    The tickets index
  </p>
</div>

<p style="text-align: left;">
  I went ahead and created the other tickets while you were reading the previous section. Make sure to tag your bugs as <strong>defect </strong>and your feature requests as <strong>enhancement. </strong>Make sure to go eover each ticket with your client to make sure you are both on the same page, and also to further clarify the ticket. Posting an update to a ticket is a simple as adding a comment.  You can change the ticket title, upload screenshots, change tags, or change anything else about the ticket from the &#8220;<strong>Ticket Detail</strong>&#8221; screen. Once you have entered all the initial tickets, you can start organizing them into &#8220;<strong>Ticket Bins</strong>&#8220;, which is a fancy way of saying a group of tickets.
</p>

## Step 4: Grouping and sorting tickets with Ticket Bins

<p style="text-align: left;">
  I like to use Ticket Bins to group my tickets into separate categories. I make one ticket bind for <strong>defects</strong>, and another one for <strong>enhancements</strong>. Lighthouseapp already provides some ticket bins by default, under &#8220;<strong>Shared Ticket Bins</strong>&#8221; on the right hand side of the screen. These are &#8220;<strong>Open Tickets</strong>&#8220;, &#8220;<strong>Resolved Tickets</strong>&#8220;, and &#8220;<strong>This week's tickets</strong>&#8220;. Each ticket bin has a number next to it showing how many tickets are in that bin. A ticket can be in an unlimited number of bins.
</p>

<p style="text-align: left;">
  I always create two new ticket bins with every new project. These bins will be called &#8220;<strong>Open Defects</strong>&#8220;, and &#8220;<strong>Open Enhancements</strong>&#8220;.
</p>

<div id="attachment_135" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-135" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/ticketsscreen-2/"><img class="size-large wp-image-135" title="ticketsscreen" src="http://www.jblotus.com/wp-content/uploads/2010/12/ticketsscreen1-550x309.jpg" alt="Click the &quot;Tickets&quot; menu link" width="550" height="309" /></a>

  <p class="wp-caption-text">
    Click the "Tickets" menu link
  </p>
</div>

<p style="text-align: left;">
  To create a custom search bin, aka ticket bin, you must first be on the main tickets index screen. To get here quickly you can click the &#8220;<strong>Tickets</strong>&#8221; menu item.
</p>

<div id="attachment_136" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-136" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/findtickets-2/"><img class="size-large wp-image-136" title="findtickets" src="http://www.jblotus.com/wp-content/uploads/2010/12/findtickets1-550x309.jpg" alt="We have to enter our criteria under &quot;Find Tickets&quot;" width="550" height="309" /></a>

  <p class="wp-caption-text">
    We have to enter our criteria under "Find Tickets"
  </p>
</div>

We have to set the search criteria for our ticket bins. In fact we can save any search as a ticket bin. [Click here for a list of all searchable attributes][9].

<div id="attachment_137" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-137" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/taggeddefect-2/"><img class="size-large wp-image-137" title="taggeddefect" src="http://www.jblotus.com/wp-content/uploads/2010/12/taggeddefect1-550x190.jpg" alt="Add in tagged:defecttagged" width="550" height="190" /></a>

  <p class="wp-caption-text">
    Add in tagged:defecttagged
  </p>
</div>

[ ][10]To get a list of all open defect tickets that I am responsible for, I need to enter the following onto the &#8220;**Find Tickets**&#8221; line.

<pre lang="bash">responsible:me state:open tagged:defect</pre>

<div id="attachment_138" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-138" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/onlydefects-2/"><img class="size-large wp-image-138" title="onlydefects" src="http://www.jblotus.com/wp-content/uploads/2010/12/onlydefects1-550x361.jpg" alt="The ticket search shows only tickets tagged with defect" width="550" height="361" /></a>

  <p class="wp-caption-text">
    The ticket search shows only tickets tagged with defect
  </p>
</div>

Now only tickets that I previously tagged as &#8220;**defect**&#8221; will appear in the list. To save the bin you must click on &#8220;**Save as Search Bin**&#8220;.

[ ][11]Give the search bin a name &#8220;**Open Defects**&#8220;. I also repeated these steps for our &#8220;**Open Enhancements**&#8221; ticket bin.

<div id="attachment_142" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-142" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/newbins-2/"><img class="size-large wp-image-142" title="newbins" src="http://www.jblotus.com/wp-content/uploads/2010/12/newbins1-550x443.jpg" alt="New bins will now appear on the right hand side of the screen" width="550" height="443" /></a>

  <p class="wp-caption-text">
    New bins will now appear on the right hand side of the screen
  </p>
</div>

The new search bins will now appear in the list of &#8220;**Shared Ticket Bins**&#8220;. This is a very convenient way to organize your tickets by almost any criteria you can think of!

## Step 5: Integrate Version Control From GitHub or Beanstalkapp

The best part of all of this is the fact that you can integrate your commit messages from GitHub, Beanstalkapp, or several other providers. It just so happens that I set up a project for this over at [Beanstalkapp][6], but setting it up via [GitHub ][5]is quite similar.

<div class="mceTemp mceIEcenter">
  <div id="attachment_144" style="width: 560px" class="wp-caption aligncenter">
    <a rel="attachment wp-att-144" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/apitoken-2/"><img class="size-large wp-image-144" title="apitoken" src="http://www.jblotus.com/wp-content/uploads/2010/12/apitoken1-550x443.jpg" alt="You create an API token for your username" width="550" height="443" /></a>

    <p class="wp-caption-text">
      You create an API token for your username
    </p>
  </div>
</div>

To create an &#8220;**API token**&#8220;, you must click on your username, and you will see on the right hand side of the screen a select list. Choose the current account to create an API token for this specific tracker account, or select &#8220;**All Accounts**&#8221; to generate an API token that can be used on all tracker accounts associated with your the logged in user.

<div id="attachment_145" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-145" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/currentproject-2/"><img class="size-large wp-image-145" title="currentproject" src="http://www.jblotus.com/wp-content/uploads/2010/12/currentproject1-550x443.jpg" alt="Set API key permissions" width="550" height="443" /></a>

  <p class="wp-caption-text">
    Set API key permissions
  </p>
</div>

For the purposes of our example, I chose to create an &#8220;**API Token**&#8221; for the current account, labeled Beanstalkapp since that is who I am giving the key to. I also chose to gave the key &#8220;**Full Access**&#8221; instead of &#8220;**Read only access**&#8220;.

<div id="attachment_146" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-146" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/apitoken-3/"><img class="size-large wp-image-146" title="apitoken" src="http://www.jblotus.com/wp-content/uploads/2010/12/apitoken2-550x443.jpg" alt="Your new API token will appear on the right hand side" width="550" height="443" /></a>

  <p class="wp-caption-text">
    Your new API token will appear on the right hand side
  </p>
</div>

Once in Beanstalk or GitHub, you will need to connect your repository to Lighthouse. Here is a[ complete screencast for integrating Lighthouse with GitHub][12].

<div id="attachment_147" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-147" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/integration-2/"><img class="size-large wp-image-147" title="integration" src="http://www.jblotus.com/wp-content/uploads/2010/12/integration1-550x304.jpg" alt="Integration in Beanstalkapp" width="550" height="304" /></a>

  <p class="wp-caption-text">
    Integration in Beanstalkapp
  </p>
</div>

<div id="attachment_148" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-148" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/activate-2/"><img class="size-large wp-image-148" title="activate" src="http://www.jblotus.com/wp-content/uploads/2010/12/activate1-550x304.jpg" alt="Activate Lighthouse Integration" width="550" height="304" /></a>

  <p class="wp-caption-text">
    Activate Lighthouse Integration
  </p>
</div>

<div id="attachment_149" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-149" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/accountdetails-2/"><img class="size-large wp-image-149" title="accountdetails" src="http://www.jblotus.com/wp-content/uploads/2010/12/accountdetails1-550x394.jpg" alt="Add Lighthouse Tracker URL" width="550" height="394" /></a>

  <p class="wp-caption-text">
    Add Lighthouse Tracker URL
  </p>
</div>

<div id="attachment_150" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-150" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/apitokens-2/"><img class="size-large wp-image-150" title="apitokens" src="http://www.jblotus.com/wp-content/uploads/2010/12/apitokens1-550x394.jpg" alt="Paste your API token into the box" width="550" height="394" /></a>

  <p class="wp-caption-text">
    Paste your API token into the box
  </p>
</div>

<div id="attachment_151" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-151" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/selectproject-2/"><img class="size-large wp-image-151" title="selectproject" src="http://www.jblotus.com/wp-content/uploads/2010/12/selectproject1-550x506.jpg" alt="Select the project from the list" width="550" height="506" /></a>

  <p class="wp-caption-text">
    Select the project from the list
  </p>
</div>

<div id="attachment_152" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-152" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/activateconfirmation-2/"><img class="size-large wp-image-152" title="activateconfirmation" src="http://www.jblotus.com/wp-content/uploads/2010/12/activateconfirmation1-550x506.jpg" alt="Activate the Integration" width="550" height="506" /></a>

  <p class="wp-caption-text">
    Activate the Integration
  </p>
</div>

[ ][13]Once inside your Beanstalk repository dashboard interface, click the &#8220;Integration&#8221; menu link. You will see a list of services on the left that you can hook into with Beanstalk. Choose &#8220;Lighthouse&#8221; and click the &#8220;Activate&#8221; button. All you have to do in the next few forms is paste in your Lighthouse URL, paste in your API, select your active project and you are ready to Activate the integration.

Once we are hooked in via Beanstalk or GitHub, you can add things in your commit messages that will post directly to tickets you create in Lighthouse.

<div id="attachment_153" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-153" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/commitmessage-2/"><img class="size-large wp-image-153" title="commitmessage" src="http://www.jblotus.com/wp-content/uploads/2010/12/commitmessage1-550x328.jpg" alt="Make some changes and commit your files" width="550" height="328" /></a>

  <p class="wp-caption-text">
    Make some changes and commit your files
  </p>
</div>

Now when you commit your files, you can add in special tags to tell Beanstalk to push your changesets to Lighthouse.

<div id="attachment_154" style="width: 560px" class="wp-caption aligncenter">
  &#8220;]<a rel="attachment wp-att-154" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/ticketnumber-2/"><img class="size-large wp-image-154" title="ticketnumber" src="http://www.jblotus.com/wp-content/uploads/2010/12/ticketnumber1-550x327.jpg" alt="Add the ticket number number in brackets ex [#1]" width="550" height="327" /></a>

  <p class="wp-caption-text">
    Add the ticket number number in brackets ex [#1
  </p>
</div>

To assign a commit to a specific ticket do this:

<pre lang="bash">this is my commit message [#1] //#1 is the ticket number</pre> To close a ticket do this:

<pre lang="bash">this is my commit message [#1 state:resolved] //#1 is the ticket number</pre> All of the keywords go inside square brackets []. I usually don't close tickets directly from a commit, and I find that assigning a commit to a specific ticket is usually good enough. You can do some pretty crazy things with Beanstalk such as automatic deployment, just from a simple commit message.

[Here is a complete list of Lighthouse keywords][14].

<div id="attachment_155" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-155" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/newchangeset-2/"><img class="size-large wp-image-155" title="newchangeset" src="http://www.jblotus.com/wp-content/uploads/2010/12/newchangeset1-550x216.jpg" alt="New Changesets show up in Lighthouse" width="550" height="216" /></a>

  <p class="wp-caption-text">
    New Changesets show up in Lighthouse
  </p>
</div>

Now if you look at the project overview in Lighthouse, you will see our newest changeset and the associated commit message.

<div id="attachment_156" style="width: 560px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-156" href="http://www.jblotus.com/2010/12/12/issue-tracking-with-lighthouseapp/updatedticket-2/"><img class="size-large wp-image-156" title="updatedticket" src="http://www.jblotus.com/wp-content/uploads/2010/12/updatedticket1-550x216.jpg" alt="The changeset and update details as a commentchangeset " width="550" height="216" /></a>

  <p class="wp-caption-text">
    The changeset and update details as a commentchangeset
  </p>
</div>

You will see the newest changeset and commit message applied as a comment to the ticket. You can easily view the changes made by clicking on the corresponding link.

## Step 6: Ticket Management

At this point you might want to mark a ticket as &#8220;Resolved&#8221; if the issue described has been solved. The different ways you can mark a ticket's status are:

  * **New **- New ticket that has not been worked on. Use this when you first create a ticket.
  * **Open **- A ticket is currently being worked on
  * **Resolved **- A issue has been resolved successfully.
  * **Hold **- Valid issues that can't be resolved or that aren't worth resolving.
  * **Invalid** &#8211; Issues that are poorly formed, already working, or generally not bugs or feature requests.

How you use these states will be largely up to you, but try not to use &#8220;**Resolved**&#8221; in cases where &#8220;**Hold**&#8221; or &#8220;**Invalid**&#8221; might be more appropriate, such as when you can't reproduce a bug, or the ticket did not describe a bug but rather incorrect usage.

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
 [10]: http://www.jblotus.com/wp-content/uploads/2010/12/taggeddefect.jpg
 [11]: http://www.jblotus.com/wp-content/uploads/2010/12/savebin.jpg
 [12]: http://bigbangtechnology.com/blog/post/setting_up_communication_between_github_and_lighthouse_via_a_secure_token
 [13]: http://www.jblotus.com/wp-content/uploads/2010/12/activateconfirmation.jpg
 [14]: http://help.lighthouseapp.com/faqs/ticket-workflow/ticket-keyword-updates
