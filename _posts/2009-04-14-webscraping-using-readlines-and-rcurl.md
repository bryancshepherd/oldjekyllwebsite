---
layout: post
title: Webscraping using readLines and RCurl
date: 2009-04-14 22:56
author: bryan
comments: true
categories: [Articles]
---


There is a massive amount of data available on the web.  Some of it is in the form of precompiled, downloadable datasets which are easy to access.  But the majority of online data exists as web content such as blogs, news stories and cooking recipes.  With precompiled files, accessing the data is fairly straightforward; just download the file, unzip if necessary, and import into R.  For "wild" data however, getting the data into an analyzeable format is more difficult.  Accessing online data of this sort is sometimes reffered to as "webscraping".  Two R facilities, <code>readLines()</code> from the base package and <code>getURL()</code> from the RCurl package make this task possible.
<br/>

<h2>readLines</h2>
For basic webscraping tasks the <code>readLines()</code> function will usually suffice.  <code>readLines()</code> allows simple access to webpage source data on non-secure servers.  In its simplest form, <code>readLines()</code> takes a single argument - the URL of the web page to be read:

<code>web_page <- readLines("http://www.interestingwebsite.com")</code>
<br/>

As an example of a (somewhat) practical use of webscraping, imagine a scenario in which we wanted to know the 10 most frequent posters to the R-help listserve for January 2009.  Because the listserve is on a secure site (e.g. it has https:// rather than http:// in the URL) we can't easily access the live version with <code>readLines()</code>.  So for this example, I've posted a local copy of the  list archives on the this site.
<br/>

One note, by itself  <code>readLines()</code> can only acquire the data.  You'll need to use <code>grep(), gsub()</code> or equivalents to parse the data and keep what you need.

<code># Get the page's source</code>
<code>web_page <- readLines("http://www.programmingr.com/jan09rlist.html")</code>

<code># Pull out the appropriate line</code>
<code>author_lines <- web_page[grep("&lt;I&gt;", web_page)]</code>

<code># Delete unwanted characters in the lines we pulled out</code>
<code>authors <- gsub("&lt;I&gt;", "", author_lines, fixed = TRUE)</code>

<code># Present only the ten most frequent posters</code>
<code>author_counts <- sort(table(authors), decreasing = TRUE)</code>
<code>author_counts[1:10]</code>
<br/>

<div style="text-align: center"><a href="http://www.programmingr.com/images/webscrape1.jpg"><img height="300" title="webscrape results" src="http://www.programmingr.com/images/webscrape1.jpg"/></a></div>
<br/>

We can see that Gabor Grothendieck was the most frequent poster to R-help in January 2009.
<br/>

<h2>The RCurl package</h2>
To get more advanced http features such as POST capabilities and https access, you'll need to use the RCurl package.  To do webscraping tasks with the RCurl package use the <code>getURL()</code> function.  After the data has been acquired via <code>getURL()</code>, it needs to be restructured and parsed.  The <code>htmlTreeParse()</code> function from the XML package is tailored for just this task.  Using <code>getURL()</code> we can access a secure site so we can use the live site as an example this time.

<code># Install the RCurl package if necessary</code>
<code>install.packages("RCurl", dependencies = TRUE)</code>
<code>library("RCurl")</code>

<code># Install the XML package if necessary</code>
<code>install.packages("XML", dependencies = TRUE)</code>
<code>library("XML")</code>

<code># Get first quarter archives</code>
<code>jan09 <- getURL("https://stat.ethz.ch/pipermail/r-help/2009-January/date.html", ssl.verifypeer = FALSE)</code>

<code>jan09_parsed <- htmlTreeParse(jan09)</code>

<code># Continue on similar to above</code>
<code>...</code>
<br/>

For basic webscraping tasks <code>readLines()</code> will be enough and avoids over complicating the task.  For more difficult procedures or for tasks requiring other http features <code>getURL()</code> or other functions from the RCurl package may be required.  For more information on cURL visit the project page <a href="http://curl.haxx.se" title="cURL project page">here</a>.



<br />
