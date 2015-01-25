---
layout: post
title: "Passing Validation"
description:"Testing for valid HTML and CSS"
date: 2015-01-16 16:30:00 
category: web
tags: [css, html, validation]
---
It occurred to me recently that I should make sure that this blog's HTML and CSS pass HTML5 and CSS3 validation.

## HTML5 validation
On passing the main index through the [HTML checking tool](http://validator.w3.org), I was astonished to find that the main index.html that it came up with over forty errors and a similar number of warnings.  Most of these related to wrongly closed `<p>` tags.  A quick look at the HTML of the page showed this to be the case.  After a bit of head-scratching I pin pointed this to the us eof `{{ post.content | truncatewords:100}}` (which outputs the first 100 characters of a post) in the index.html template.  This was naturally cutting things off mid-paragraph and thus leaving `<p>` unclosed.  The solution to this was to replace it with `{{ post.excerpt}}` which outputs the first paragraph of the post.

This left me with a lot of strange errors arising from the svg images that were included in the footer.  There were four problems which I fixed by amending the svgs
- duplicate ids.  Solved by prefixing the ids with the filename (e.g. changing `id="Path"` to `id="facebook-path"`.  In some cases an id appeared more than once in the same svg; in those cases I added a numerical suffix.
- the tags `xmlns:sketch...`, `sketch:type...` and `xmlns:svg...` were objected to by the validator.  Solved by removing them.

Not strictly a validation issue, but I noticed that the HTML contained a lot  of blank lines.  I tracked this down to my nicely formatted Liquid in the template. Each time it went round a loop, it inserted the blank line that was present in the template.  I solved that by removing the blank lines from the template, but I can't help thinking that Jekyll shouldn't be doing this.

## CSS validation
Passing the main index through the [CSS checking tool] gave over twenty errors, this was mainly down to me thinking that `center` was a valid value for `float` which it isn't.  There were (and still are) a number of warnings about `backgrpund-color` and `border-color` having the same value.  Finally there's an error saying that `::selection` isn't valid.  This is strictly speaking true as it isn't part of the CSS3 specification, however it is supported by all the major browsers, so no real problem

