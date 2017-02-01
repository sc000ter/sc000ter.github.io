---
layout: post
title: How did we get here?
subtitle: How I set up this blog on the FREE! (or very cheap!)
image: /img/R2_BB8.jpeg
share-img: /img/R2_BB8.jpeg
tags: [blogging, github]
---

# How did we get here?
##Background

I love software development.  I really do.  I love to cook too.  I approach my software development like I approach my cooking: assemble the ingredients, follow the recipe, add a bit of flair, and watch the magic happen!

It gives me great satisfaction being able to assemble something that can bring people joy or fulfillment.  Even if what I assemble is a mundane line-of-business application that uses the Fibonacci sequence to collate characters pulled from strings into custom TPS reports delivered in PowerPoint being delivered to a CompuServe user.  Or pie.  Everyone loves pie. --but I digress.

## On with the bloggings!
So, how'd I setup this site for nothing.  Simple.  Click on the <a href="http://deanattali.com/beautiful-jekyll/" target="_blank">beautiful-jekyll</a> link at the bottom of the page.  Or the one in the previous sentence.  Whatevs.  Check out the `readme.md` file.  That's it.

Done.

Seriously, if you follow Dean's instructions in that readme file, you will have a fully functioning blog.  The beauty is, you don't even need to know how to write HTML!  You just need to learn Markdown.  Easy peasy.  

Now, let's take it a step further.  For about $11/year, you can buy a domain from Namecheap.  In the readme file, go look at Dean's advanced post about hooking up your own domain name to your github repository.  Follow them exactly.  Namecheap's interface has changed a bit.  Once you have your domain, login to NameCheap.  On the dashboard, click the "Manage" button to the right of your new shiny domain.  Then click on "Advanced DNS" and make those entries look like what is described in Dean's post.  Voila! Your domain is now pointed to the GitHub pages site you set up and you even have some fake blog posts to play around with.

## Enhancements
I decided I wanted to open the site up to comments so I grabbed an account on <a href="https://disqus.com/" target="_blank">Disqus</a>.  I followed the instructions for installing disqus on my site wihtout actually installing it.  Because once you are setup to moderate a channel (or site, that was a little confusing to me), you will get a disqus "short name".  

Reading more in the `readme.md` file, you will see a commented out section that talks about disqus.  Just uncomment out the line where you need to put your "short-name" and now your pages will have a disqus comment section at the bottom of them!

How 'bout some analytics?  Go grab a Google login and head over to <a href="https://www.google.com/analytics/analytics/#?modal_active=none" target="_blank">Google Analytics</a>.  Sign up, it's free.  Add a new compaign.  Just call it your domain name.  I did.  I don't expect to get crazy with the analytics, but I would like to know how many people visit my site, how long they stay, what are popular posts, etc...

## Markdown, you say?
Now that you have everything setup (oh sure, there's more you can add if you explore the `readme.md` file and I encourage you to do so), but I have enough customization now.  I've filled in all the social media outlets I want to share with you, given the e-mail you can reach me at, and you can subscribe to my RSS feed.  But now it's time to start producing content.

The only thing you need to know is Markdown.  If you know HTML and CSS, that's helpful but not necessary. 

Markdown files are typically files that with an extension of `.md`.  That readme file I keep directing you to is a big markdown file.  GitHub uses Jekyll to render it into HTML your browser can understand.

Before you compose the next great blog post, spend some time and go <a href="http://markdowntutorial.com/" target="_blank">learn the basics of markdown</a>.

## Managing your posts
Everything we've talked about up to now has been free--except the domain name, I know....  I like the idea of being able to construct my posts offline and them upload or "commit" them to my reblogsitory whenever I want.

To do that I downloaded <a href="https://desktop.github.com/" target="_blank">GitHub for desktop</a>.  This allows me to not have to have a browser window up and I can sync my local copy up to GitHub whenever I feel like it.  If I happen to be in a browser session and make changes up in the repository, GitHub for Desktops can pull it down so I stay in sync.

Now, to easily create posts, I'm using <a href="https://markdownmonster.west-wind.com/" target="_blank">Markdown Monster</a>.  This is an editor that features interactive preview while you type, a helpful markdown toolbar to assist in heading sizes, inserting images and links, and other shortcuts.  I especially like the spell check.  This software has a trial license but I may end up buying it.  Constructing posts in advance is worth it to me.  I can just ignore syncing those posts until the time comes to post them and, then sync with GitHub.  

I know, I could just dig into the `index.html` file and manipulate the code to only pull posts that are today and older.  Maybe after I get a few under my belt.

Alright, looks good around here!  We've got a simple, easy-to-maintain blog up and running.  I am the master of my content.  Having a local copy means I can quit Git at any time and I haven't lost anything.  If I feel like running Jekyll locally, I can do that but so far, the ease-of-use of Git combined with Markdown Monster makes for a compelling case.  (Yes, Markdown Monster has already crashed on me while I was writing this post and I lost the first 3 paragraphs, nothing's perfect)

In future posts, I'll share just what my vision of "The Backyard" is and give you the 30,000 foot view.  It is--at least in my mind--a pretty ambitious project.  Then we'll come down to the nitty gritty and git busy. :)

--Brian
