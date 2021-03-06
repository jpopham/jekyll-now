---
layout: post
title: "Tuning the blog - 1) adding the date and time to posts"
date: 2015-01-03 13:50:00
description: "Adding the date and time to GitHub Pages blog posts"
category: blogging
tags:
- jekyll
- frontmatter
---

Although the instructions for setting up the blog mentioned in my prevous post were excellent, a few tweaks were needed behind the scenes.

First up, the date and time of posts.  As forked from jekyll-now, the date of the post appears at the bottom of the page containing the whole post, but the time doesn't. In addition, neither the daate nor the time appears on the index page.

###Adding the time to the post page
Adding the hours and minutes to the date at the bottom of the post at first face seems very simple.  Each post has the template in _layouts/post.html.  All that appears to be needed is to change the line in _layouts/post.html

<div class="falseCode">Written on &#x007b;&#x007b;  page.date | date: "%B %e, %Y" &#x007d;&#x007d;</div>

as far as I can tell this line is taking the posts date and piping it through a date formatter.  This needs to be changed to:  

<div class="falseCode">Written on &#x007b;&#x007b; page.date | date: "%B %e, %Y at %R" &#x007d;&#x007d;</div> 

    

(%R is the Unix date formatter for 24 hour clock HH:MM formatting).  However all this does is to add "at 00:00" to the date of the post.  This is because the date of the post is taken from the the start of the filename of the post and there is no way to specify the hours and minutes in the file name.

### Forcing a time on the post
To give a time to a post it is necessary to add a line to the Front Matter of a post (i.e the bit that lies between the two sets of three hyphens at the beginning of the post.  The line to be added is in the format:

    date: YYYY-MM-DD HH:MM:SS

e.g.

    date: 2015-01-03 03:10:00

###Adding the time at the top of each post in the index page.
The index page is contained in index.html.  This contains a loop which Jekyll iterates through to include the summaries of each post.  To add the date and time to each post add

    <div class="read-more">
    
   <div class="falseCode"><pre>    &#x007b;&#x007b;post.date | date: "%B %e, %Y %R" &#x007d;&#x007d;</pre>

    </div>
        
just above

    <div class="entry">

<div class="falseCode"><pre>    &#x007b;&#x007b;post.content | truncatewords:40&#x007d;&#x007d;</pre></div>

    </div>
    
The class of "read-more" is just a temporary fix until I have time to trawl through styles.css to find a more appropriate one.

###An aside about displaying the code blocks in this post
The normal way to enter a code block in Markdown (I learned today) is to indent the lines by four or more spaces or at least one tab.  This indeed works fine and was used for some of this post - however this renders <span class="falseCode">&#x007b;&#x007b;</span> as <span class="falseCode">&amp;#x007b;&amp;#x007b;</span>.  Worse than this, if I put this type of line in a non-code block they were interpreted by Jekyll (and thus the examples above rendered the date)
To get round this where lines contain <span class="falseCode">&#x007b;&#x007b;</span> and <span class="falseCode">&#x007d;&#x007d;</span> I've replaced <span class="falseCode">&#x007b;&#x007b;</span> and <span class="falseCode">&#x007d;&#x007d;</span>  with their HTMLEntity equivalents <span class="falseCode">&amp;#x007b;&amp;#x007b;</span> and <span class="falseCode">&amp;#x007d;&amp;#x007d;</span>.  In addition I've wrapped those lines in 
   
     <div class="falseCode"><pre>[followed by four spaces][rest of the line]</pre></div>
     
Finally I've added an entry in style.scss for <span class="falseCode"> .falseCode </span> that replicates the formatting of the standard <span class="falseCode">code</span>. I don't yet understand what .scss files are about other than Jekyll generates .css files from them.

That was all much more complicated than I would have liked, but it works and will do until I find a better way.

***Update***

I've found a much better way to do this:

1. Before the block of code you are inserting put `<div><\div>` with a blank line before and after it
2. On the next line indent by a tab (thus creating a code block) and put `{% raw %}{% raw %}{% endraw %}` on it
3. Insert the code that you want to display with at least one tab at the beginning of each line
4. On the last line of the code block put `{{ "{%" }} endraw %}` on it
5. Put at least one blank line (that starts in the first column - i.e. no spaces or tabs on it) after the code block

I've no idea why step 1. is necessary, but the method doesn't work without it. The code block gets rid of any need to make special arrangements for the HTML tags and the `{% raw %}{% raw %}{% endraw %}` and `{{ "{%" }} endraw %}` get rid of any need to make special arrange ments for  `{{ "{{" }}` , `{% raw %}}}{% endraw %}` , `{{ "{%" }}` and `{% raw %}%}{% endraw %}` 

