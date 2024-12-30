---
layout: home
author_profile: true
entries_layout: grid
classes: wide
---

Here, I read, build, think and also record. Welcome to my playground.

## Recent Posts

{% for post in site.posts limit:5 %}
  <article>
    <h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
    <p class="post-meta">
      <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time>
      {% if post.categories %}
      • <span>{{ post.categories | join: ", " }}</span>
      {% endif %}
    </p>
    {% if post.excerpt %}
    <p>{{ post.excerpt | strip_html | truncate: 160 }}</p>
    {% endif %}
  </article>
{% endfor %}

## Contact

- Email: rui91seu@gmail.com
- GitHub: [rz0718](https://github.com/rz0718)
- LinkedIn: [Rui Zhao](https://www.linkedin.com/in/rui-zhao-ph-d-cfa-1b4288112/) 