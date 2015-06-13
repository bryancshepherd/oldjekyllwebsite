---
layout: post
title: Animations in R
date: 2012-04-24 15:02
author: bryan
comments: true
categories: [Articles]
noindex: true
---

Animated charts can be very helpful in illustrating concepts or discovering relationships, which makes them very helpful in teaching and exploratory research. Fortunately, creating animated graphs in R is fairly straightforward, once you have the right tools and understand a few basic principles about how the animations are created.
In this article I'll provide an example of how to use the `animation` package to create an animated chart with a couple of bells and whistles.
The package installs out-of-the-box with several animations that are tailored for instruction. The examples are of varying complexity ranging from a simple coin flip simulation to illustrations of mathematical problems such as Buffon's needle problem. In most scenarios, however, you'll want to create your own animations, so let's look at how to do that.
First, there are several different formats in which you can create your animations - GIF, HTML, LaTeX, SWF and mp4. The `saveGIF()` function call below illustrates the generic format for each of the calls:

```r
saveGIF({
  for (i in 1:10) plot(runif(10), ylim = 0:1)
})
```

Understanding that the package creates animations by generating and then compiling many graphs is central to creating polished custom animations. As you can see, the syntax looks a little unfamiliar at first because the inside of the function call is a custom loop that creates the individual graphs. (Note: If you're familiar with with the way the `boot()` function works, this is somewhat similar.) Once those individual graphs are created, the function compiles the images in the format specified by the function call. As you might have guessed, most of the animation types require that you install 3rd party libraries for R to be able to do the compilations. The installation of these libraries is covered in the package help.
Basic use of the animation functions is covered in the package help, but the application of the functions to novel tasks can still be a little difficult. As a result, I've created an example that illustrates how to use the functions to create animations with a couple of bells and whistles.
This animation plots the density functions of 150 draws of 100 values from a normally distributed random variable. To make things a little more interesting (i.e., make the distribution move), a constant that varies based on the iteration count is added to the 100 values. The chart also includes a slightly stylized frame tracker (or draw counter) along the top of the chart and a horizontal bar that notes the current position and previous two positions of the sample mean. Finally, the foreground color of the chart changes based on the mean of the distribution. 

```r
library(animation)

#Set delay between frames when replaying
ani.options(interval=.05)

# Set up a vector of colors for use below
col.range <- heat.colors(15)

# Begin animation loop
# Note the brackets within the parentheses
saveGIF({
	# For the most part, it's safest to start with graphical settings in 
	# the animation loop, as the loop adds a layer of complexity to 
	# manipulating the graphs. For example, the layout specification needs to 
	# be within animation loop to work properly.
	layout(matrix(c(1, rep(2, 5)), 6, 1))

	# Adjust the margins a little
	par(mar=c(4,4,2,1) + 0.1)

		# Begin the loop that creates the 150 individual graphs
		for (i in 1:150) {
			# Pull 100 observations from a normal distribution
			# and add a constant based on the iteration to move the distribution
			chunk <- rnorm(100)+sqrt(abs((i)-51))

			# Reset the color of the top chart every time (so that it doesn't change as the 
			# bottom chart changes)
			par(fg=1)

			# Set up the top chart that keeps track of the current frame/iteration
			# Dress it up a little just for fun
			plot(-5, xlim = c(1,150), ylim = c(0, .3), axes = F, xlab = "", ylab = "", main = "Iteration")
			abline(v=i, lwd=5, col = rgb(0, 0, 255, 255, maxColorValue=255))
			abline(v=i-1, lwd=5, col = rgb(0, 0, 255, 50, maxColorValue=255))
			abline(v=i-2, lwd=5, col = rgb(0, 0, 255, 25, maxColorValue=255))

			# Bring back the X axis
			axis(1)

			# Set the color of the bottom chart based on the distance of the distribution's mean from 0
			par(fg = col.range[mean(chunk)+3])

			# Set up the bottom chart
			plot(density(chunk), main = "", xlab = "X Value", xlim = c(-5, 15), ylim = c(0, .6))

			# Add a line that indicates the mean of the distribution. Add additional lines to track
			# previous means
			abline(v=mean(chunk), col = rgb(255, 0, 0, 255, maxColorValue=255))
			if (exists("lastmean")) {abline(v=lastmean, col = rgb(255, 0, 0, 50, maxColorValue=255)); prevlastmean &lt;- lastmean;}
			if (exists("prevlastmean")) {abline(v=prevlastmean, col = rgb(255, 0, 0, 25, maxColorValue=255))}

			#Fix last mean calculation
			lastmean <- mean(chunk)
		}
})
```

And the final product:</p><p><br /><a href="http://imgur.com/GLByH"><img title="Hosted by imgur.com" src="http://i.imgur.com/GLByH.gif" alt="" /></a><br /><br /><br />A couple of closing notes:
<ul><li>Because there are external programs involved (e.g., SWF Tools, ImageMagick, FFmpeg), the setup for this package is slightly more difficult than the average package and things will likely seem less polished than normal. Things may also not work as well; you'll need to be prepared to be flexible with your animation formats and graph layouts.</li></ul><ul><li>Animation works exceptionally well when smaller numbers of individual graphs are being compiled, but as the number of individual graphs grows, so does your likelihood of hitting a problem. E.g., although GIF is a very exportable and transportable format, and therefore ideal for many situations, I found that animations with more than ~500 source graphs just didn't compile. The limit for HTML was similar. Your mileage may vary, but again, be prepared to be flexible.</li></ul><ul><li>If you do not need to transport your animation and it will have less than a few hundred individual images, you can avoid installing 3rd party software by using the saveHTML function. This output also includes an interface that allows you to pause and move within the animation easily.</li></ul><ul><li>As mentioned in the code above, if you're having trouble getting a particular graphical parameter to work, make sure that it is in the internal loop. For efficiency, you want to keep the loop as clean as possible of course, but some things need to be specified each time a new chart is plotted, and therefore need to be inside the loop.</li></ul><ul><li>Animations aren't very common in research presentations, but can provide extensive insight beyond static images. Given R's advanced graphing capabilities, it's possible to create very nice animations without needing to learn a completely different software package.</li></ul>
If you've created an animation you'd like to share or have additional tips, feel free add them to the comments.
