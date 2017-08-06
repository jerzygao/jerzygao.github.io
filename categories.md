---
layout: category
title: 分类
---

<ul>
{% for cat in site.categories %}
    {% if cat[0] != 'blog' %}
    <a name="{{ cat[0] }}"></a>
    <!-- <div style="margin: 25px 0; width: 710px; height: 35px;border-top: dashed 1px #999">
    </div>
		-->
   <h2>{{ cat[0] }}({{ cat[1].size }})</h2>
     {% for post in cat[1] %}
    <li><h4><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></h4></li>
    {% endfor %}
   {% endif %}
{% endfor %}
</ul>
