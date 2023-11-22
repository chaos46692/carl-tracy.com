---
title: "History of carltracy.com"
date: 2021-03-12T12:45:26-05:00
draft: false
tags : [website,history]
categories : [nerd]
---
## Disclaimer
First off let me say that I am by no means an authority on web technology, I wanted to re-do my website and part of that is making content. Writing is one of the things that I really suck at doing so I'm basically forcing myself to do it here, and part of that is discussing the technologies I've used over the years to build it.
# How my websites were built
## The early days
Back in the mid to late 90s, around the time I was graduating from college, the web started to become a thing. It was this neat new tool that would allow you build something that anyone in the world could see. The functionality was pretty limited and a good portion of the content was written in HTML (hypertext markup language), by hand, in text files and had to be set up manually on your system (usually in a directory like ~\\public_html\\). You had to set permissions and do all sorts of crazy stuff to make it work and even beyond that you had to have *access* to a computer system that allowed you to host websites.

My first website was built on a NeXT computer and hosted at [Rose-Hulman](https://rose-hulman.edu/). I learned by trial and error, looking at other people's code and hitting up my friends. I learned to use tables to build layouts and make my site look more interesting. I learned how to compress images to make them faster to download. I learned the ways of the \&nbsp; and various other tricks that you had to do to make your website look rad on the primitive browsers of the day in HTML version 2.1 or whatever the hell it was back then. For the most part building a website was very manual and always static.

Over the years many technologies were added that allowed people to make better content, most notably JavaScript and CSS. CSS allowed a way to specify how your content should look and be formatted in ever increasingly more complicated ways. JavaScript allowed you to put an actual programming language *into your website*, which was really awesome. I mostly did incredibly stupid shit with these kinds of things (I once made a boomarklet that lets you put googly eyes on a website).
## Dynamic content
Static content is okay for a lot of things, but it didn't take long for somebody to realize that if you are hosting these files on a computer network you could use them to... *compute*. People started using languages like perl to create scripts that could take inputs and do things with them. Now websites were not longer static, they could react to things. Well if you are hooking it up code, you can use that code to access a database which means you can remember things. You can allow users to log on to your website and remember the things they've done. They can buy things, take classes, check your bank balance, literally anything that you could accomplish with a computer you could hook up to the internet. Woot!

With this came the advent of content management systems (CMS). These were very complex platforms that allowed a user to build a website that dynamically built websites on-demand. The content was stored in a database and whenever someone went to your website the webpage was built automatically from some code. It's a really neat technology and for many years this website was hosted on one of those CMSs (Joomla).
## Current iteration Static Site Generator
My current site is built with a static site generator named [Hugo](https://gethugo.io). It simplifies the process of creating a webpage by using a language called Markdown, which greatly simplifies the process of creating formatted text. HTML and CSS are very particular and require lots of brackets and formatting and generally are very distracting from the process of writing content. Markdown does have a learning curve, but it is much less steep and it takes less effort to get down words without being distracted by the specifics of *how the words should look*.
## What is a static site generator {#ssg}
Static site generators are sort of a "Back to the Future" kind of deal. It's using modern technology to create something not too dissimilar to webpages I used to make all those years ago in college. A static site generator takes content from *somewhere* (in my case text files) and generates a static website that is nothing but plain old HTML, CSS and maybe some JavaScript.
### Why would you use a static site generator?
So why the hell would I do that, isn't that moving backwards? Yes and no. Here are my main reasons for using a static site generator:
#### It's fast
**Super fast**. One of my big frustrations with database based CMSs is that they are slow, *especially* if you are running them on shared hosting. If you happen to be on a server with another website that is experiencing a surge or your hosting service has oversold the hosting it can take 10, 20 or even 30 seconds for your webpage to load up, and that just sucks. In theory I could get a dedicated server, but the cost of that is thousands of dollars a year and is a ton of work.
#### It's more secure
One of the biggest problems I had with Joomla is that it get hacked *constantly*. I was always playing whack-a-mole with hackers getting into my websites and using them for spam. Because most of them use languages like php and because they have to be set up to run a certain way CMSs have to be built very carefully, otherwise hackers can inject malicious code into URLs which will allow them to gain access to your website. Because a static site generator has no code there's not really an easy way for them to do that.
#### It allows you to use tools like git to backup your site
Because all of the inputs are text files, I can use version control systems like [GitHub](https://github.com) to store my website. With newer versions of cPanel hosting you can pull from GitHub which means deploying is relatively easy.
#### I never really used the dynamic features of other CMSs
At some point I realized that Jooma (or WordPress, etc.) was pretty cool, but for my needs it was a sledgehammer-size solution to a peanut sized problem. All of that functionality was wasted on me and given the other downsides it just didn't make sense.
#### Experimenting and updating is more secure
I like to futz with my technology and static site generators like Hugo definitely scratch that itch in a way that is much safer than traditional CMSs. Because the production version is stashed on GitHub/the server I can mess with it as much as I want locally until I finally get it right. If I screw it up Hugo pukes and lets me know I screwed it up. With Joomla it was much harder to do that kind of stuff (and as noted before was often inviting hackers into your site).
### Why wouldn't you use a static site generator
The funny thing about static site generators is that while they allow you to concentrate on writing, if you want to do *anything* other than what your theme allows you to do out of the box it is going to get very hard, very quickly. I picked up Hugo pretty quick but I have a lot of experience with technology and setting up things like this. To be perfectly honest we use [Squarespace](https://www.squarespace.com/) for the barn website and I actually really love it. I have had exactly zero problems with it over the years and would recommend it to anyone who wants to build a website but doesn't want to learn about coding.
### Update 2022 - moving to carltracy.com
I got around to figuring out how to [deploy the site from GitHub]({{< relref "hosting-from-github-with-aws.md" >}}).
