---
layout: post
title: Creating your personal, portable R code library with GitHub
date: 2013-09-23 09:29
author: bryan
comments: true
categories: [Articles]
noindex: true
---
As I discussed in a <a title="R Helper Functions" href="http://bryancshepherd.com/r-helper-functions/">previous post</a>, I have a few helper functions I've created that I commonly use in my work. Until recently, I manually included these functions at the start of my R scripts by either the tried-and-true copy-and-paste method, or by extracting them from a local file with the `source()` function. The former approach has the benefit of keeping the helper code inextricably attached to the main script, but it adds a good bit of code to wade through. The latter approach keeps the code cleaner, but requires that whoever is running the code always has access to the sourced file and that it is always in the same relative path - and that makes sharing or moving code more difficult. The start of a recent project requiring me to share my helper function library prompted me to find a better solution.

The resulting approach takes advantage of GitHub Gists and R's ability to source via a web-based location to enable you to create a personal, portable library of R functions for private use or to share.

The process is very straightforward. If you don't have a GitHub account, getting one is the first step. After doing so, click on "Gist" in the menu bar to create a new Gist. (There are a couple of reasons for using a Gist instead of a formal repository. For one, repositories cannot be made private unless you have a paid GitHub membership. Gists, however, can be created as "Secret" and therefore available only to people who have the exact URL.) Name your Gist, add your code, and choose whether you want it to be a secret or public Gist to save that revision. After saving your Gist you will be taken to a screen displaying your new Gist.

Now that you've created your Gist, you just need to source it in your R code. That step is easily done via the `source()` function. Rather than including a file path as you would with a local file, you simply include the URL to your Gist. Note that you can't just copy the URL of the GitHub page you're currently on. You need to source the URL of the raw code. To get that URL, click on the `<>` button on the top right of your Gist (circled in red below). This will take you to the raw code *for the current revision*. To get the path that always points to the most recent revision, remove everything in the URL after "/raw/".
<p style="text-align: center;"><a href="http://www.programmingr.com/wp-content/uploads/2013/09/GitHubGist.jpg"><img class="aligncenter size-full wp-image-1006" alt="GitHubGist" src="http://www.programmingr.com/wp-content/uploads/2013/09/GitHubGist.jpg" width="821" height="356" /></a></p>
Regardless of which version you link to, you'll still be left with a long URL that includes a randomly generated alphanumeric string that you don't want to have to try to remember. Here's a protip, use a URL shortener that let's you define a custom URL (e.g., tinyurl) to create a shortened version that is easy to remember. Something like the little library at <a title="ProgrammingR Example Library" href="http://tinyurl.com/ProgRLib">http://tinyurl.com/ProgRLib</a>  (YES! You can use and add to this library!).

Now you've got a portable, easy to reference, personal library of functions that you can easily include in your code or share with others.
