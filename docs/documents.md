---
layout: documents
title: DOCUMENTS
permalink: /documents/
---

### 日付順
<ul>
{% for post in site.posts %}
  <li>
    <a href="{{ site.baseurl }}{{ post.url }}"><code>{{ post.date | date: "%Y-%m-%d" }}</code>　{{ post.title }}</a>
  </li>
{% endfor %}
</ul>


### タグ順
{% assign tag_names = "" | split: "|"  %}
{% for posts_by_tag in site.tags %}
  {% assign tag_names = tag_names | push: posts_by_tag.first %}
{% endfor %}
{% assign tag_names = tag_names | sort %}
{% include tag_cloud.html tag_names=tag_names %}

{% for tag_name in tag_names %}
<h4 id="{{ tag_name }}">
	{{ tag_name | replace: "_", " " }}
</h4>
<ul>
	{% for post in site.tags[tag_name] %}
	<li>
		<a href="{{ post.url | prepend: baseurl }}">
			{{ post.title }}
		</a>
	</li>
	{% endfor %}
</ul>
{% endfor %}
