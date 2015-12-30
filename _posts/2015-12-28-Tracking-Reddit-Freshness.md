---
layout: post
title: Tracking Reddit Freshness with Python and RPi
date: 2015-12-28 
author: bryan
comments: true
categories: [Articles]
noindex: false
published: false
---

A couple of years ago I purchased two Raspberry Pis to build a home computing cluster with. A cluster of RPis aren't going to win any computing awards, but since they run a modified version of Debian Linux, much of what you learn doing it is generalizable to the real world. Also, it's fun to say you have a compute cluster at your house - well, for people like me it is.

D3.js was exciting the visualization solution at the time so after playing around with the clustering capabilites, I decide to use the Pi's webstack to serve a small D3-based dasboard. I often end up using Reddit when I am looking for streaming data to play with because the API is so easy to work with (via a module called PRAW) and there is a lot of data. This work was almost lost to the ages, but as I was cleaning up the RPis for use as print and backup servers, I came across the relevant code. As this code might be useful to others, I figured I would write it up quickly and post the code to GitHub. This post describes using the Raspberry Pi, Python, Reddit, D3, and some basic statistics to setup a simple dashboard that displays the freshness of the Reddit front page and r/new. 

#### Getting the data

Raspberry Pis come with several OS choices. Raspbian is a version of Debian tailored to the Raspberry Pi and is usually the best option for general use. It comes preinstalled with Python 2.7 and 3, meaning it's easy to get up and running for general computing. The PRAW module (add link) is a wrapper for the Reddit API and is what I used to access the Reddit data. It's a well written package that I've used for several projects, just because of its ease of use and the depth of the available data. PRAW does not come preinstalled, so you'll need to add it the usual way with

`sudo python3 -m pip install praw'

(If you're using Python != 3 just specify your version in the code above)

Use of the PRAW is beyond the scope of this write up, but you can check out the well-written docs and examples here (add link) and check my code in the GitHub repo linked at the end of the article. 

#### Analyzing the data

The main goal of the project was to get to something that could be displayed on the dashboard, not to do fancy stats, but this is one of those cases where simple can be very informative. The front page analysis includes a few metrics related to rank correlation (although not all are used); the r/new analysis is based on the percent of new articles on each API pull. 

#### Displaying the data

D3 is the main workhorse in the data visualization, primarily using SVG.



