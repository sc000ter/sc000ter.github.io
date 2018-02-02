---
layout: post
title: The Backyard, Part 2
subtitle: Break it down!
image: /img/web_small.jpg
share-img: /img/web_small.jpg
tags: [software development, requirements]
---

Where were we?  I apologize for being away for so long.  Life got in the way and this blog got pushed to the back.

Last time we were talking about a fictitious scenario where we had the owner of a restaurant concept called "The Backyard"  He had a lot of requirements.  Since we are developers here and we want to _develop_, let's begin by outlining the base requirments that are the underpinnings of the entire solution.

There's a common theme running through the owner's comments.  He wants to offer a lot of different reservation options:

>Patrons can reserve either an indoor table or outdoor table

>I want to add special events and even give my customers the ability to reserve games in the summer months or fire pits in the cooler months.  

>It would be really nice if we offered an app to do reservations and pre-order appetizers and drinks. 

The owner wants to improve his establishment's practices of managing the menu, tables, and servers:

>I also need a great system for managing tables that our servers could use. 

>I want to be able construct menus and instantly see what the profit it, what ingredients are needed, etc. 

There's lots more that we can look at but now that we know in a general sense what the owner wants,
let's start to talk about the solution archetecture.  This solution will have lots of moving parts 
and it is probably a good idea now to figure out how we can organize it all.

Do we break out the major parts into their own sub-projects within the solution?  Or should theyu be solutions themselves?  The owner mentioned a website.  I'm sure that he'll want to support all the suff he talked about on that.

So, let's start with a solution that can support APIs, webpages, and other points of extnsibility.

I'm going to spend some time building out an empty solution with what I feel the structure should be and post that for your review next time.

I'll spin up a Git repo that you can start following along with.  I'm not sure if I want to honor pull requests, BUT if you have comments or suggestions on stuff (or want to code something that sounds interesting to you), please feel free.

--Brian
