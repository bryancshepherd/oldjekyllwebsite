---
layout: post
title: Getting LaTeX to Display in a GitHub-based Jekyll Blog
date: 2016-1-14
author: bryan
comments: true
categories: [Articles]
noindex: false
published: false
---


1. Add the appropriate source to your header. Probably in `_layouts/default.html`:

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

2. Make sure your markdown parser is `redcarpet`. kramdown may also work, but may need additional work.

3. Wrap your LaTeX in divs to keep the markdown parser from breaking it.
