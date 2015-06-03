---
layout: post
title: Installing quantstrat from R-forge and source
date: 2012-01-10 22:52
author: bryan
comments: true
categories: [Articles]
---


R is used extensively in the financial industry; many of my recent clients have been working in or developing products for the financial sector. Some common applications are to use R to analyze market data and evaluate quantitative trading strategies. Custom solutions are almost always the best way to do this, but the <code>quantstrat</code> package can make it easy to quickly get a high-level understanding of a strategy's potential. However, <code>quantstrat</code> is still under development, and this, combined with a lack of documentation and the complex nature of the tasks involved, make it difficult to work with. This article addresses one of the most basic issues with <code>quantstrat</code> - getting it installed. <code>quantstrat</code> and it's required packages currently aren't available on CRAN - you have to get them from R-forge. As a result, the installation is slightly less straightforward than other packages and provides an opportunity to discuss how to install packages from R-forge and locally from source. Although this article focuses on installing <code>quantstrat</code>, these instructions will help with any R-package that you need to build from source.
</br>
If you're installing from R-forge, the process is only moderately different than installing from CRAN; simply change the <code>install.packages</code> command to point to the R-forge repository:

<code>
install.packages("FinancialInstrument", repos="http://R-Forge.R-project.org")</code>

<code>install.packages("blotter", repos="http://R-Forge.R-project.org")</code>

<code>install.packages("quantstrat", repos="http://R-Forge.R-project.org")</code>
</br>
Since the <code>FinancialInstrument</code> and <code>blotter</code> packages are dependencies for <code>quantstrat</code>, you can download and install all three at once with just the last line.
</br>
In some cases, you may need to build the packages yourself. You'll need to set your system up to compile R source code if it isn't already. To do so, follow steps 1-3 below. If your system is already set up to compile R source code, you can skip to step 4.
</br>
# 1) Install <code>Rtools</code> package (must be done manually from http://www.murdoch-sutherland.com/Rtools/

# 2) Install LaTex from www.miktex.org/

# 3) Install InnoSetup http://www.jrsoftware.org/

# 4) Download the three package source files available from R-forge http://r-forge.r-project.org/R/?group_id=316

# 5) Install the packages using the commands below (substituting the appropriate version numbers):

<code>
install.packages("C:/yourpath/FinancialInstrument_0.9.18.tar.gz", repos = NULL, type="source")</code>

<code>install.packages("C:/yourpath/blotter_0.8.4.tar.gz", repos = NULL, type="source")</code>

<code>install.packages("C:/yourpath/quantstrat_0.6.1.tar.gz", repos = NULL, type="source")</code>
</br>
Note that these directions are relevant until the packages are available on CRAN, after which, you'll be able to download and install them like any other package (I'll make a note on this post once that happens). Also note that since these packages are under heavy development, you'll want to update them often.



<br />
