---
layout: default
title: HN Digest Archives
---

# HN Digest Archives

{% comment %}
Collect all Markdown pages under /hn/stories, sort by filename (yyyyMMdd.md) descending.
Using site.pages works because GitHub Pages processes .md files in /docs.
{% endcomment %}

{% assign digests = site.pages
  | where_exp: "p", "p.path contains '/hn/stories/' and p.ext == '.md'"
  | sort: "name" | reverse %}

{% if digests.size > 0 %}
<p><strong>Latest:</strong>
  {% assign latest = digests[0] %}
  <a href="{{ latest.url | relative_url }}">
    {% assign d = latest.name | split: '.' | first %}
    {% if d.size == 8 %}
      {% assign y = d | slice: 0,4 %}
      {% assign m = d | slice: 4,2 %}
      {% assign dd = d | slice: 6,2 %}
      {{ y }}-{{ m }}-{{ dd }}
    {% else %}
      {{ d }}
    {% endif %}
  </a>
</p>

<hr/>

<ul>
{% for p in digests %}
  {% assign d = p.name | split: '.' | first %}
  {% if d.size == 8 %}
    {% assign y = d | slice: 0,4 %}
    {% assign m = d | slice: 4,2 %}
    {% assign dd = d | slice: 6,2 %}
    {% assign label = y | append: '-' | append: m | append: '-' | append: dd %}
  {% else %}
    {% assign label = d %}
  {% endif %}
  <li><a href="{{ p.url | relative_url }}">{{ label }}</a></li>
{% endfor %}
</ul>
{% else %}
<p>No digests published yet.</p>
{% endif %}
