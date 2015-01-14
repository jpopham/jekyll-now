---
layout: default
published: true
---
{% include catheader.html %}
<div class="posts">
  {% for post in site.posts %}
    <article class="post">   
       <div class="postdate">
		<a href="{{ page.url }}">
      			{{ post.date | date:"<span class='day'>%d</span> <span class='month'>%b</span> <span class='year'>%Y</span>" }}
		</a>
	
	</div>
      <h1 id="post-title"><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1>
 {% if post.category %}
            {% for site_category in site.data.categories %}
                {% if site_category.slug == post.category %}
                    {% assign category = site_category %}
                {% endif %}
            {% endfor %}
            {% if category %}
                {% capture category_content %} in <span class="label" style="background-color:{{ category.color }}"><a href="/blog/category/{{ category.slug }}/">{{ category.name }}</a></span>{% endcapture %}
            {% endif %}
        {% else %}
            {% assign category_content = '' %}
        {% endif %}

        {% if post.tags.size > 0 %}
            {% capture tags_content %} with {% if post.tags.size == 1 %}<i class="fa fa-tag"></i>{% else %}<i class="fa fa-tags"></i>{% endif %}: {% endcapture %}
            {% for post_tag in post.tags %}
                {% for data_tag in site.data.tags %}
                    {% if data_tag.slug == post_tag %}
                        {% assign tag = data_tag %}
                    {% endif %}
                {% endfor %}
                {% if tag %}
                    {% capture tags_content_temp %}{{ tags_content }}<a href="/blog/tag/{{ tag.slug }}/">{{ tag.name }}</a>{% if forloop.last == false %}, {% endif %}{% endcapture %}
                    {% assign tags_content = tags_content_temp %}
                {% endif %}
            {% endfor %}
        {% else %}
            {% assign tags_content = '' %}
        {% endif %}

        <p class="cat-list">Posted {{ category_content }}{{ tags_content }}</p>
      <div class="entry">

        {{ post.content | truncatewords:100}}
      </div>
      
      <a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Read More...</a>
    </article>
  {% endfor %}
</div>
