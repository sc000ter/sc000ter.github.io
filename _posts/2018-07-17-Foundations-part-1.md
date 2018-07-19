---
layout: post
title: Foundations, part 1
subtitle: Let's talk about structure
image: /img/Foundation_small.jpg
share-img: /img/Foundation_small.jpg
tags: [software development, requirements]
---

Before we can talk about writing actual code, we need to think about some very important items that will lay the foundation for our success or failure.

_TL:DR I'm not writing code just yet.  This architecture stuff is important_

We've already explored the requirements a little bit and now we can start to get an idea of the systems we will need.

With the widespread availability of the Internet, and being connected in general, it makes sense to talk about that in the terms of software architecture.

The owner of The Backyard threw a lot different systems and "like to haves" at us when describing his dream system.  However, there is one thing that can assure our success now matter what type of system we end up building:  build for connectedness.

We might have an app the runs on a tablet that doesn't have a persistent connection to the Internet.  We have to account for that.  Maybe the "back office" system is a Windows application that only needs to be connected occasionally.  We know there will be a website and mobile applications, which require a persistent connection to the Internet.

When building an application, I like to start off with the data we are going to have.  Traditionally, we would probably start with creating a database and start designing tables.

But an interesting paradigm shift has been happening in recent years that makes sense when you start to think about it: micro services.  What are micro services?  <a href="http://microservices.io/" target="_blank">This page</a> has a ridiculous amount of information.  

Putting it as succinctly as I can, when applications are being developed, they are usually developed in what is now referred to as a "monolithic" application or <a href="http://www.laputan.org/mud/" target="_blank">"Big Ball Of Mud"</a>.  They are developed for expediency's sake and the maintainability, scalability, security, and availability are secondary concerns.  I've been guilty of this a lot and it is a way that you can get something done in an expedient manner.

However, now that systems are getting more complex and it is not usually the case where one developer works entirely on a single project, new ways of designing and implementing application architecture and function have emerged.  Some of the ideas going around have been around a long time and some are relatively recent developments.

Micro services gives us a way to implement small, composable pieces of our application.  They can share a database, but most often shouldn't.  Micro services allow teams of developers to work on feature sets of an application concurrently.

There are as many ways to design and implement micro services as there are developers to develop them.  It is a good idea to decide on what your architecture for micro services will look like, where it will live, and where the separation of concerns will lie.  Will all the services share the same database?  Will they share a library project that has all the data access code in it?  Will they just be separate controllers in one web project?  Will they live behind an API gateway?    

I know that is a lot of questions to ask and you might be thinking "Jeez, just start writing some code already!!"  I told you at the beginning that we are going to have these kinds of discussions before we open up our development environment.

<a href="https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/microservices" target="_blank">Microsoft</a>'s suggestion for a micro service architecture is their Azure product.  As we progress through this project we may move to Azure or Amazon Web Services.  Initially though, we'll keep it local so that following along won't be too much of a pain.

## Benefits of Micro services
The big deal with micro services is that it gives us a way to implement in a concrete way a concept from <a href="https://martinfowler.com/tags/domain%20driven%20design.html" target="_blank">Domain-driven design</a> called <a href="https://martinfowler.com/bliki/BoundedContext.html" target="_blank">bounded contexts</a>.

The best book I've seen written about Domain-driven design is by <a href="http://domainlanguage.com/" target="_blank">Eric Evans</a>.  His book <a href="https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/ref=sr_1_2?ie=UTF8&qid=1531963931&sr=8-2&keywords=domain+driven+design" target="_blank">Domain-driven Design: Tackling Complexity in the heart of Software</a> is probably the most well-known and most read on the subject.  In his book, Eric talks about the entire development process.  From getting the business and the developers to use the same language and terms, to the patterns used to express that language.  There are several core concepts in DDD that we might brush up against, but as this is just you, dear reader, and me (for now) that will be moving through this software project, our ubiquitous language will be whatever is written on this page.

That's not to say if I get enough comments that something won't change.  But I am intentionally cutting out some complexity because this project will be complex enough!

So, where were we?  Ah yes.  Micro services.  We aren't going to explore our implementation of them just yet.  Again, there are as many different views on this subject as there are people that care about it.  

So, do some homework for me.  Get read in on micros services and bounded contexts.  Here are a couple great places to start:
* Jimmy Bogard has a great <a href="https://jimmybogard.com/tag/microservices/" target="_blank">series</a> on micro services (should it be one word?  two?) and I kind of like his views and implementation details.  
* There is even implementation details from <a href="https://docs.microsoft.com/en-us/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/net-core-microservice-domain-model" target="_blank">The Source</a>

-Brian
