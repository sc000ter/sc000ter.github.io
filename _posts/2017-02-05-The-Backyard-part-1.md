---
layout: post
title: The Backyard, Part 1
subtitle: Let's explore The Backyard, shall we?
image: /img/web_small.jpg
share-img: /img/web_small.jpg
tags: [software development, requirements]
---

When you first are approached by a customer that needs some software development done, what do you talk about?  Do you talk about the technology
and "cool tools" you are going to use on their project?  Do you talk about the number of servers needed and the potential storage space needed by
the database?  How about licensing issues?  Does the customer need to be aware or even care about all that?

Of course not.

But, time and again, we as software developers find ourselves listening to customer requirements, or "wants" while thinking "OK, I'll need a RAID array 
for the SQL server and we'll have to come up with a coherent data backup plan.  Not to mention the web servers: Do we build out a farm?  Is it load-balanced?
Where will user session live?"  Yeah, I've been there, done that.

Back when I got started in software development, I made that mistake.  But as I worked on more and more customer projects, I realized that for the most
part, that stuff didn't matter.  If I had a good foundation to build on, I didn't need to concern myself with overly technical details at the start.
Oh sure, if the customer wanted something that was a "cool" feature that would greatly enhance the site, my mind started whirling away like a dervish
thinking about how to accomplish it.  

But, once I moved into thinking that software development was more craft than science, developing started to easier for me.  I would envision what I wanted 
to do and figure out "the how" i.e the science of getting there.  One of the things I love about software development is that there is always more than
one way to get to where you want to go.

The trick is to make it so that you don't need a PhD to maintain or fix it.  Sure, it is cool to build a completely JavaScript site that is dynamically
generated from a conglomeration of XML and XSLT files.  Need to add a page?  Just add this XSLT file.  But who in their right mind is going to call that
scalable, easily maintainable, and in fact, sane?

So, now we have this customer.  We are at the very beginning of engagement on this project.  What is "The Backyard"?

>I own and operate a restaurant called The Backyard.  I wanted to create a place that feels like home.  Indoor traditional seating and outdoor seating.
Patrons can reserve either an indoor table or outdoor table.  But the problem is our reservation system is kinda old and doesn't work very well.
I want to add special events and even give my customers the ability to reserve games in the summer months or fire pits in the cooler months.  My menu needs to rotate but I don't have
a good way of keeping track of what I'm buying and using.  How much am I making on each dish?  We can figure it out manually, but I want to do better.
I want to be able construct menus and instantly see what the profit it, what ingredients are needed, etc.  It would be really nice if we offered an app
to do reservations and pre-order appetizers and drinks.  Is there some way that customers can "check in" when they get here?  If it's busy, customers
can use the app to put themselves in line, do the pre-ordering thing.  I'd love to offer coupons on busy days to people who have to wait for an hour
or more to get seated.  They could instantly use it and maybe, just maybe, that would convince them to stay.  It would also be nice to integrate with Open
Table and other reservation services.  I also need a great system for managing tables that our servers could use.  They could see at a glance how the 
tables they are responsible for are doing.  Maybe have the customer print out a bar-coded "ticket" that stays with them the entire time, tracking their
wait time, food, drinks, and allowing them to pay at the table without a server disappearing with their credit card.  I know this is a lot, but that's where
I want to go.  Can you help me?

Now you know what this project is all about.

Next time, we'll break this apart and start asking more questions to get more detail so we can start figuring out what we need to build here.  Keep in
mind that some of this stuff exists already out there, but I want to build this entire project from the ground up.  Sure, Open Table has their own
API, but I want to build a fictitious one for this project.  I don't want to build my own message bus if we need it.  I'm not that crazy.  But as much
as I can, I want to explore the different pieces of this project and use appropriate technology and tools to make it work.

-- Brian
