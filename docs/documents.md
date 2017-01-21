---
layout: documents
title: DOCUMENTS
permalink: /documents/
---


<ul>
{% for post in site.posts %}
  <li>
    <a href="{{ site.baseurl }}{{ post.url }}"><code>{{ post.date | date: "%Y-%m-%d" }}</code>　{{ post.title }}</a>
  </li>
{% endfor %}
</ul>
