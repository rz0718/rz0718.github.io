---
layout: single
author_profile: true
classes: wide
---

Here, I read, build, think and also record.

## Recent Posts

<section class="post-index post-index--timeline" aria-label="Recent posts">
{% for post in site.posts limit:10 %}
  <article class="post-index__item">
    <time class="post-index__date" datetime="{{ post.date | date_to_xmlschema }}">
      <span>{{ post.date | date: "%b %d" }}</span>
      <small>{{ post.date | date: "%Y" }}</small>
    </time>
    <div class="post-index__body">
      <div class="post-index__meta">
        {% if post.categories %}
          <span>{{ post.categories | join: ", " }}</span>
        {% endif %}
      </div>
      <h3 class="post-index__title">
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </h3>
    {% if post.excerpt %}
    <p class="post-index__excerpt">
      {{ post.excerpt | strip_html | truncate: 160 }}
    </p>
    {% endif %}
    </div>
  </article>
{% endfor %}
</section>
