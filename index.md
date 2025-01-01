---
layout: single
author_profile: true
classes: wide
---

Here, I read, build, think and also record.

## Recent Posts

{% for post in site.posts limit:6 %}
  <article class="archive__item">
    <h3 class="archive__item-title">
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </h3>
    <p class="archive__item-excerpt">
      <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time>
      {% if post.categories %}
      • {{ post.categories | join: ", " }}
      {% endif %}
    </p>
    {% if post.excerpt %}
    <div class="archive__item-excerpt">
      {{ post.excerpt | strip_html | truncate: 160 }}
    </div>
    {% endif %}
  </article>
{% endfor %}