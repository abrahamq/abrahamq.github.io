---
layout: post
title: "Setting Up a Static Site Using AWS"
date: 2018-07-18
categories: guides aws
---

This is a generic guide to setting up a static site using AWS.

1. Register domain name using route 53 on aws.

2. Create an s3 bucket and upload your static site to that bucket.

3. Create a CloudFront distribution, selecting the s3 bucket as the origin domain name.

   - Set the distribution to redirect http to https
   - Select a custom SSL Certificate and request one with ACM
   - Add a CNAME that matches your domain name
   - Deploy your cloudfront distribution, this will take a while (~10 minutes for me in us-east-1)
   - Edit the origin so that "Restrict Bucket Access" is checked. Now create an origin access identity that will let cloudfront read the s3 bucket contents. You can either have cloudfront modify the bucket policy, or you can do it yourself.

4. Once deployed, go back to route 53 and create a record set. Choose yes on the alias radio button and then click on Alias Target, choosing your cloudfront distribution as the target.

5. You're all done!
