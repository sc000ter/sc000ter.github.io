---
layout: post
title: Further user properties
subtitle: Let's define further attributes of our users and where that data will live
image: /img/R2_BB8.jpeg
share-img: /img/R2_BB8.jpeg
tags: [foundation, Identity, API, the what,
---

# Identity, part 2
Now that we've seen the basic attributes of users, let's explore our application-specific attributes of our users.  Where should this data reside?  Should it be in the user store?  Should it live in its own database?

Since this data will most likely include preferences such as cuisine, restaurants, favorite meals, etc., it makes sense to kee the why]p this data in a separate schema or data store.

Now that we have decided to store user preferences in its own place, now comes a little bit of DDD discussion.  I've referred to the concept of "Bounded Context" before, but what does that look like implementation-wise?

We now have two contexts in our application:  Identity and preferences.  Identity should handle authorization and authentication.  Authentication is simply the act of verifying the user is who he says he is.  Verifying a userid/password combination, using 2 factor authentication, etc.  As far as authorization, our app is pretty simple.  We'll use roles.  Users that sign-in though the public facing website or a possible mobile app will be just users.  Users created in our admin piece can have one or more roles assigned to them, including user. So, we will keep authorization logic pretty separated.

But, we shouldn't get ahead of ourselves here.  We are talking about the logical and physical implementation of our contexts.

Let's look at the data.  We have a few choices:
* Separate Databases
* Same database
* Same database but different schema

Personally, I like either the first or last scenarios.  In applications past, everything would live in the same database.  But that was when we had just a website or a stand-alone desktop app utilizing the data.  Keeping all the data in the same database and schema encourages short-cuts and other "domain" services reaching in where they shouldn't and mucking about with the data.

In today's world, we have web sites, mobile apps, APIs, and desktop apps that all need access to the data.  Not only do all these different data pathways open up a wider attack surface for nefarious users, but having all the data in one place isn't as durable and scalable as it might seem.

I'll be the first to say that supporting all this makes our job as architects and developers more difficult.  Now we have to think about how and by what "actor" our data will be accessed.

There are literally about 10 thick books out there on DDD, from Eric Evans' blue book, to Scott Millett and Nick Tune's Wrox press' "Patterns, Principles, and Practices of Domain-driven Design".  There are hundreds if not thousands of blog posts about DDD and/or subsets of its tenants.  Even presentations that mock up an architectual style.  So, again, if you aren't familiar with basic tenents of DDD, I would suggest you do some Googling, get one of the books I mentioned, and dig into the theory and practice.

Now for how I intend to set things up.  I plan on using micro services (again, like I've mentioned before) to implement bounded context on the "back end" of this entire project.

We'll setup the following:
* Separate databases
* Separate Web API projects
* Separate projects for the user-facing website, the restaurant management website, and any other "back end" websites we might need.
* Separate project for the mobile app

We'll look at either versioning our database or using Entity Framework migrations to keep track of changes and rollbacks.

I will attempt to make releases in the Github repo for the entire solution.  That way, you can download a snapshot of the project at different times, fork the repo, clone it, or whatever you would like to do with it.

As with anything, I'm sure it won't be perfect and it wouldn't surprise me if I had to blow away the repo and start over.  But, I'll document all that nonesense here. :)

Until next time!!

--Brian
