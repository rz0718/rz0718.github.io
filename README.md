# Personal Website

This is my personal website built with Jekyll and the Minimal Mistakes theme. You can visit it at [https://rz0718.github.io](https://rz0718.github.io).

## Features

- Clean, responsive design using Minimal Mistakes theme
- Articles, Notes, and Blog posts
- About page with professional information

## Content Creation Guidelines

### Using Templates

The `_templates` directory contains templates for different types of content:

1. **Technical Articles** (`article-template.md`):
   - Use for: Technical tutorials, research findings, analysis
   - File naming: `YYYY-MM-DD-article-name.md`
   - Location: `_posts/articles/`
   - Features:
     - Table of contents
     - Code blocks
     - Technical diagrams
     - References section
   - Tips:
     - Keep code snippets concise
     - Include diagrams for complex concepts
     - Add references for technical claims

2. **Personal Blogs** (`blog-template.md`):
   - Use for: Life reflections, travel stories, personal experiences
   - File naming: `YYYY-MM-DD-blog-title.md`
   - Location: `_posts/blogs/`
   - Features:
     - Story structure
     - Photo galleries
     - Personal insights
   - Tips:
     - Focus on personal narrative
     - Use photos to enhance storytelling
     - Share genuine reflections

3. **Book Notes** (`book-note.md`):
   - Use for: Book summaries, reading reflections
   - File naming: `YYYY-MM-DD-book-title.md`
   - Location: `_posts/notes/`
   - Features:
     - Book info sidebar
     - Cover image
     - Structured format
   - Tips:
     - Include book metadata
     - Add meaningful quotes
     - Share personal takeaways

### Creating New Content

1. **Choose Template**:
   ```bash
   cp _templates/[template-name].md _posts/[category]/YYYY-MM-DD-title.md
   ```

2. **Edit Front Matter**:
   ```yaml
   ---
   title: "Your Title"
   categories:
     - [articles/blogs/notes]
   tags:
     - relevant-tag
   excerpt: "Brief description"
   ---
   ```

3. **Add Images**:
   - Place in correct directory:
     ```
     assets/images/
     ├── articles/
     ├── blogs/
     └── books/
     ```
   - Optimize before uploading
   - Use meaningful names

4. **Format Content**:
   - Follow template structure
   - Add/remove sections as needed
   - Include relevant images
   - Add proper citations/references

### Image Usage

See `_templates/image-template.md` for detailed examples of:
```markdown
# Basic image
![Alt text](/path/to/image.jpg)

# Image with caption
{% include figure image_path="/path/to/image.jpg" caption="Caption text" %}

# Sized image
<img src="/path/to/image.jpg" width="300"/>

# Photo gallery
{% include gallery caption="Collection of memories" %}
```

## Citation System

This blog includes a citation system that allows you to add academic-style references to your posts. Here's how to use it:

### 1. Adding References

Add your references to `_data/references.yml` in the following format:

```yaml
einstein1905special:
  authors: "Einstein, A."
  year: 1905
  title: "On the Electrodynamics of Moving Bodies"
  journal: "Annalen der Physik"
  volume: 17
  pages: "891-921"
  url: "https://example.com/paper"
```

### 2. Using Citations in Posts

To cite a reference in your text, use:

```liquid
{% include cite.html key="einstein1905special" %}
```

This will display as "[Einstein 1905]" in your text and link to the full reference at the bottom of the post.

### 3. Adding Reference List

At the end of your post, add the reference list:

```liquid
## References
{% include references.html keys="einstein1905special,newton1687principia" %}
```

Multiple references should be comma-separated.

### Example

Here's a complete example:

```markdown
---
title: "My Physics Article"
---

The theory of relativity {% include cite.html key="einstein1905special" %} 
revolutionized our understanding of space and time.

## References
{% include references.html keys="einstein1905special" %}
```

The citation system will:
- Display in-text citations as [Author Year]
- Create clickable links to the full references
- Automatically format the reference list
- Support multiple citations in one post

## Development

This website is built with:
- Jekyll
- Minimal Mistakes theme
- GitHub Pages

## Local Development

1. Install Ruby and Bundler
2. Clone this repository
3. Run `bundle install`
4. Run `bundle exec jekyll serve`
5. Visit `http://localhost:4000`

## Deployment

The site is automatically deployed through GitHub Pages when changes are pushed to the main branch.