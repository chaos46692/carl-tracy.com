---
title: "Hosting from AWS with GitHub"
date: 2022-11-06T09:02:38-05:00
draft: false
tags : [website,hugo,ssl,AWS]
categories : [nerd]
---
#Back at it again
Well I certainly haven't done much wiht this lately. Part of that is I just feel like I didn't have much to say and part of it is the setup I had was a little too much of 
a pain to deploy. Because, you know, actually having to log in and push 2 or 3 buttons is **so** much of a pain. Anyway, I got a bug up my butt and figured out how to build 
things with AWS and GitHub and did a small project over at 	[Evil Peep](https://www.evilpeep.com) which has been sitting dormant for around 12 years. I figured while I was 
at it I might was well implement it for my site and use the opportunity to move from my old domain to this one! While I'm at it I figured I'd document what I did to make it 
all work.
##Amazon Web Services
I like AWS for a couple of reasons: it's fast, you're only charged for what you use it's an industry standard, and lots of people have done this stuff before me so I don't have to figure it out myself. 
##Setting up S3 and CloudFront
I used [this article](https://www.stormit.cloud/blog/setup-an-amazon-cloudfront-distribution-with-ssl-custom-domain-and-s3/#route-53-first) as a guide to set up my site. You can host your page directly 
from S3, but unfortunately it doesn't allow you to use https, which means that Chrome will mark the site as 
[as unsafe ](https://blog.google/products/chrome/milestone-chrome-security-marking-http-not-secure/) which for some reason annoys me. So, you have to deploy through CloudFront, which is actually helpful 
because in addition to making the site even faster, it means that I can push the full hugo folder to GitHub, but point CloudFront at the public/ folder (in the origin settings set "Origin path" as public/). 

One other stumbling block along the way was that I needed to point CloudFront at the address for the static website, not the S3 bucket. Basically the URL should look like s3-website-us-blah-blah rather than s3-us-blah-blah. 
Also, the subfolder for CloudFront is "public/" **not** "/public" _facepalm_.
##Updating with Lambda
The last bit of work that needs done is to make sure that the 
[CloudFront distribution gets updated](https://medium.com/fullstackai/aws-creating-a-cloudfront-invalidation-in-codepipeline-using-lambda-actions-49c1fd3a3c31)  
after the S3 container is refreshed.