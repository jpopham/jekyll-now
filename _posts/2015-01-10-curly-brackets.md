---
layout: post
title: 'What are these curly brackets?'
date: 2015-01-10 14:01:47
category: software
tags:
    - liquid
    - jekyll
    - markdown
---
I mentioned in my previous post that *Drafts* was "corrupting" files containing curly brackets.   These brackets are part of the [Liquid templat img language](http://liquidmarkup.org) which is used by Jekyll to turn templates into html.

>There are two types of markup in Liquid: Output and Tag.

>Output markup (which may resolve to text) is surrounded by
>&#x007b;&#x007b;matched pairs of curly brackets (ie, braces) }}
>Tag markup (which cannot resolve to text) is surrounded by
>&#x007b;% matched pairs of curly brackets and percent signs %}

Further concise explanation can be [found here](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers).