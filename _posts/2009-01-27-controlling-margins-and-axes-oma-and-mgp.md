---
layout: post
title: Controlling margins and axes with oma and mgp
date: 2009-01-27 04:09
author: bryan
comments: true
categories: [Articles, Help]
noindex: true
---


When creating graphs, we're usually most concerned with what happens near the center of our displays, as this is where most of the important information is generally held.  But sometimes, either for aesthetics or clarity, we want to adjust what's outside of the box - in the margins, labels or tick marks.  The <code>par()</code> function offers several ways to do this and I'll discuss two that deal primarily with spatial orientation - rather than content - below.
<br />
<strong>The oma, omd, and omi options</strong>

To control the width of the outer margins of your graph (the empty sections outside of the axes and labels) use either the <code>oma</code>, <code>omd</code>, or <code>omi</code> option of the <code>par()</code> function.  All three of these options have the same effect and differ only in the units used to define the parameter.  <code>oma</code> defines the space in lines, <code>omd</code> as a fraction of the device region, and <code>omi</code> specifies the size in inches.  <code>oma</code> and <code>omi</code> take a four item vector where position one sets the bottom margin, position two the left margin, position three the top margin and position four the right margin.  <code>omd</code> uses a four item vector where positions one and three define, in percentages of the device region, the starting points of the x and y axes, respectively, while positions two and four define the end points.  Because these options all effect the same graph space, changing one also changes the remaining two.  A few examples of code and the charts they produce are shown below.  To help illustrate the different margin sizes, the blue area indicates the dimensions of the device display:

<code>
# generate some data
x<-log(seq(1:100))
</code>

<code>
# oma, omd, and omi defaults
par()
</code>

<code>
>...
>$oma
>[1] 0 0 0 0
>
>$omd
>[1] 0 1 0 1
>
>$omi
>[1] 0 0 0 0
>...
</code>

<code>
# plot using default margin settings
plot(x,pch=1, col = "red", ylab = "Y Label", xlab = "X Label")
title("Default")
</code>

<div style="text-align:center"><a href="http://www.programmingr.com/images/oma1.jpg"><img height=300 src="http://www.programmingr.com/images/oma1.jpg" alt="oma default" title="oma default" /></a></div>

<code>
# add four lines to bottom and top margins
par(oma = c(4, 0, 4, 0))
plot(x, pch=1, col = "red", ylab = "Y Label", xlab = "X Label")
title("oma = c(4, 0, 4, 0)")
</code>

<div style="text-align:center"><a href="http://www.programmingr.com/images/oma2.jpg"><img height=300 src="http://www.programmingr.com/images/oma2.jpg" alt="oma 2" title="oma 2" /></a></div>

<code>
# change via omd
par(omd = c(.15, .85, .15, .85))
plot(x, pch=1, col = "red", ylab = "Y Label", xlab = "X Label")
title("omd = c(.15, .85, .15, .85)")
</code>

<div style="text-align:center"><a href="http://www.programmingr.com/images/oma3.jpg"><img height=300 src="http://www.programmingr.com/images/oma3.jpg" alt="oma 3" title="oma 3" /></a></div>

<code>
# because oma, omd, and omi all affect the same graph space
# this doesn't make sense
par(omi = c(0, 0, 0, 0), omd = c(.10, .90, .10, .90))
</code>

<code>
# reset oma, omd, and omi to default by changing omi
par(omi = c(0, 0, 0, 0))
</code>
<br />
<strong>The mgp option</strong>

In addition to changing the margin size of your charts, you may also want to change the way axes and labels are spatially arranged. One method of doing so is the <code>mgp</code> parameter option.  The <code>mgp</code> setting is defined by a three item vector wherein the first value represents the distance of the axis labels or titles from the axes, the second value is the distance of the tick mark labels from the axes, and the third is the distance of the tick mark symbols from the axes.  As with the <code>oma</code> option discussed above, the distances are given in line widths.  The defaults for the <code>mgp</code> setting are <code>c(3, 1, 0)</code>.  The examples below illustrate the effects of changing the various <code>mgp</code> values.  <em>Note: the </em> <code>mgp.axis()</code> <em>function in the</em> Hmisc <em>package can be used to change these settings for each axis individually.</em>

<code>
# mgp default settings
plot(x, pch=1, col = "red", ylab = "Y Label", xlab = "X Label")
</code>

<div style="text-align:center"><a href="http://www.programmingr.com/images/mgp0.jpg"><img height=300 src="http://www.programmingr.com/images/mgp0.jpg" alt="mgp default" title="mgp default" /></a></div>

<code>
# move labels close to axes
par(mgp = c(0, 1, 0))
plot(x, pch=1, col = "red", ylab = "Y Label", xlab = "X Label")
</code>

<div style="text-align:center"><a href="http://www.programmingr.com/images/mgp1.jpg"><img height=300 src="http://www.programmingr.com/images/mgp1.jpg" alt="mgp move labels" title="mgp move labels" /></a></div>

<code>
# move tick labels out
par(mgp = c(0, 3, 0))
plot(x, pch=1, col = "red", ylab = "Y Label", xlab = "X Label")
</code>

<div style="text-align:center"><a href="http://www.programmingr.com/images/mgp2.jpg"><img height=300 src="http://www.programmingr.com/images/mgp2.jpg" alt="mgp move tick labels" title="mgp move tick labels" /></a></div>

<code>
# move tick lines out
par(mgp = c(0, 3, 2))
plot(x, pch=1, col = "red", ylab = "Y Label", xlab = "X Label")
</code>

<div style="text-align:center"><a href="http://www.programmingr.com/images/mgp3.jpg"><img height=300 src="http://www.programmingr.com/images/mgp3.jpg" alt="mgp move tick lines" title="mgp move tick lines" /></a></div>
<br />
<strong>Summary</strong>
The <code>oma, omd, omi,</code> and <code>mgp</code> parameter settings can be useful in defining and adjusting the outer regions of your charts.  To arrage and size multiple graphing areas you may also find other <code>par()</code> settings such as <code>fig</code>, <code>fin</code>, or <code>layout</code> helpful.




<br />
