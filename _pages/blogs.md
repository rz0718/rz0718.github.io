---
title: "Blogs"
layout: single
permalink: /blogs/
taxonomy: blogs
author_profile: false
classes: wide
---

<p class="page-intro">
  Personal thoughts, experiences, and observations about technology, life, and everything in between. A space where I share my journey and insights.
</p>

<section class="post-index post-index--articles" aria-label="Blogs">
{% assign blog_posts = site.categories.blogs %}
{% for post in blog_posts %}
  {% assign words_per_minute = site.words_per_minute | default: 200 %}
  {% assign read_minutes = post.content | number_of_words | divided_by: words_per_minute | ceil %}
  {% if read_minutes == 0 %}
    {% assign read_minutes = 1 %}
  {% endif %}
  <article class="post-index__item">
    <div class="post-index__body">
      <div class="post-index__meta">
        <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time>
        <span>{{ read_minutes }} minute read</span>
      </div>
      <h2 class="post-index__title">
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </h2>
      {% if post.excerpt %}
      <p class="post-index__excerpt">
        {{ post.excerpt | strip_html | truncate: 180 }}
      </p>
      {% endif %}
    </div>
  </article>
{% endfor %}
</section>
