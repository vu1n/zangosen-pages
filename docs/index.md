---
layout: default
title: HN Digest Archives
---

# HN Digest Archives

{% comment %}
We build from /docs, so site.pages lists pages relative to that folder.
We want files under "hn/stories/" named YYYYMMDD.md.
We avoid where_exp for max compatibility with GitHub Pages' Liquid.
{% endcomment %}

{%- assign pages_sorted = site.pages | sort: "name" | reverse -%}

{%- assign latest_url = nil -%}
{%- assign latest_label = nil -%}
{%- assign found_latest = false -%}

{%- comment -%} First pass: find the latest digest {%- endcomment -%}
{%- for p in pages_sorted -%}
  {%- if p.path contains 'hn/stories/' and p.name contains '.md' and found_latest == false -%}
    {%- assign d = p.name | split: '.' | first -%}
    {%- if d.size == 8 -%}
      {%- assign y = d | slice: 0,4 -%}
      {%- assign m = d | slice: 4,2 -%}
      {%- assign dd = d | slice: 6,2 -%}
      {%- assign latest_label = y | append: '-' | append: m | append: '-' | append: dd -%}
    {%- else -%}
      {%- assign latest_label = d -%}
    {%- endif -%}
    {%- assign latest_url = p.url -%}
    {%- assign found_latest = true -%}
  {%- endif -%}
{%- endfor -%}

{%- if latest_url -%}
<p><strong>Latest:</strong> <a href="{{ latest_url | relative_url }}">{{ latest_label }}</a></p>
<hr/>
{%- else -%}
<p>No digests published yet.</p>
{%- endif -%}

<ul>
{%- comment -%} Second pass: list all digests {%- endcomment -%}
{%- for p in pages_sorted -%}
  {%- if p.path contains 'hn/stories/' and p.name contains '.md' -%}
    {%- assign d = p.name | split: '.' | first -%}
    {%- if d.size == 8 -%}
      {%- assign y = d | slice: 0,4 -%}
      {%- assign m = d | slice: 4,2 -%}
      {%- assign dd = d | slice: 6,2 -%}
      {%- assign label = y | append: '-' | append: m | append: '-' | append: dd -%}
    {%- else -%}
      {%- assign label = d -%}
    {%- endif -%}
    <li><a href="{{ p.url | relative_url }}">{{ label }}</a></li>
  {%- endif -%}
{%- endfor -%}
</ul>
