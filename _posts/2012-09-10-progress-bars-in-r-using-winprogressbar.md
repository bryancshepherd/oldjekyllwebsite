---
layout: post
title: Progress bars in R using winProgressBar
date: 2012-09-10 12:11
author: bryan
comments: true
categories: [Articles]
---
Using progress bars in R scripts can provide valuable timing feedback during development and additional polish to final products. <code>winProgressBar</code> and <code>setWinProgressBar</code> are the primary functions for creating progress bars in R.

Progress bars, and progress indicators in general, are relatively uncommon in R programming. This makes sense, as they can add bloat and, being design elements, they generally fall into the classification of "nice but not necessary". However, during development, especially when using loops, progress bars can a cleaner way of tracking loop progress than, for example, printing iteration numbers. And for programmers who prepare scripts or packages for non-programmers, they add feedback that users have come to expect from other software.

To add progress bars in R scripts use the <code>winProgressBar</code> and <code>setWinProgressBar</code> functions. For non-Windows users there's also a very similar Tcl/Tk version (<code>tkProgressBar</code> and <code>settkProgressbar</code>); depending on your current set up, you may need to install the <a href="http://cran.r-project.org/web/packages/tcltk2/index.html" title="Tck/tk R library">tcltk</a> library to use it.

Setting up a progress indicator to track the progress of a loop is very straightforward. First, initialize the display:

<code>pb <- winProgressBar(title="Example progress bar", label="0% done", min=0, max=100, initial=0)</code>

Use the <code>title</code> and <code>label</code> options to set the style of the display. The <code>min</code> and <code>max</code> options should use whatever values are most applicable to your task, but in most cases this will be displaying a percentage so a range of 0 to 100, starting at 0, makes sense.

After initializing the progress indicator, add your loop code and update the progress bar by calling the <code>setWinProgressBar</code> function at the end of each loop:

<code>for(i in 1:100) {
  Sys.sleep(0.1) # slow down the code for illustration purposes
  info <- sprintf("%d%% done", round((i/100)*100))
  setWinProgressBar(pb, i/(100)*100, label=info)
}

Once the loop is exited, close the progress bar window:

close(pb)</code>

Here is a snapshot of the progress indicator in action:

<a href="http://www.programmingr.com/wp-content/uploads/2012/09/winProgressBar.png"><img class="aligncenter size-full wp-image-615" title="winProgressBar" src="http://www.programmingr.com/wp-content/uploads/2012/09/winProgressBar.png" alt="Windows Progress Bar Example" width="400" height="178" /></a>

To track the progress of an entire script or program, you just need to place the initializing function (<code>winProgressBar</code>) at the beginning of the code and do updates via <code>setWinProgressBar</code> at key points within the flow of your program. For example, if the first task your code performs usually takes about 10% of the total time required, set the value to 10% after that task completes.

If a popup progress bar doesn't work for your task, there is a minimalist option, <code>txtProgressBar</code>, that by default draws a line in the console. It also allows for some fun customizations:

<p style="text-align: center;"><a href="http://www.programmingr.com/wp-content/uploads/2012/09/txtProgressBar1.png"><img class="size-full wp-image-620 aligncenter" title="txtProgressBar" src="http://www.programmingr.com/wp-content/uploads/2012/09/txtProgressBar1.png" alt="Text Progress Bar Example" width="670" height="78" /></a></p>
