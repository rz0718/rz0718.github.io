---
title: "How AK uses LLM"
categories:
  - articles
tags:
  - LLM
  - Application
  - Production
  - Efficiency
excerpt: "Productivity"
toc: true
toc_sticky: true
toc_label: "Table of Contents"
---

Andrej Karpathy recently posted a youtube [video](https://www.youtube.com/watch?v=EWvNQjAaOHw) about how he uses LLM in his work and his best practies of interacting with LLM. Like other content that he producted, it is very insightful and worth watching. In this short blog, I will try to summary the key points from his two hours long video.

## Chat Management
- Start a new chat when switching topics to maintain context clarity
- Be mindful of which model you're using and its capabilities
- For math and code tasks, use thinking models for better accuracy
- Start with non-thinking models for speed, then escalate if needed

## Tool Use

### Internet Search
- Useful for real-time information and trending topics
- Example use case: Checking if Vercel offers PostgreSQL datasets
- Combine with thinking capabilities, it is the deep research feature from GPT. But AK suggested to treat results as first drafts and than verify information

### Document Processing
- PDF uploads convert to text but may lose images and table formatting
- For AK, it is very effective for book analysis with targeted prompts such as "We are reading [Book Name]. Please summarize this chapter."

### Code Execution
- Python interpreter available for computation
- Examples:  for simple math like 4* 50, the calculations are memory-based, while for more complex math like 4544343243 * 45442432432, it requires tool use for actual computation. And he kind of show grok is not able to call tools while GPT's big models are able to do so.

## Other Applications

### Diagram Generation
- Claude can generate diagrams from various inputs. It is useful for visualizing information from books and papers. And it also helps to organize and structure complex information

### Code Context
- ChatGPT web interface has limited context awareness
- Use Cursor for file system integration and better code understanding

### Google's Notebook LLM
- Supports multiple source types (PDF, webpage)
- Enables interactive chat
- Can generate podcast content

### Image Processing
- Processes images as token sequences
- OCR capabilities for text extraction
- Can analyze image content (e.g., nutrition information)
- Process:
  1. Transcribe image content
  2. Query the extracted information

## Summary

Rather than trying to eliminate LLM limitations like hallucination, AK advocates for understanding and working within these constraints. His approach focuses on identifying appropriate use cases and developing effective workflows that account for LLM capabilities and limitations. And he also helped us to try multiple paid LLM tools. It is good for us to save the money and time to find the best tool for our use case.