---
layout: post
title: Setting up Bluehost DNS for a GitHub Jekyll blog
date: 2015-05-27
author: bryan
comments: true
categories: [Articles]
---

When switching to a Jekyll/GitHub-based blog I found the available directions for setting up the DNS rather unhelpful. In Googling for additional information, many of the top search results contradicted each other or were out of date. Additionally, there was nothing specifically related to my hosting provider - Bluehost. 

For posterity, here is how to set up the Bluehost DNS settings to point to a GitHub blog served from the default user page repository, i.e., `<username>.github.io`. This information is current as of 5-27-2015.

#### Edit your CNAME file

Setting up the user page repository is out of scope here, so I'm assuming you have all of that done already. If not, there are a number of Google results that will help you with that.

First, decide whether you want the URL displayed in the address bar to include 'www' or not. This will determine what you need to change on the Bluehost side. There is a whole technical sidebar discussion and set of jargon that we'll avoid here (and that I am not an expert in), but unless you know you want a certain version, you're fine choosing based on preference. 

First, decide whether you want the URL displayed in the address bar to include'www' or not. This will determine what you need to change on the Bluehost side. There is a whole technical sidebar discussion and set of jargon that we'll avoid here (and that I am not an expert in), but unless you know you need a certain version, you're fine choosing based on preference. 


Once you've decided, edit the CNAME file in your `<username>.github.io` repo to match your decision. If you don't already have this file, you will need to create it. This file will have only one line, either:

`www.yoururl.com`

or:

`yoururl.com`


#### Edit your Bluehost DNS setting

After editing your CNAME file you need to change your DNS settings in Bluehost, by follwing these steps:


1. Log in to your Bluehost account 
2. In cPanel go to 'DNS Zone editor'
3. Select the appropriate domain from the drop down menu
4. In the 'Add DNS Record' form do one of two things, depending on your CNAME edit (do not do both!)


* If you used 'www' in your CNAME file
  1. Enter 'www' in the 'Host Record' field
  2. Select 'CNAME' from the 'Type' drop down
  3. Enter your GitHub user page repo name, e.g., `<username>.github.io` in the 'Points To' field
  4. Leave all other fields at their defaults and click 'add record'
 
 
* If you did not use 'www' in your CNAME file
  1. Under the 'A (Host)' DNS settings, delete any existing '@' host records
  2. In the 'Add DNS Record' form, enter your URL, without 'www' in the 'Host Record' field, e.g. 'yoururl.com'
  3. Leave (or select) 'Type' as 'A'
  4. Enter the first GitHub IP address `192.30.252.153` in the 'Points To' field
  5. Leave all other fields at their defaults and click 'add record'
  6. Repeat to add another GitHub IP address `192.30.252.153`


After you've made one set of changes above, give them a few hours to propogate. You can check the status at [What's My DNS](https://www.whatsmydns.net/)
