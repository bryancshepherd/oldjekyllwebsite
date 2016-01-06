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

#### Background
A couple of years ago I purchased two Raspberry Pis to build a home computing cluster with. A cluster of RPis isn't going to win any computing awards, but since they run a modified version of Debian Linux, much of what you learn doing it is generalizable to the real world. Also, it's fun to say you have a compute cluster at your house - well, for people like me it is.

Unfortunately, that code and project notes are lost to the ages, so let us never speak of it again. However, after finishing that project I moved on to another one. That work was almost lost to the ages *too*, but as I was cleaning up the RPis for use as print and backup servers, I came across the aging and dusty code. Although old, this code might be useful to others, so I figured I would write it up quickly and post the code to GitHub.

#### Dashboarding
D3.js was the exciting visualization solution at the time so after playing around with the clustering capabilites, I decided to use the Pi's webstack to serve a small D3-based dashboard. I often end up using Reddit when I am looking for streaming data to play with because the API is so easy to use (via a module called PRAW) and there is a lot of data.  This post describes using the Raspberry Pi, Python, Reddit, D3, and some basic statistics to setup a simple dashboard that displays the freshness of the Reddit front page and r/new.

#### Getting the data
Raspberry Pis come with several OS choices. Raspbian is a version of Debian tailored to the Raspberry Pi and is usually the best option for general use. It comes preinstalled with Python 2.7 and 3, meaning it's easy to get up and running for general computing. The [PRAW module](https://praw.readthedocs.org/en/stable/) is a wrapper for the Reddit API and is what I used to access the Reddit data. It's a well written package that I've used for several projects, just because of its ease of use and the depth of the available data. PRAW does not come preinstalled, so you'll need to add it the usual way with

`sudo python3 -m pip install praw`

(If you're using Python != 3 just specify your version in the code above)

PRAW is straightforward to use, but I'm trying to keep this short so it's beyond the scope of this write up. You can check out the well-written docs and examples at the link above and also check my code in the [GitHub repo](https://github.com/bryancshepherd/RedditFresh).

#### Analyzing the data
The main goal of the project was to get to something that could be displayed on the dashboard, not to do fancy stats, but this is one of those cases where simple can be very informative. The front page analysis includes a few metrics related to rank correlation (although not all are used); the r/new analysis is based on the percent of new articles on each API pull.

Where statistics or data manipulation were required they were done with [Scipy](http://www.scipy.org/) and/or [pandas](http://pandas.pydata.org/).

#### Displaying the data
D3 is the main workhorse in the data visualization, primarily using SVG. The code is relatively simple, as far as these things go. There are essentially two key code blocks, one for displaying the percent of new articles in r/new, the other for displaying a Root Mean Squared Difference of article ranks on the front page.

```js
		var s = d3.selectAll('svg');
		s = s.remove();
		var svg = d3.select("body").append("svg")
			.attr("width", width + margin.left + margin.right)
			.attr("height", height + margin.top + margin.bottom)
		    .append("g")
			.attr("transform", "translate(" + margin.left + "," + margin.top + ")");

		d3.csv("./data/corr_hist.csv", function(data) {

			dataset = data.map(function(d) {
				return {
					corr: +d["correlation"],
					dt: parseDate(d["datetime"]),
					sig: +d["significance"],
					rmsd : +d["rmsd"]
				};
			});

		  x.domain(d3.extent(dataset, function(d) { return d.dt; }));
		  y.domain([0,20]);
		  svg.append("g")
			  .attr("class", "x axis")
			  .attr("transform", "translate(0," + height + ")")
			  .call(xAxis);
		  svg.append("g")
			  .attr("class", "y axis")
			  .call(yAxis)
			.append("text")
			  .attr("transform", "rotate(-90)")
			  .attr("y", 6)
			  .attr("dy", ".71em")
			  .style("text-anchor", "end")
			  .text("RMSD");
		  svg.append("path")
			  .datum(dataset)
			  .attr("class", "line")
			  .attr("d", line);

		  svg.append("text")
			.attr("x", (width / 2))             
			.attr("y", 0)
			.attr("text-anchor", "middle")  
			.style("font-size", "16px")
			.style("font-weight", "bold")  
			.text("Change in the Top 50 Article Ranks (Root Mean Squared Difference)");
		});
```

And here's a screenshot of what you get with that little bit of code:

![RedditFreshness](http://www.bryancshepherd.com/images/RedditFresh.jpg)

If you want to know more of the details head on over to the GitHub repo and browse or clone the code.
