---
title: "Articles"
layout: single
permalink: /articles/
taxonomy: articles
author_profile: false
classes: wide
---

<p class="page-intro">
  A collection of articles about machine learning, quantitative trading, software development, and other technical topics. Here I share my experiences, insights, and learnings from working in these fields.
</p>

<section class="post-index post-index--articles" aria-label="Articles">
{% assign article_posts = site.categories.articles %}
{% for post in article_posts %}
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
