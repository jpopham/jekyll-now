---
layout: post
date: 2015-01-05 22:20:00
title: Tuning the blog - 4) creating category and tag pages
category: blogging
tags: 
    - jekyll
    - categories
    - tags
---
 
This looked like it was going to be straightforward, but it took me ages to sort this out.   Because of the nature of what Jekyll plugins are allowed in GitHub Pages, it is not possible to create tag and category pages.  [The solution from Stephan Groß](http://www.minddust.com/post/tags-and-categories-on-github-pages/) - slightly adapted by me - is to create a dummy file for each tag in the folder /blog/tags with a name like *tagname*.md with an entry like this example for the tag GitHub-pages:

