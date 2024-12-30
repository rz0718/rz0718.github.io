---
title: "Different Ways to Add Images in Articles"
categories:
  - articles
tags:
  - tutorial
excerpt: "A guide showing different ways to add and style images in articles."
---

There are several ways to add images in your articles. Here are the common patterns:

## 1. Basic Image
![Basic image description](/assets/images/articles/sample-image.jpg)

## 2. Image with Caption
{% include figure image_path="/assets/images/articles/sample-image.jpg" alt="Detailed description" caption="This is an image caption" %}

## 3. Image with Custom Size
<img src="/assets/images/articles/sample-image.jpg" alt="Size controlled image" width="300"/>

## 4. Left Aligned Image
<img src="/assets/images/articles/sample-image.jpg" alt="Left aligned" width="200" style="float: left; margin-right: 10px;"/>
Text wrapping around the left-aligned image. This shows how text flows around an image when it's aligned to the left.

<div style="clear: both;"></div>

## 5. Right Aligned Image
<img src="/assets/images/articles/sample-image.jpg" alt="Right aligned" width="200" style="float: right; margin-left: 10px;"/>
Text wrapping around the right-aligned image. This shows how text flows around an image when it's aligned to the right.

<div style="clear: both;"></div>

## 6. Centered Image
<p align="center">
  <img src="/assets/images/articles/sample-image.jpg" alt="Centered image" width="400"/>
</p>

## 7. Two Images Side by Side
<p float="left">
  <img src="/assets/images/articles/sample-image.jpg" width="200"/>
  <img src="/assets/images/articles/sample-image.jpg" width="200"/>
</p>

## 8. Image with Link
[![Clickable image](/assets/images/articles/sample-image.jpg)](https://your-link.com)

## Tips for Using Images

1. **Image Location**: 
   - Place images in `/assets/images/articles/` for articles
   - Use `/assets/images/blogs/` for blog posts

2. **File Naming**:
   - Use descriptive, lowercase names
   - Replace spaces with hyphens
   - Example: `machine-learning-diagram.jpg`

3. **Image Sizes**:
   - Keep images under 1MB
   - Use appropriate dimensions (e.g., 800px max width)
   - Compress images when possible

4. **Alt Text**:
   - Always include descriptive alt text
   - Helps with accessibility and SEO 