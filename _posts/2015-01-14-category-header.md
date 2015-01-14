---
layout: post
title: 'Creating the Category index'
date: 2015-01-14 09:18:30
category: scripting
tags: 
    - jekyll
    - categories
---
In my last post I mentioned that I preferred my list of categories to go along the top of the summary of posts rather than in a column to the left or right.  I also wanted them to appear in the same colours that were used in the blog posts.  This is the code that I used:

<div></div>
	{% raw %}
	<p id="post-meta">
		<a href="/"><span class="label" style="color:black">All</span></a>
		{% for theCategory in site.data.categories %}
			<a href="/blog/category/{{theCategory.["slug"]}}">
				{% if {{theCategory.["slug"]}} == {{page.category}} %}
					<span class="label" style='color:#ffffff;background-color:{{theCategory.["color"]}}'>
				{% else %}
					<span class= "label" style='color:{{theCategory.["color"]}}'>
				{% endif %}
			{{theCategory.["name"]}}</span></a>
		{% endfor %}
	</p>
	{% endraw %}

I put this in `_layouts\cathead.html` and included it in `_layouts\blog_by_html`
