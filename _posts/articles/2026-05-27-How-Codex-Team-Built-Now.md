---
title: "How Codex Team Built Now"
categories:
  - articles
tags:
  - Codex
  - Agents
  - Product Engineering
excerpt: "Some thoughts after using Codex more than Claude Code recently, and after listening to how the Codex team builds with Codex."
toc: true
toc_sticky: true
toc_label: "Table of Contents"
---

Recently I have been using Codex more than Claude Code considering the limits for token usages is high. Then I came across Peter Yang's podcast episode with Alex and Romain from the OpenAI Codex team. The episode was nominally about how the Codex team builds with Codex, but I found the deeper message more interesting: **this is a snapshot of how an agentic organization starts to look from the inside**.

The important part is not only that they use AI to write code. Many teams do that now. The interesting part is that the tool has already started to change their planning horizon, role boundaries, decision structure, and hiring taste.

That is why I want to record these thoughts. Tools change first. Then workflows change. Then organization shape changes.

## Short Term or Long Term, Never Medium Term

One line from the episode stayed with me:

> Plan concrete milestones up to 8 weeks out and have a long-term vision of where models are heading. Never medium term.

This sounds simple, but it is a very sharp planning philosophy.

Short term planning still works. Within the next 6 to 8 weeks, model capability is relatively visible, user pain is concrete, and a team can make real tradeoffs. You can say: this workflow is slow, this agent handoff is confusing, this review step is too expensive, this product surface should be cleaned up.

Long term thinking also still matters. You need a direction: where models are going, what the product should become when agents are much more capable, and what kind of human role should remain in the loop.

The dangerous part is the middle. A 6-month or 12-month detailed roadmap is often neither concrete enough nor visionary enough. In an AI product, the model capability curve moves so quickly that medium-term plans can become stale before the team finishes executing them. The plan was written for yesterday's model, but the team is already building on tomorrow's model.

This matches my own feeling using these tools. The product surface is changing fast, but the more important change is the assumption underneath. Six months ago, the question was often "can the agent summary my meeting notes?" Now the question is closer to "how many agents can I supervise in parallel, and what context structure keeps the output reliable?"

That is a different product problem.

## Decision Moves Downward

Traditional companies tend to move decision rights upward.

The person closest to the problem reports to a manager. The manager summarizes it to another manager. The decision eventually lands with someone who has broad responsibility but is several layers away from the actual details.

That structure made sense when execution was slow and information was expensive to move. If shipping took weeks, there was time for status updates, planning meetings, review meetings, and alignment meetings. The organization could afford many translation layers.

Agentic tools compress that window.

When a designer can prototype directly, when a PM can generate a working flow, when an engineer can open ten agent threads and test several implementation paths in parallel, the old waiting cost becomes much more visible. The person closest to the problem often has the freshest context and now also has enough execution power to act on it.

So the decision right naturally moves closer to the edge.

This does not mean "no leadership" or "everyone does whatever they want". It means the leadership function changes. Instead of routing every decision upward, leadership has to create a clear environment: product taste, technical standards, permission boundaries, evaluation loops, and a shared sense of where the model frontier is going.

Once that environment exists, the best decisions often happen near the work.

## The Talent Stack Is Collapsing

One of the most striking parts of the podcast is the point that designers on the Codex team now write more code than engineers did six months ago.

I do not read that as a cute anecdote. It is a serious signal.

Traditional software organizations are built on clear role boundaries:

- Engineers write code.
- Designers produce designs.
- PMs write specs and manage priorities.
- Managers coordinate the people above.

Those boundaries are partly about specialization, but they are also about tooling limitations. A designer could imagine an interaction but could not easily turn it into production code. A PM could understand the user problem but could not easily prototype the solution. A junior person could have taste but not enough technical depth to push through implementation complexity.

If the agent removes a large part of the implementation barrier, these role boundaries start to blur.

Designers can become builders. PMs can stop writing abstract strategy documents and start making concrete prototypes. Engineers can move further upstream, talk to users directly, and decide what to build from the bottom up.

This is not the end of expertise. Deep infrastructure, distributed systems, security, databases, and model research still require real depth. But for a large surface area of product engineering, the bottleneck is shifting away from "who can type the code" toward "who can define the right artifact, judge the result, and close the loop."

That changes what kind of person becomes valuable.

## The Advantage Moves to the Crossing Point

In the old career model, depth inside one vertical was the safest path. You became a frontend engineer, backend engineer, product designer, PM, data scientist, or infra specialist. Years of experience were used as a proxy for how much complexity you had absorbed.

That proxy is getting weaker.

If an agent can absorb the first layer of technical complexity, then the advantage moves to people who can cross boundaries:

- Engineers who understand product taste.
- Designers who can reason about implementation.
- PMs who can judge technical complexity.
- Researchers who can turn ideas into usable systems.
- Operators who can automate their own workflows.

The opportunity is not simply "AI makes everyone faster". The opportunity is that people who were previously blocked by one missing skill can now move through that boundary.

This is also why the Codex team's working style feels uncomfortable in a useful way. It conflicts with the picture many of us have in our head of how a normal company works. Normal companies have clean lanes, formal planning, handoffs, and decision ladders. The Codex team sounds more like a small group of high-agency builders using agents to collapse distance between idea, prototype, review, and shipping.

That discomfort is a good signal. It means the mental model is being stretched.

## Minimal Process, More Ownership

Another detail I liked is their attitude toward specs. From the podcast summary, they often avoid heavy formal documentation and may write something closer to 10 bullets when a project needs shared context.

This does not mean documentation is useless. It means the cost-benefit curve changes.

When implementation is slow, a long spec can be cheaper than building the wrong thing. When implementation is fast, a working prototype may be cheaper than a long explanation. The artifact that creates alignment shifts from a document to a running thing.

That is one reason agentic development can feel like a "pirate ship" culture. The team can stay small, the loop can stay tight, and senior people have to remain close to the work. You cannot manage agentic development only from a distance, because the important questions are often in the details:

- Did the agent choose the right abstraction?
- Is the generated code easy to maintain?
- Did the product interaction actually feel right?
- Is the review loop strong enough?
- Did the permission boundary make the workflow safer or just slower?

This is very similar to what I have felt using Codex myself. The more I use it, the less the job feels like "writing code" and the more it feels like directing parallel work, maintaining context, reviewing output, and deciding when to stop.

The work becomes less manual, but not less responsible.

## From Tool Adoption to Agentic Transformation

The deeper lesson from the podcast is that "using AI tools" and "agentic transformation" are not the same thing.

A team can use AI tools and still keep the old organization:

- PM writes a spec.
- Designer makes a mock.
- Engineer implements it with AI autocomplete.
- Manager checks status.
- QA tests at the end.

That is useful, but it is still the old workflow with faster typing.

Agentic transformation is different. It asks whether the organization still needs the same handoffs when many people can directly create working artifacts. It asks whether decision rights should remain centralized when execution and information both move faster. It asks whether job titles still describe the real work.

The chain looks like this:

```text
Better tools
  -> faster execution
  -> more information symmetry
  -> fewer useful handoffs
  -> blurrier role boundaries
  -> different organization shape
```

This is the part I find most insightful. The Codex team is not just building an agent product. They are also becoming a live example of what happens when an agent product is good enough to reshape the team building it.

## What I Take Away

There are a few things I want to remember.

First, medium-term roadmaps are weaker in a fast model cycle. Plan the next concrete push, keep a long-term direction, and be careful with detailed plans in the middle.

Second, decision rights should move closer to the person with the richest context. AI reduces execution cost, so the old cost of waiting for multi-layer approval becomes more obvious.

Third, role boundaries will keep blurring. This is not chaos. It is what happens when tools remove part of the technical barrier between idea and implementation.

Fourth, career advantage moves toward the crossing point. The best place to stand is often between product and engineering, design and code, workflow and automation, human judgment and agent execution.

Finally, agentic work still needs ownership. The human role does not disappear. It moves toward setting direction, designing the environment, reviewing artifacts, and deciding what quality means.

This is changing. Let us jump into the storm.

## References

1. [How OpenAI's Codex Team Builds with Codex | Alex & Romain](https://www.youtube.com/watch?v=9qXc-THAvc0)
