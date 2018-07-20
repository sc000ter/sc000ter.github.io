---
layout: post
title: Foundations, part 2
subtitle: Landscapes, clouds, and the ocean of possibilites
image: /img/Foundation_small.jpg
share-img: /img/Foundation_small.jpg
tags: [software development, requirements]
---

I realize that my discussion of bounded contexts was a little fast and loose.  Maybe we should stay on that concept for a bit.

When we talk about the problem our software application(s) are going to solve, we are really discussing the "domain" of a problem.  It could be like this project, a restaurant management system, EDI file processing, billing for drug tests, or any number of line-of-business problems that need solutions.

At the heart of any domain is a "model".  The model can express transport, a menu, a hotel reservation, or whatever it is that is the "heart" of what you are building for. 

These concepts are ancillary to a bounded context.  A bounded context enables us to segregate responsibility for parts of the domain.  For instance, maybe we need to manage all the facets of a recipe.  We would spin up a recipe service that handles aggregation of ingredients, assemblage of ingredients into a recipe, and storage of directions.

Now imagine we have a menu service that needs recipes.  Instead of adding the menu functionality to the recipe service, we add a menu service.  It's responsible for holding recipe Ids, descriptions of the recipe, prices, and possibly when the menu expires.  

We've explored two contexts that should be kept separate. This is a simplified example of bounded contexts.  There are several other concerns we need to think about: how do they communicate with one another?  Do we need services that combine these two services together?  Would it be better for the information for both of these to live in the same database?  We'll explore these as we start building out our project.

Alright, I think you've got a good idea where I'm coming from regarding contexts.  I like to express these contexts as micro services.  Now that we've tied the two concepts together in a concrete way, when I use either term interchangeably, you'll know what I"m talking about.,

What next?  Next, we explore the different pieces the owner wants and then go back him and get a priority for each of the pieces that he wants.  Once we know what we will be building in what order, then we can start getting into the nitty-gritty of software development.

Stay tuned.  I love talking about this high-level architecture stuff.  But even better, I like putting it into practice.

-Brian