---
layout: post
title: The beginning of the fork in the road
subtitle: Starting a new project is all about choices.  Let's explore some choices.
image: /img/choices.jpg
share-img: /img/choices.jpg
tags: [requirements, solution architecture]
---
# Let's write come code!
Just kidding.  heh.

We have some direction from the customer.  We've talked about some of the foundations of building software.  Now we need to get into some specifics.

Specifically, what environment are we going to develop in?  What languages are we going to use?  What server and network infrastructure do we need to be successful?

In an ideal world, we'd have an IT department to lean on for the hardware pieces of this solution.  Fortunately for us, we aren't going to need to worry about that for some time.

What *do* we worry about?  Well, the software part, of course!  Since I've been on the MS stack for 20 years plus, and it is what I know, I've made the decision that we are going to use the Microsoft stack of technologies up and down the solution.  We'll augment it with Open source software and / or tools when appropriate.

I've been using Visual Studio since it was first introduced and that is what I typically develop in.  One of the reasons I'm doing this project is to learn, so I will start using Visual Studio Code in conjunction with Visual Studio.  Maybe I'll learn something along the way that you, dear reader, can learn from. :)

At work, we have moved to Git recently.  We haven't migrated all our projects to it yet and I keep going back and forth from Team Foundation Server to Git and my Git skills are to put it bluntly, weak sauce.  However, if you read my second post on this blog, you'll remember I'm hosting this on GitHub Pages.  So, I can use Git but I am by no means a master.  I guess I put that out there to say that if code starts showing up and then things disappear it's most likely due to my poor Git skills. :)

Alright, We have our development environment and version control settled on.  Next, let's spend a small amount of time on some other necessities to make this project work.

SQL Server will be our database.  Again, since that is what I know, I'm going to use it as the main database.  I may work in some NoSQL database action as well.  It depends on the use case.  As we move along in the development cycle, we'll probably need to start thinking about automating our builds.  There are some great solutions out there, from Octopus, Jenkins, and other solutions.  I'll have to research which one would be best.  Again, I'll be learning and sharing what I learn.  Hopefully, in some sort of coherent way.

We're going to need some kind of messaging system so we will need to evaluate some of the currently available products.

We'll need to decide on a UI/UX platform to support web and mobile front-ends after a while.  Do we develop in Angular?  React?  Vue?  Utilize Xamarin to make mobile apps?  We'll get to that as we progress.  I promise.

One thing we will have is users.  From chefs, to wait staff, to customers, we'll have a variety of users that can have access to functionality across all the different applications we are going to build.  So, how do we handle users?

Microsoft has built out an identity management system for their ASP.NET solutions.  We might use some of that for our user store everywhere.

It looks like this will the first bit of our domain we need to figure out.  What do our users look like?  What kind of attributes do they have?  How do we express those attributes?

Next time, I'll have the beginnings of what our users look like.  You wouldn't think an entire article could be dedicated to just users, but you would be suprised.

--Brian