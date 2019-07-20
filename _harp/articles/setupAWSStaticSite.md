# This is a generic guide to seting up a static site using AWS

1. Register domain name using route 53 on aws.

2. Create an s3 bucket and upload your static site to that bucket.

3. Create a CloudFront distribution, selecting the s3 bucket as the origin domain name.
    1. Set the distribution to redirect http to https
    2. Select a custom SSL Certificate and request one with ACM.
    3. Add a CNAME that matches your domain name
    4. Deploy your cloudfront distribution, this will take a while (~10 minutes for me in us-east-1)
    5. Edit the origin so that "Restrict Bucket Access" is checked. Now create an
    origin access identity that will let cloudfront read the s3 bucket contents. You can
    either have cloudfront modify the bucket policy, or you can do it yourself.
4. Once deployed, go back to route 53 and create a record set. Choose yes on the alias radio 
button and then click on Alias Target, choosing your cloudfront distribution as the target. 


5. You're all done!

6. You can also set up a terraform script to do this all for you, like I
have done [here](https://github.com/abrahamq/SapienSense/blob/gh-pages/deploy.tf).
