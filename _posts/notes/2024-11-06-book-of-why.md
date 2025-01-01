---
title: "The Book of Why"
categories:
  - notes
tags:
  - Science
excerpt: "Cause and Effects"
sidebar:
  - title: "Book Info"
    image: "/assets/images/books/book-of-why.jpg"
    image_alt: "Book cover"
    image_width: 150
    text: |
      **Author**: Judea Pearl
---

"The Book of Why" by Judea Pearl offers an intricate exploration of causal reasoning through what he terms the "ladder of causation," 
- Level 1: Associations, observational data (seeing)
- Level 2: Intervention (doing, or RCT)
- Level 3: Counterfactuals (thinking)

The book starts with a neat structure, diving deep into the historical and philosophical underpinnings of causality, and emphasizing how traditional statistical methods and the most recent deep learning methods have often overlooked causal inference.

In this book, he also highlights the importance of the "do-calculus" and "directed acyclic graphs (DAGs)" in causal inference.
-  A DAG is simply a diagram containing nodes that are connected by edges (lines) with directions, and tracing these edges according to their directions should not result in a closed loop. In causal analysis, DAGs are used to represent the relationships between variables in a system, with the directed edges indicating the (assumed) direction of causation.

- Do-calculus is a set of rules that allows us to compute the effect of an intervention on a variable in contrast to obersvation. It is kind of the difference between P(A|do(B)) and P(A|B).

The above two approaches and the causal inference does not simply determine the causes of an event but enriches our understanding of associations beyond mere correlation.

This work is also deeply personal; Pearl reflects on his career and the impact of his son's tragic death on his scholarly pursuits, adding a poignant layer to his academic discussions. He critiques the resistance within the statistical community to adopt causal thinking, positioning his "causal revolution" as a necessary shift towards more meaningful data analysis.

However, the book is not easy to read for those who are not familiar with the basics of statistics and probability theory. But it is also too simple for those reseachers or technologists who are already familiar with casual inference. But Pearl's clear and engaging style makes complex concepts accessible, offering readers both a comprehensive overview of causal analysis and a compelling call to rethink how we interpret data.

Yes, and data are fundamentally dumb. You’re smarter than your data.

In practice, the causal inference is not easy to apply. I also struggled a lot with causal Inference. Over my experiences, the challenges are:
1. Technicial part: high-dimensional data and the lack of understanding of the causal relationships between variables.
2. Non-technical part: how to get buy-in from stakeholders and how to communicate the results to them.