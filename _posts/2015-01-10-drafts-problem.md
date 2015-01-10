---
layout: post
title: 'A small problem with Drafts'
date: 2015-01-10 13:50
categories: software
tags: 
	- markdown
---
All was going really well with *Drafts* until I used it to edit my main index.html file (now renamed to index.md, for no particular reason other than to remind myself that this isn't a pure html file).  This file contains a number of instances of things enclosed in double curly brackets (the ones that looks like this: {} ).  When I saved the file back in *Working Copy* I noticed the file was garbled.  Luckily I hadn't committed the file so I was able to revert it.

The problem is a result of *Drafts* using double curly brackets to denote the need to URL encode everything between them.  I've emailed the supplier (ironically, given the name of this blog, Agile Tortoise) to see if they have a solution.  For the time being I've switched to Textastic for editing files containing double curly brackets, but stuck with *Drafts* for posts.