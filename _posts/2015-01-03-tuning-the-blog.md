---
layout: post
title: "Tuning (part 1 - adding the date and time to posts)"
date: {}
published: true
---

Although the instructions for setting up the blog mentioned in my prevous post were excellent, a few tweaks were needed behind the scenes.

First up, the date and time of posts.  As forked from jekyll-now, the date of the post appears at the bottom of the page containing the whole post, but the time doesn't. In addition, neither the daate nor the time appears on the index page.

###Adding the time to the post page
Adding the hours and minutes to the date at the bottom of the post at first face seems very simple.  Each post has the template in _layouts/post.html.  All that appears to beneeded is to change the line in _layouts/post.html

    Written on {{ page.date | date: "%B %e, %Y" }}

to  

    Written on {{ page.date | date: "%B %e, %Y at %R" }}
    

(%R is the Unix date formatter for 24 hour clock HH:MM formatting).  However all this does is to add "at 00:00" to the date of the post.  This is because the date of the post is taken from the the start of the filename of the post and there is no way to specify the hours and minutes in the file name.

### Forcing a time on the post
To give a time to a post it is necessary to add a line to the Front Matter of a post (i.e the bit that lies between the two sets of three hyphens at the beginning of the post.  The line to be added is in the format:

date: YYYY-MM-DD HH:MM:SS
e.g.

date: 2015-01-03 03:10:00

###Adding the time at the top of each post in the index page.
The index page is contained in index.html.  This contains a loop which Jekyll iterates through to include the summaries of each post.  To add the date and time to each post add

	<div class="read-more">
    	{{ post.date | date: "%B %e, %Y %R" }}
    </div>
        
just above

	<div class="entry">
        {{ post.content | truncatewords:40}}
    </div>
    
The class of "read-more" is just a temporary fix until I have time to trawl through styles.css to find a more appropriate one.
