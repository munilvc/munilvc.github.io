---
layout: page
title: List all posts
---
## Blog Posts

{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}