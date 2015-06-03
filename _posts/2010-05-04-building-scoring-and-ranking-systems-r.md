---
layout: post
title: Building Scoring and Ranking Systems in R
date: 2010-05-04 00:17
author: bryan
comments: true
categories: [Articles]
---


<em>This guest article was written by <a href="http://www.amazon.com/Enhanced-Indexing-Strategies-Utilizing-Performance/dp/0470259256" title = "Enhanced Indexing Strategies">author</a> and <a href="http://www.yates-mgt.com" title = "Yates Management">consultant</a> Tristan Yates (see his bio below). It emphasizes R's data object manipulation and scoring capabilities via a detailed financial analysis example.</em>
<br />
Scoring and ranking systems are extremely valuable management tools.  They can be used to predict the future, make decisions, and improve behavior – sometimes all of the above.  Think about how the simple grade point average is used to motivate students and make admissions decisions.
<br />
R is a great tool for building scoring and ranking systems.  It’s a programming language designed for analytical applications with statistical capabilities.  The capability to store and manipulate data in list and table form is built right into the core language.
&nbsp;

But there’s also some validity to the criticism that R provides too many choices and not enough guidance.  The best solution is to share your work with others, so in this article we show a basic design workflow for one such scoring and ranking system that can be applied to many different types of projects.
<br />
<strong>The Approach</strong>
For a recent article in Active Trader, we analyzed the risk of different market sectors over time with the objective of building less volatile investment portfolios.  Every month, we scored each sector by its risk, using its individual ranking within the overall population, and used these rankings to predict future risk.
<br />
Here’s the workflow we used, and it can be applied to any scoring and ranking system that must perform over time (which most do):

<ol>
<li>Load in the historical data for every month and ticker symbol.</li>
<li>Load in the performance data for every month and ticker symbol.</li>
<li>Generate scores and rankings for every month and ticker symbol based upon their relative position in the population on various indicators.</li>
<li>Review the summary and look for trends.</li>
</ol>

In these steps, we used four data frames, as shown below:
<br >
<table>
<tr>
<th>Name</th>
<th>Contents</th>
</tr>
<tr>
<td>my.history</td>
<td>historical data</td>
</tr>
<tr>
<td>my.scores</td>
<td>scoring components, total scores, rankings</td>
</tr>
<tr>
<td>my.perf</td>
<td>performance data</td>
</tr>
<tr>
<td>my.summary&nbsp;&nbsp;</td>
<td>summary or aggregate data</td>
</tr>
</table>
&nbsp;
One of my habits is to prefix my variables – it helps prevent collisions in the R namespace.
<br />
Some people put all of their data in the same data.frame, but keeping it separate reinforces good work habits.  First, the historical data and performance data should never be manipulated, so it makes sense to keep it away from the more volatile scoring data.
<br />
Second, it helps draw a clear distinction between what we know at one point in time – which is historical data - and what we will know later – which is the performance data.  That’s absolutely necessary for the integrity of the scoring system.
<br />
My.history, my.scores, and my.perf are organized like this:
<br >
<table>
<tr>
<th>&nbsp;yrmo&nbsp;</th>
<th>&nbsp;ticker&nbsp;</th>
<th>&nbsp;&nbsp;var1&nbsp;&nbsp;</th>
<th>&nbsp;&nbsp;var2&nbsp;&nbsp;</th>
<th>&nbsp;&nbsp;etc...&nbsp;&nbsp;</th>
</tr>
<tr>
<td>200401</td>
<td>&nbsp;&nbsp;XLF</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
</tr>
<tr>
<td>200401</td>
<td>&nbsp;&nbsp;XLB</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
</tr>
<tr>
<td>etc...</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
</tr>
</table>
&nbsp;
yrmo is the year and month and ticker is the item to be scored.   We maintain our own list of dates (in yrmo format) and items in my.dates and my.items.  Both these lists are called drivers, as they can help iterate through the data.frame, and we also have a useful data.frame called my.driver which has only the yrmo and ticker.
<br />
One trick – we keep the order the same for all of these data.frames.  That way we can use indexes on one to query another.  For example, this works just fine:
<br />
<code>&nbsp;&nbsp;Vol.spy <- my.history$vol.1[my.score$rank==1]</code>
<br />
<strong>Loading Data</strong>
First, we get our driver lists and my.driver data.frame set up. We select our date range and items from our population, and then build a data.frame using the rbind command.
<br />
<code>&nbsp;&nbsp;#this is based on previous analysis</code>
<code>&nbsp;&nbsp;my.dates <- m2$yrmo[13:(length(m2$yrmo)-3)]</code>
<code>&nbsp;&nbsp;my.items <- ticker.list[2:10]</code>
<br />
<code>&nbsp;&nbsp;#now the driver</code>
<code>&nbsp;&nbsp;my.driver <- data.frame()</code>
<code>&nbsp;&nbsp;for (z.date in my.dates) {</code>
<code>&nbsp;&nbsp;&nbsp;&nbsp;my.driver <- rbind(my.driver,data.frame(ticker=my.items,yrmo=z.date))</code>
<code>&nbsp;&nbsp;}</code>
<br />
Next, let’s get our historical and performance data.  We can make a function that can be called once for each row in my.driver that then loads any data needed.
<br />
<code>&nbsp;&nbsp;my.seq <- 1:length(my.driver[,1])</code>
<code>&nbsp;&nbsp;my.history <- data.frame(ticker=my.driver$ticker,yrmo=my.driver$yrmo,</code>
<code>&nbsp;&nbsp;&nbsp;&nbsp;vol.1=sapply(my.seq,calc.sd.fn,-1,-1))</code>
<br />
Each variable can be loaded by a function called with the sapply command.  The calc.sd.fn function first looks up the ticker and yrmo from my.driver using the index provided, and then returns the data.  You would have one function for each indicator that you want to load.  My.perf, which holds the performance data, is built in the exact same way.
<br />
The rbind command is slow unfortunately, but loading the historical and performance data only needs to be done once.
<br />
<strong>Scoring The Data</strong>
This is where R really shines.  Let’s look at the highest level first.
<br />
<code>&nbsp;&nbsp;my.scores <- data.frame()</code>
<code>&nbsp;&nbsp;for (z.yrmo in my.dates) {</code>
<code>&nbsp;&nbsp;&nbsp;&nbsp;my.scores <- rbind(my.scores,calc.scores.fn(z.yrmo))</code>
<code>&nbsp;&nbsp;&nbsp;&nbsp;}</code>
<code>&nbsp;&nbsp;my.scores$p.tot <- (my.scores$p.vol.1)</code>
<br />
Every indicator gets its own score, and then that can be combined in any conceivable way to create total score.  In this very simple case, we’re only scoring one indicator, so we just use that score as the total score.
<br />
For more complex applications, the ideal strategy is to use multiple indicators from multiple data sources to tell the same story.  Ignore those who advocate reducing variables and cross-correlations.  Instead, think like a doctor that wants to run just one more test and get that independent confirmation.
<br />
Now the calc functions:
<br />
<code>&nbsp;&nbsp;scaled.score.fn <- function(z.raw)</code>
<code>&nbsp;&nbsp;&nbsp;&nbsp;{pnorm(z.raw,mean(z.raw),sd(z.raw))*100}</code>
<code>&nbsp;&nbsp;scaled.rank.fn <- function(z.raw) {rank(z.raw)}</code>
<br />
<code>&nbsp;&nbsp;calc.scores.fn <- function(z.yrmo) {</code>
<code>&nbsp;&nbsp;&nbsp;&nbsp;z.df <- my.history[my.history$yrmo==z.yrmo,]</code>
<code>&nbsp;&nbsp;&nbsp;&nbsp;z.scores <- data.frame(ticker=z.df$ticker,yrmo=z.df$yrmo,</code>
<code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;p.vol.1=scaled.score.fn(z.df$vol.1),r.vol.1=scaled.rank.fn(z.df$vol.1))</code>
<code>&nbsp;&nbsp;&nbsp;&nbsp;z.scores</code>
<code>&nbsp;&nbsp;&nbsp;&nbsp;}</code>
<br />
The calc.scores.fn function queries the data.frame to pull the population data for just a single point in time.  Then, each indicator is passed to the scaled.score.fn and scaled.rank.fn function, returning a list of scores and ranks.
<br />
Here, we use the pnorm function to calculate a statistical Z-score, which is a good practice for ensuring that a scoring system isn’t dominated by a single indicator.
<br />
<strong>Checking the Scores</strong>
At this point, we create a new data.frame for summary analysis. We use the always useful and always confusing aggregate function and combine by rank.  Notice how we easily we can combine data from my.history, my.scores and my.perf.
<br />
<code>&nbsp;&nbsp;data.frame(rank=1:9,p.tot=aggregate(my.scores$p.tot,</code>
<code>&nbsp;&nbsp;&nbsp;&nbsp;list(rank=my.scores$r.vol.1),mean)$x,ret.1=aggregate(my.perf$ret.1,</code>
<code>&nbsp;&nbsp;&nbsp;&nbsp;list(rank=my.scores$r.vol.1),mean)$x,sd.1=aggregate(my.perf$ret.1,</code>
<code>&nbsp;&nbsp;&nbsp;&nbsp;list(rank=my.scores$r.vol.1),sd)$x,vol.1=aggregate(my.history$vol.1,</code>
<code>&nbsp;&nbsp;&nbsp;&nbsp;list(rank=my.scores$r.vol.1),mean)$x,vol.p1=aggregate(my.history$vol.1,</code>
<code>&nbsp;&nbsp;&nbsp;&nbsp;list(rank=my.scores$r.vol.1),mean)$x)</code>
<br />
Here’s the result.  We could check plots or correlations, but the trend – higher relative volatility in the past (vol.p1, p.tot) is more likely to mean higher relative volatility in the future (vol.1, sd.1) - is crystal clear.
<br >
<table>
<tr>
<th><FONT FACE='Courier' SIZE=1>rank&nbsp;</FONT></th>
<th><FONT FACE='Courier' SIZE=1>p.tot&nbsp;<FONT FACE='Courier' SIZE=1></th>
<th><FONT FACE='Courier' SIZE=1>ret.1&nbsp;&nbsp;<FONT FACE='Courier' SIZE=1></th>
<th><FONT FACE='Courier' SIZE=1>sd.1 &nbsp;&nbsp;<FONT FACE='Courier' SIZE=1></th>
<th><FONT FACE='Courier' SIZE=1>vol.1&nbsp;&nbsp;<FONT FACE='Courier' SIZE=1></th>
<th><FONT FACE='Courier' SIZE=1>vol.p1&nbsp;&nbsp;<FONT FACE='Courier' SIZE=1></th>
</tr>
<tr>	<td><FONT FACE='Courier' SIZE=1>1</FONT></td>	<td><FONT FACE='Courier' SIZE=1>12.1</FONT></td>	<td><FONT FACE='Courier' SIZE=1>0.131</FONT></td>	<td><FONT FACE='Courier' SIZE=1>4.03</FONT></td>	<td><FONT FACE='Courier' SIZE=1>16.5</FONT></td>	<td><FONT FACE='Courier' SIZE=1>13.8</FONT></td>	</tr>
<tr>	<td><FONT FACE='Courier' SIZE=1>2</FONT></td>	<td><FONT FACE='Courier' SIZE=1>19.4</FONT></td>	<td><FONT FACE='Courier' SIZE=1>0.0872</FONT></td>	<td><FONT FACE='Courier' SIZE=1>4.82</FONT></td>	<td><FONT FACE='Courier' SIZE=1>16.6</FONT></td>	<td><FONT FACE='Courier' SIZE=1>16.1</FONT></td>	</tr>
<tr>	<td><FONT FACE='Courier' SIZE=1>3</FONT></td>	<td><FONT FACE='Courier' SIZE=1>27.1</FONT></td>	<td><FONT FACE='Courier' SIZE=1>0.2474</FONT></td>	<td><FONT FACE='Courier' SIZE=1>4.96</FONT></td>	<td><FONT FACE='Courier' SIZE=1>20.1</FONT></td>	<td><FONT FACE='Courier' SIZE=1>18</FONT></td>	</tr>
<tr>	<td><FONT FACE='Courier' SIZE=1>4</FONT></td>	<td><FONT FACE='Courier' SIZE=1>35.6</FONT></td>	<td><FONT FACE='Courier' SIZE=1>0.4247</FONT></td>	<td><FONT FACE='Courier' SIZE=1>5.31</FONT></td>	<td><FONT FACE='Courier' SIZE=1>20.9</FONT></td>	<td><FONT FACE='Courier' SIZE=1>19.9</FONT></td>	</tr>
<tr>	<td><FONT FACE='Courier' SIZE=1>5</FONT></td>	<td><FONT FACE='Courier' SIZE=1>44.9</FONT></td>	<td><FONT FACE='Courier' SIZE=1>0.6865</FONT></td>	<td><FONT FACE='Courier' SIZE=1>5.98</FONT></td>	<td><FONT FACE='Courier' SIZE=1>22.1</FONT></td>	<td><FONT FACE='Courier' SIZE=1>21.7</FONT></td>	</tr>
<tr>	<td><FONT FACE='Courier' SIZE=1>6</FONT></td>	<td><FONT FACE='Courier' SIZE=1>53</FONT></td>	<td><FONT FACE='Courier' SIZE=1>0.3235</FONT></td>	<td><FONT FACE='Courier' SIZE=1>5.84</FONT></td>	<td><FONT FACE='Courier' SIZE=1>21.5</FONT></td>	<td><FONT FACE='Courier' SIZE=1>23.2</FONT></td>	</tr>
<tr>	<td><FONT FACE='Courier' SIZE=1>7</FONT></td>	<td><FONT FACE='Courier' SIZE=1>65.1</FONT></td>	<td><FONT FACE='Courier' SIZE=1>1.019</FONT></td>	<td><FONT FACE='Courier' SIZE=1>5.86</FONT></td>	<td><FONT FACE='Courier' SIZE=1>24.6</FONT></td>	<td><FONT FACE='Courier' SIZE=1>25.4</FONT></td>	</tr>
<tr>	<td><FONT FACE='Courier' SIZE=1>8</FONT></td>	<td><FONT FACE='Courier' SIZE=1>78</FONT></td>	<td><FONT FACE='Courier' SIZE=1>0.7276</FONT></td>	<td><FONT FACE='Courier' SIZE=1>6.04</FONT></td>	<td><FONT FACE='Courier' SIZE=1>26.9</FONT></td>	<td><FONT FACE='Courier' SIZE=1>28.4</FONT></td>	</tr>
<tr>	<td><FONT FACE='Courier' SIZE=1>9</FONT></td>	<td><FONT FACE='Courier' SIZE=1>96.4</FONT></td>	<td><FONT FACE='Courier' SIZE=1>0.0837</FONT></td>	<td><FONT FACE='Courier' SIZE=1>9.34</FONT></td>	<td><FONT FACE='Courier' SIZE=1>35.2</FONT></td>	<td><FONT FACE='Courier' SIZE=1>38.3</FONT></td>	</tr>
</table>
&nbsp;
In the case of our analysis, the scores aren’t really necessary – we’re only ranking nine items every month.  If we did have a larger population, we could use code like this to create subgroups (six groups shown here), and then use the above aggregate function with the new my.scores$group variable.
<br />
<code>&nbsp;&nbsp;my.scores$group <- cut(my.scores$p.tot,</code>
<code>&nbsp;&nbsp;&nbsp;&nbsp;breaks=quantile(my.scores$p.tot,(0:6)/6),include.lowest=TRUE,labels=1:6)</code>
<br />
<strong>Wrap-up</strong>
We ultimately only ended up scoring one variable, but it’s pretty easy to see how this framework could be expanded to dozens or more.  Even so, it’s an easy system to describe – we grade each item by its ranking within the population.  People don’t trust scoring systems that can’t be easily explained, and with good reason.
<br />
There’s not a lot of code here, and that’s a testimony to R’s capabilities.  A lot of housekeeping work is done for you, and the list operations eliminate confusing nested loops.  It can be a real luxury to program in R after dealing with some other “higher level” language.
<br />
We hope you find this useful and encourage you to share your own solutions as well.
<br />
<em>Tristan Yates is the Executive Director of <a href="http://www.yates-mgt.com" title = "Yates Management">Yates Management</a>, a management and analytical consulting firm serving financial and military clients.  He is also the author of <a href="http://www.amazon.com/Enhanced-Indexing-Strategies-Utilizing-Performance/dp/0470259256" title = "Enhanced Indexing Strategies">Enhanced Indexing Strategies</a> and his writing and research have appeared in publications including the Wall Street Journal and Forbes/Investopedia.</em>



<br />
