---
layout: post
title: "Setting Up Mahout on AWS"
author: bryan
date: 2016-07-25
comment: true
categories: [Articles]
published: false
noindex: false
---

I'll be following the tutorial posted at the link below, but I'll be filling in some things that I had trouble with as I went along.

[Building a Recommender with Apache Mahout on Amazon Elastic MapReduce (EMR)](https://blogs.aws.amazon.com/bigdata/post/Tx1TDK3HHBD4EZL/Building-a-Recommender-with-Apache-Mahout-on-Amazon-Elastic-MapReduce-EMR)

### Step 0: Set up an AWS account
If you haven't already, set up an AWS account. That's beyond the scope of this walkthrough, so it won't be covered here.

### Step 1: Create an IAM User
Create a IAM user (or select a user you have already created) for the Manhout app that we're creating. To do this:

- Log in to the Amazon Web Services console.
- Click `Identity & Access Management` under `Security & Identity`.

![IAM](/images/IAM.jpg)

- Select `Users` on the left.
- If you don't have any users listed or want to create a new one for use with this app, click the 'Create New Users' button near the top of the page.

Once you have a user you want to use in your list, you need to give them the appropriate permissions:

- Click on the username to go to that users details.
- Click on the `Permissions` tab.
- Click `Attach Policy` to assign permissions.
- You'll see a searchable list of permission settings. To keep things simple, we'll use the policy with full EMR access. Type `AmazonElasticMapReduceFullAccess` into the search bar.
- Check the box for that result and then click `Attach Policy`. (This level of access may or may not be right for your environment - that's a question for your AWS admin.)

### Step 2: Setup the AWS CLI
The AWS CLI is how you will interact with AWS; you should install it now.
The Amazon documentation for this step is very clear and concise; I can't improve on it. It is here:
[AWS Command Line Interface Guide](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)

### Step 3: Test Everything So Far
If everything has worked well so far, you should be able to run the following command without error:

```sh
$ aws ec2 describe-instances --output table --region us-east-1
```
You'll get something like:

```sh
-------------------
|DescribeInstances|
+-----------------+
...
```
But your output may be different depending on what you have running on your EC2.


EMR management guide:
http://docs.aws.amazon.com//ElasticMapReduce/latest/ManagementGuide/emr-what-is-emr.html
