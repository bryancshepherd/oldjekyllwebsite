---
layout: post
title: Calling Python from R with rPython
date: 2014-01-13 19:23
author: bryan
comments: true
categories: [Articles]
---

Python has generated a good bit of buzz over the past year as an alternative to R. Personal biases aside, an expert makes the best use of the available tools, and sometimes Python is better suited to a task. As a case in point, I recently wanted to pull data via the Reddit API. There isn't an R package that provides easy access to the Reddit API, but there is a very well designed and documented Python module called <a href="https://praw.readthedocs.org/en/latest/" title="Python Reddit API Wrapper">PRAW </a>(or, the Python Reddit API Wrapper). Using this module I was able to develop a Python-based solution to get and analyze the data I needed without too much trouble. 

However, I prefer working in R, so I was glad to discover the <a href="http://cran.r-project.org/web/packages/rPython/index.html" title="rPython">rPython</a> package, which enables calling Python scripts from R. After finding rPython, I was able to rewrite my purely Python script as a primarily R-based program. 

If you want to use rPython there are a couple of prerequisites you'll need to address if you haven't already. No surprise, you'll need to have Python installed. After that, you'll need to install the PRAW module via <code>pip install praw</code>. Finally, install the rPython package from CRAN. (But see the note below first if you're on Windows.)

After you've completed those steps, it's as easy as writing your Python script and adding a line or two to your R code.

First create a Python script that imports the praw module and does the first data call:

```python
import praw

# Set the user agent information
# IMPORTANT: Change this if you borrow this code. Reddit has very strong 
# guidelines about how to report user agent information 
r = praw.Reddit('Check New Articles script based on code by ProgrammingR.com')
   
# Create a (lazy) generator that will get the data when we call it below
new_subs = r.get_new(limit=100)

# Get the data and put it into a usable format
new_subs=[str(x) for x in new_subs]
```

Since the Python session is persistent, we can also create a shorter Python script that we can use to fetch updated data without reimporting the praw module

```python
# Create a (lazy) generator that will get the data when we call it below
new_subs = r.get_new(limit=100)

# Get the data and create a list of strings
new_subs=[str(x) for x in new_subs]
```

Finally, some R code that calls the Python script and gets the data from the Python variables we create:

```r
library(rPython)

# Load/run the main Python script
python.load(&quot;GetNewRedditSubmissions.py&quot;)

# Get the variable
new_subs_data &lt;- python.get(&quot;new_subs&quot;)

# Load/run re-fecth script
python.load(&quot;RefreshNewSubs.py&quot;)

# Get the updated variable
new_subs_data &lt;- python.get(&quot;new_subs&quot;)

head(new_subs_data)
```

#### A few final notes:

* The main drawback to the rPython package is that it currently doesn't run on Windows. The developer (Carlos J. Gil Bellosta) is working to fix this, though. If that wrinkle gets resolved, I can see this being a very popular package.
* You can use RStudio to write your Python programs, which is easier than switching to another IDE for simple scripts. However, it causes an issue with EOL characters. Namely, you need to add a blank line at the end of each .py file to get it to load properly.
* The Python session rPython initiates is associated with the R session. Any Python modules you load or variables you create will be available until you remove them or close the R session.



