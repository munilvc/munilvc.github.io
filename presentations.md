---
layout: page
title: Presentations
---
<div>
<ul>
{% for presentation in site.presentations %}
  <li><b>{{ presentation.date | date_to_string }} : </b> <a href="{{ presentation.url }}" > {{ presentation.title }} </a></li>
{% endfor %}
</ul>
</div>