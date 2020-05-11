---
layout: archive
title: "개발 노트"
permalink: /dev-notes
author_profile: false
---

{% if paginator %}
  {% assign posts = paginator.posts %}
{% else %}
  {% assign posts = site.posts %}
{% endif %}

{% assign items_grouped = posts  | where_exp:"item", 
  "item.categories[0] == 'dev-note'" | group_by_exp: "item", "item.categories[1]" | sort: 'name' %}
{% assign etc_text = site.data.ui-text[site.locale].etc | default: "Etc" %}

{% for group in items_grouped %}
  {% assign subtile = group.name %}
  {% for data_category in site.data.categories %}
    {% if subtile == data_category.slug %}
      {% assign subtile = data_category.name %}
    {% endif %}
  {% endfor %}
  <h3 class="archive__subtitle">{{ subtile | etc_text }}</h3>
  {% assign sorted = group.items | sort: 'last_modified_at' | reverse %}
  {% for post in sorted limit:3 %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}