---
title: "Hosting from AWS with GitHub"
date: 2022-11-06T09:02:38-05:00
draft: false
tags : [website,hugo,ssl,AWS]
categories : [nerd]
---
# Back at it again
Well I certainly haven't done much with this lately. Part of that is I just feel like I didn't have much to say and part of it is the setup I had was a little too much of a pain to deploy. 
Because, you know, actually having to log in and push 2 or 3 buttons is **so** much of a pain. Anyway, I got a bug up my butt and figured out how to build things with AWS and GitHub and did 
a small project over at [Evil Peep](https://www.evilpeep.com) which has been sitting dormant for around 12 years. I figured while I was at it I might was well implement it for my site 
and use the opportunity to move from my old domain to this one! While I'm at it I figured I'd document what I did to make it all work.

I like AWS for a couple of reasons: it's fast, you're only charged for what you use it's an industry standard, and lots of people have done this stuff before me so I don't have to figure it out myself. 
## Basic outline
### Setting up static web hosting (S3) and https (CloudFront)
 The list below is a very short summary of the steps from [this article](https://www.stormit.cloud/blog/setup-an-amazon-cloudfront-distribution-with-ssl-custom-domain-and-s3/#route-53-first)
- Sign up for [Amazon AWS](https://aws.amazon.com/)
- Create an AWS [S3 bucket](https://s3.console.aws.amazon.com/s3/)
  - Create Bucket
  - Set the permissions to public
  - Edit the Bucket Policy
  - Turn on static web hosting (in properties)  
- Set up a repository on [GitHub](https://wwww.github.com") with my code
- Set up an AWS CodePipeline that pulled my code from GitHub anytime it was deployed to the S3 container
- Linked a CloudFront distribution to the S3 static website **not the container** that will cause problems (took me an hour to figure that out when I was moving this site onto AWS)
- Tranfer nameservers to AWS Route 53
- Set up https on CloudFront (because [Google]((https://blog.google/products/chrome/milestone-chrome-security-marking-http-not-secure/) ))
- Pointed [Evil Peep](https://www.evilpeep.com) at CloudFront through Route 53
- Created (okay I borrowed code lol) an AWS Lambda that invalidates the CloudFront distribution files every time I deployed

### Updating CloudFront with Lambda
The last bit of work that needs done is to make sure that the [CloudFront distribution gets updated](https://medium.com/fullstackai/aws-creating-a-cloudfront-invalidation-in-codepipeline-using-lambda-actions-49c1fd3a3c31) after the S3 container is refreshed. Once the code from Lambda is added to your CodePipeline every time you deploy to GitHub the CloudFront distribution will automatically be invalidated which will force a refresh. _Voila_ automatic website updates!



