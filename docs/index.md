---
layout: default
title: HN Digest Archives
---

# HN Digest Archives

{% comment %}
We build from /docs, so site.pages includes all .md files in this folder.
Filter to names starting with "hn-digest-", sort by name (lexicographic),
then reverse so newest (YYYY-MM-DD...) appears first.
{% endcomment %}

{% assign digests = site.pages
  | where_exp: "p", "p.name contains '.md'"
  | sort: "name"
  | reverse %}

{% if digests.size > 0 %}
<ul>
  {% for p in digests %}
  <li>
    <a href="{{ p.url | relative_url }}">
      {{ p.name | replace: '.md', '' }}
    </a>
  </li>
  {% endfor %}
</ul>
{% else %}
<p>No digests published yet.</p>
{% endif %}
