---
layout: post
title: Displaying LaTeX in Atom's Live Markdown Preview
date: 2016-01-09
author: bryan
comments: true
categories: [Articles]
noindex: false
published: true
---

#### The Atom editor
I've been using the [Atom editor](https://atom.io/) for a few months and I am very pleased with it overall. It doesn't beat well-developed, language-specific IDEs such as R-Studio or iPython Notebooks, but it works very well for general purpose editing and in cases where there isn't a comprehensive IDE available (e.g., Julia). It has an active development community and that translates into a lot of extensibility. It has broad support for syntax highlighting and markdown editing, including a very helpful live markdown preview. Both of those features make writing markdown with embedded code very easy.

However, when I recently tried to include some LaTeX in one of my markdown files I hit a roadblock. Atom doesn't support LaTeX in markdown previews out of the box and the solution took a little longer to find than I think it should have. Ergo, I'm posting this information in hopes that it might help others find the solution faster.

#### Defining the parameters
To be clear, this isn't about rendering `.tex` files in Atom. if you're looking for a package to build and display pure `.tex` files, that's fairly straightforward - start with the [latex package](https://atom.io/packages/latex).

The goal here is to display LaTeX blocks and inline LaTeX in markdown, i.e., `.md` file previews. Being clear on that distinction can save you a good bit of time, as getting the latex package set up can take a while and it's not required at all for markdown files. If you start there, you'll waste an hour or two of your valuable time.  

#### Markdown Preview Plus
The key to getting LaTeX to display in Atom markdown previews is the [Markdown Preview Plus](https://atom.io/packages/markdown-preview-plus) package. It is a fork of the core markdown preview package that adds several features, one of which is LaTeX support (via MathJax*).

What this package does can be a [little unclear](https://github.com/Galadirith/markdown-preview-plus/issues/5) from the description, with some people assuming it provides support for `.tex` files (it does not). But I assure you, if you're looking to preview LaTeX in your `.md` files this is what you want.

The install is simple:

`apm install markdown-preview-plus`

then disable the markdown preview package that ships with Atom (`markdown-preview`). Details on the installation process are [here](https://github.com/Galadirith/markdown-preview-plus/blob/v2.2.2/docs/installation.md), if you need them.

After that, you need to know how to use the package. The docs are pretty clear that `ctrl-shift-m` opens a preview and `ctrl-shift-x` turns on math support, but that's only half the battle. If you try to use straight LaTeX from there you'll get no love. The key is in [this doc](https://github.com/Galadirith/markdown-preview-plus/blob/master/docs/math.md) - namely, you need to add `$$` to delineate your math blocks and `$` for inline math.

#### And you're done
With that information in hand, you should be good to go, hopefully in less time than it would have taken otherwise.  

---
\* The [distinction between LaTeX and MathJax](http://meta.math.stackexchange.com/questions/6177/what-is-the-relation-between-latex-and-mathjax) may or may not be important for your purposes.
