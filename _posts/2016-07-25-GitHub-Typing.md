---
layout: post
title: "GitHub Typing"
author: bryan
date: 2016-07-25
comment: true
categories: [Articles]
published: false
noindex: false
---

>"We do not do these things because they are easy, we do them because they are hard"
-JFK

## Background
The contribution heatmaps on GitHub profiles are interesting. For one, I'm not sure what purpose they serve other than gamification. I somewhat resent that they are used as a measuring stick for productivity - there are just so many other things that might affect one's commit frequency - although I admit doing it myself.

The really interesting thing about them is that although they are intended to be passive data visualizations, they can do more, namely, act as a 7xN pixel *very slowly* scrolling display. Just like the white train that haunts Ramon in [Beat Street](https://en.wikipedia.org/wiki/Beat_Street), I decided I had to do something to shape the blank canvas that is my GitHub commit log.

## The plan
Ostensibly, it should be very simple: the color of each cell of the heatmap is based on the number of commits made that day, so one just needs to automate the appropriate number of commits per day to get the shading required. For simplicity we'll start with something that uses just the darkest shade possible. There will be some noise in the coloring, because there will be days when no shading is required that real work gets done, but with enough commits on other days it should be fine.

## The execution
And to be honest, it pretty much *was* that simple. The most difficult part was finding a Python library to use for programmatically doing the git commits. Many stack overflow discussions essentially suggested rolling your own functions because it is relatively simple and flexible. Had I been building something I cared more about, that might have been the way to go, but I was determined not to spend more than a few minutes on this project and I didn't need a lot of flexibility. I really wanted to find something off-the-shelf with good documentation.

#### Connecting to GitHub
I first tried XXXX and XXXX. Both seem to be valiant entries into the space, but what one lacked in functionality the other lacked in documentation. Finally I tried github3.py and found the right mix. Without too much trouble, I was able to automate connecting to GitHub and making commits.

#### Translating letters to useable format
Now that I had a way to do commits programmatically, I needed a way to schedule the program to run at the appropriate time. The ultimate goal was to be able to tell the program what I wanted to do at the beginning and have it run unsupervised for a few weeks until it completed with me *never having to worry about it again*.

Two things were required for this - first a way to schedule the script to run every day. Ok, we'll need some version of `cron`, no big deal. But we also need a way for the program to know whether it should do commits that day or not.


letter.py

#### Automating and scheduling the runs
Pythonanywhere


## Troubleshooting
#### An example of using API from command line (helpful for diagnosing problems)
curl https://api.github.com/repos/bryancshepherd/GitHubTyper/contents/files/file1.txt
