---
layout: post
title: R Helper Functions
date: 2012-09-25 23:55
author: bryan
comments: true
categories: [Articles]
---
If you do a lot of R programming, you probably have a list of R helper functions set aside in a script that you include on R startup or at the top of your code. In some cases helper functions add capabilities that aren't otherwise available. In other cases, they replicate functionality that is available elsewhere without loading unnecessary components. Below I present two of my most frequently used data manipulation helper functions as examples. 

<h3>Descriptives R Helpler Function</h3>
[sourcecode language="r"]
# Display some basic descriptives
descs &lt;- function (x) {
  if(!hidetables) {
    if(length(unique(x))&gt;30) {
      print(&quot;Summary results:&quot;)
      print(summary(x))
      print(&quot;&quot;)
      print(&quot;Number of categories is greater than 30, table not produced&quot;)
    } else {
      print(&quot;Summary results:&quot;)
      print(summary(x))
      print(&quot;&quot;)
      print(&quot;Table results:&quot;)
      print(table(x, useNA=&quot;always&quot;))
    }
   
  } else {
    print(&quot;Tables are hidden&quot;)
  }
}


# Set hide tables to true to hide tables
hidetables &lt;- FALSE
[/sourcecode]

<h3>Dummy Variable R Helper Function</h3>
[sourcecode language="r"]
# Create dummy variables for each level of a categorical variable
createDummies &lt;- function(x, df, keepNAs = TRUE) {
  for (i in seq(1, length(unique(df[, x])))) {
    if(keepNAs) {
      df[, paste(x,&quot;.&quot;, i, sep = &quot;&quot;)] &lt;- ifelse(df[, x] != i, 0, 1)
    } else {
      df[, paste(x,&quot;.&quot;, i, sep = &quot;&quot;)] &lt;- ifelse(df[, x] != i | is.na(df[, x]) , 0, 1)     
    }
  }
  df
}
[/sourcecode]

The Descriptives R Helper Function produces a summary or table of the passed variable/object; it uses the number of unique values to determine whether to call just the <code>summary()</code> or <code>summary()</code> and <code>table()</code> functions. It also includes NAs by default in the tables (one of <code>table()</code>'s biggest annoyances). Once the exploratory and data manipulation work is done, all output from this function can be suppressed by setting the <code>hidetables</code> object to TRUE. 

The Dummy Variable R Helper Function creates indicator variables from all values of a variable. Based on experience, I avoid the factor object as much as possible and this approach allows me to quickly create indicators that can be used in any way I want. 

If you don't have a programming background or are just beginning with R, you might not have had time to realize the benefit of helper functions or identify the tasks you do repetitively, but it's worthwhile to give the issue some consideration. Helper functions can be exceptionally useful for saving time on repetitive tasks or facilitating your work. They're so useful in fact, that there is a special ProgrammingR event planned around helper functions coming soon. For the more experienced R programmers out there, make a mental note of the most useful helper functions you've written in the past. That list will come in handy in the near future!

