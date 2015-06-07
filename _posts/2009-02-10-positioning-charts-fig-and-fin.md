---
layout: post
title: Positioning charts with fig and fin
date: 2009-02-10 03:14
author: bryan
comments: true
categories: [Articles, Help]
---


R offers several ways to spatially orient multiple graphs in a single graphing space.  The <code>layout()</code> function and <code>mfrow</code>/<code>mfcol</code> parameter settings are adequate solutions for many tasks and allow the graphing space to be broken up into tabular or matrix-based arrangements.  For more fine grained manipulation, the <code>fig</code> and <code>fin</code> parameter settings are available.  This article illustrates the capabilities and use of <code>fig</code> and <code>fin</code>.
<br />
First we'll create some simulation data to work with:

```r
# create data
sim.data <- cbind(replicate(5,runif(8,min=0, max=100)))
```

The code above results in a matrix object with eight rows and three columns.

The <code>fig</code> and <code>fin</code> parameters affect the same graphing elements via different units.  The fig parameter takes normalized device coordinates (NDC) and fin takes dimensions in inches of the device region.  Because the <code>fig</code> units are generally more user friendly, I will use it in the examples below; however, selecting equivalent dimensions using the <code>fin</code> would have an identical effect.  Similar to other functions that use NDC to define graphing space, <code>fig</code> takes a four item vector wherein positions one and three define, in percentages of the device region, the starting points of the x and y axes, respectively, while positions two and four define the end points. The default <code>fig</code> setting is <code>(0, 1, 0, 1)</code> and uses the entire device space. The default <code>fig</code> setting is <code>(0, 1, 0, 1)</code> and uses the entire device space.  The graph below illustrates the default settings of <code>fig</code>.

```r
# graph cases by first column using default fig
# settings of 0 1 0 1 (the full device width and height)
par(mar=c(2, 2, 1, 1), new = FALSE, cex.axis = .6, mgp = c(0, 0, 0))

#open plot
plot(c(0,100), c(-1,1), type = "n", ylab = "", yaxt = "n", xlab = "")
points(sim.data[,1], replicate(8, 0), pch = 19, col = 1:8, cex = 1.5)
# add center reference line
abline(0,0)
legend("bottomright", fill = c(1:8), legend = c(1:8), ncol = 4)
```

<div style="text-align: center"><a href="http://www.programmingr.com/images/fig/fig1.jpg"><img height="300" title="fig default" alt="fig default" src="http://www.programmingr.com/images/fig/fig1.jpg"/></a></div>

To make the horizontal dimensions of the graph smaller or to move the graph left or right, adjust the starting and ending x coordinates, given by the first and second positions of the <code>fig</code> value vector.  To make the vertical dimensions of the graph smaller or to move the graph up or down, adjust the staring and ending y coordinates given in the third and fourth positions as below.

```r
# decrease horizontal span
par(fig=c(0, 1, .2, .8))

#open plot
plot(c(0,100), c(-1,1), type = "n", ylab = "", yaxt = "n", xlab = "")
points(sim.data[,1], replicate(8, 0), pch = 19, col = 1:8, cex = 1.5)
# add center reference line
abline(0,0)
legend("bottomright", fill = c(1:8), legend = c(1:8), ncol = 4)
```

<div style="text-align: center"><a href="http://www.programmingr.com/images/fig/fig2.jpg"><img height="300" title="fig thin" alt="fig thin" src="http://www.programmingr.com/images/fig/fig2.jpg"/></a></div>

It is possible to resize and move a single graph to any spatial orientation on the graphing device using the approach above.  Additionally, you can also use this method to add multiple graphs of various sizes to a single device:

```r
# place graph one in the bottom left
par(fig=c(0, .25, 0, .25), mar=c(2,.5,1,.5), mgp=c(0, 1, 0))

#open plot
plot(c(0,100), c(-1,1), type = "n", ylab = "", yaxt = "n", xlab = "")
points(sim.data[,1], replicate(8, 0), pch = 19, col = 1:8)
# add center reference line
abline(0,0)

# place graph two in the top right
# set graphing parameters for next plot and set new parameter to TRUE
par(fig=c(.75, 1, .75, 1), new = TRUE)

#open plot
plot(c(0,100), c(-1,1), type = "n", ylab = "", yaxt = "n", xlab = "")
points(sim.data[,2], replicate(8, 0), pch = 19, col = 1:8)
# add center reference line
abline(0,0)

# place main graph in the center
# set graphing parameters for next plot and set new parameter to TRUE
par(fig=c(.25, .75, .25, .75), new = TRUE)

#open plot
plot(c(0,100), c(-1,1), type = "n", ylab = "", yaxt = "n", xlab = "")
points(sim.data[,3], replicate(8, 0), pch = 19, col = 1:8, cex = 1.5)
# add center reference line
abline(0,0)
legend("bottomright", fill = c(1:8), legend = c(1:8), ncol = 4)
```

<div style="text-align: center"><a href="http://www.programmingr.com/images/fig/fig3.jpg"><img height="300" title="fig multiple" alt="fig multiple" src="http://www.programmingr.com/images/fig/fig3.jpg"/></a></div>

For simplicity I have mostly avoided labels and titles in these graphs; however they can be added and manipulated as they would be without the use of <code>fig</code> or <code>fin</code>.
