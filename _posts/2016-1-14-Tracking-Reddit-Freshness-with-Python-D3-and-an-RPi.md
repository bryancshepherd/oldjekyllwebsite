---
layout: post
title: Tracking Reddit Freshness with Python, D3.js, and an RPi
date: 2016-1-14
author: bryan
comments: true
categories: [Articles]
noindex: false
published: true
---

#### Background
A couple of years ago I purchased some Raspberry Pis to build a compute cluster at home. A cluster of RPis isn't going to win any computing awards, but they're fun little devices and since they run a modified version of Debian Linux, much of what you learn working with them is generalizable to the real world. Also, it's fun to say you have a compute cluster at your house, right?

Unfortunately, the compute cluster code and project notes are lost to the ages, so let us never speak of it again. However, after finishing that project I moved on to another one. That work was almost lost to the ages *too*, but as I was cleaning up the RPis for use as print and backup servers, I came across the aging and dusty code. Although old, this code might be useful to others, so I figured I would write it up quickly and post the code to GitHub. This post describes using the Raspberry Pi, Python, Reddit, D3, and some basic statistics to setup a simple dashboard that displays the freshness of the Reddit front page and r/new.

#### Dashboarding
D3.js was the exciting new visualization solution at the time so I decided to use the Pi's Apache-based webstack to serve a small D3-based dashboard.

#### Getting the data
Raspberry Pis come with several OS choices. Raspbian is a version of Debian tailored to the Raspberry Pi and is usually the best option for general use. It comes preinstalled with Python 2.7 and 3, meaning it's easy to get up and running for general computing. Given that Python is available out of the box, I often end up using the [PRAW module](https://praw.readthedocs.org/en/stable/) to get toy data. PRAW is a wrapper for the Reddit API. It's a well written package that I've used for several projects because of its ease of use and the depth of the available data. PRAW can be added in the usual way with:

`sudo python3 -m pip install praw`

(If you're using Python != 3 just specify your version in the code above)

PRAW is straightforward to use, but I'm trying to keep this short so it's beyond the scope of this write up. You can check out the well-written docs and examples at the link above and also check my code in the [GitHub repo](https://github.com/bryancshepherd/RedditFresh).

#### Analyzing the data
The analysis of the data was just to get to something that could be displayed on the dashboard, not to do fancy stats. The Pearson correlation numbers are essentially just placeholders, so don't put much weight in them. However, the r/new analysis is based on the percent of new articles on each API pull and ended up showing some interesting trends - just a reminder that value comes from the appropriateness of your stats, not the complexity of them.

Where statistics or data manipulation were required they were done with [Scipy](http://www.scipy.org/) and/or [pandas](http://pandas.pydata.org/). The Pearson correlation metric is defined as:

{% raw %}
$$
r_{xy} =\frac{\sum ^n_{i=1}(x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum ^n_{i=1}(x_i - \bar{x})^2} \sqrt{\sum ^n_{i=1}(y_i - \bar{y})^2}}
$$
{% endraw %}

But you knew that. I just wanted to add some LaTeX to make this writeup look better.

#### Displaying the data
D3 is the main workhorse in the data visualization, primarily using SVG. The code is relatively simple, as far as these things go. There are essentially two key code blocks, one for displaying the percent of new articles in r/new, the other for displaying a correlation of article ranks on the front page. Below is a snippet that covers the r/new chart:

```js

// Create some required variables
var parseDate = d3.time.format("%Y%m%d%H%M%S").parse;
var x = d3.time.scale()
	.range([0, width]);
var y = d3.scale.linear()
	.range([height, 0]);
var xAxis = d3.svg.axis()
	.scale(x)
	.orient("bottom");
var yAxis = d3.svg.axis()
	.scale(y)
	.orient("left");

// Define the line
var line = d3.svg.line()
	.x(function(d) { return x(d["dt"]); })
	.y(function(d) { return y(+d["pn"]); });

// Skip a few lines ... (check the code in the repo for details)

// Set some general layout parameters
var s = d3.selectAll('svg');
s = s.remove();
var svg2 = d3.select("body").append("svg")
	.attr("width", width + margin.left + margin.right)
	.attr("height", height + margin.top + margin.bottom)
		.append("g")
	.attr("transform", "translate(" + margin.left + "," + margin.top + ")");

// Bring in the data
d3.csv("./data/corr_hist.csv", function(data) {

	dataset2 = data.map(function(d) {
		return {
			corr: +d["correlation"],
			dt: parseDate(d["datetime"]),
			pn: +d["percentnew"],
			rmsd : +d["rmsd"]
		};
	});

	// Define the axes and chart text
	x.domain(d3.extent(dataset2, function(d) { return d.dt; }));
	y.domain([0,100]);
	svg2.append("g")
		.attr("class", "x axis")
		.attr("transform", "translate(0," + height + ")")
		.call(xAxis);
	svg2.append("g")
		.attr("class", "y axis")
		.call(yAxis)
	.append("text")
		.attr("transform", "rotate(-90)")
		.attr("y", 6)
		.attr("dy", ".71em")
		.style("text-anchor", "end")
		.text("% new");
	svg2.append("path")
		.datum(dataset2)
		.attr("class", "line")
		.attr("d", line);

	svg2.append("text")
	.attr("x", (width / 2))             
	.attr("y", -8)
	.attr("text-anchor", "middle")  
	.style("font-size", "16px")
	.style("font-weight", "bold")  
	.text("Freshness of articles in r/new");

});
```

And here's a screenshot of what you get with that bit of code:

![RedditFreshness](http://www.bryancshepherd.com/images/RedditFresh.jpg)

As you can see, the number of new submissions follow a very strong trend, peaking in the early afternoon (EST) and hitting a low point in the early morning hours. Depending on your perspective, Americans need to spend less time on Reddit at work or Australia needs to pick up the pace.

If you want to know more of the details head on over to the GitHub repo.
