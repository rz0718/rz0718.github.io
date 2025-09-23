---
title: "What is Reasoning Behind LLM"
categories:
  - articles
tags:
  - LLM
  - Reasoning
  - Chain of Thoughts
excerpt: "LLM"
toc: true
toc_sticky: true
toc_label: "Table of Contents"
---

There is always a debate about the nature of reasoning behind LLM. Is it just pattern matching from huge amount of training data or the true emergent intelligence. We sometimes got surprised by LLM laying out a complex solution with detailed, step-by-step logic that feels truly smart but also sometimes got some nonsense answer from some trivial questions that we asked. There is a recent Stanford lecture delivered by Denny Zhou, a lead researcher at Google DeepMind who gave his practical insights about how AI "thinks". It is quite mind-blowing to me. And I have shared 5 key points from his talk. To start, I would like to share his quote from the last slide:

>"The truth, in the end, is always simpler than you could have imagined."

## Reasoning is just steps in Between

In his lecture, he moved past the above debates. They just gave an engineering-focused definition: reasoning is all the intermediate steps generated between a question and the final answer.  He illustrated this with a clever task: "last letter concatenation". Firstly, he tried the task with "first letter concatenation", but found that almost all LLM models could do it easily because all pre-training data over internet is filled with acronyms so that the model can easily memorize the pattern. So to ask the model to concatenate the last letters of "artificial" and "intelligence", it will be more challenging since the models had never seen.

No reasoning here would be if the model just outputs LE, it is only next token prediction. But if it is trying to generate the intermediate steps as 
>`The last letter of “artificial” is “l”. The last letter of “intelligence” is “e”. Concatenating “l” and “e” leads to “le”. So the answer is “le”`

it would be called reasoning. This conclusion actually lays the foundation for all of their LLM reasoning works: focus on process instead of the single answer.

## All we need is decoding - even Pretrained LLM knows how to reason

Zhou mentioned LLM already knows how to reason, i.e., generate a series of steps. But we were sometimes just talking to the model wrongly. They shared one example: given this question: "I have three apples, my dad has two more than me, how many do we have in total?" Most of earlier LLM might just generate five apples. It is because most of "old" LLMs use greedy decoding, always picking the single most probable next token. But in those models, the correct, step-by-step reasoning path like: "I have three apples. My dad has two more, so he has five. 3+5 equals 8. So we have eight apples total" exists within the model's potential outputs. But it is not the highest probabilistic one, so it is just in the side roads that greedy decoding might ignore. So pretrained LLMs can reason without further prompting engineering or fine-tuning.

To change decoding towards reasoning, so Zhou and his team just proposed COT decoding approach with the following two steps:

1. Go beyond greedy decoding by checking more generation candidates
2. Choose candidates which have the highest confidence on the final answer

Take the above example, when a model generated a correct chain of thought, its internal confidence in the final answer—the number "8"—skyrocketed to as high as 98%. So the probability of the whole chain might not be high as the one of short, quick answer but it is a state of high certainty for the final answer.

Another sharing by Zhou also explains my long-term confusion about the magic prompt: let us think step-by-step. He mentioned that he first tested it on Google's PaLM model. During the training stage, he never exposed this model to this "magic code," but it worked. It actually proves here we are not teaching AI to reason, but more to guide it to express the reasoning it already knows.

## RL Finetuning is superior to SFT

To fine-tune the base model towards reasoning models, the previous approach was supervised fine-tuning (SFT) that domain experts provide reasoning steps and train the model to match those. It has its own limitation: weak generalization capability. Scaling does not help much. Therefore, it leads to a new approach which is also well-known now, RL-fine-tuning, which is a self-improvement loop with the following steps:

1. First, collect a set of problems and their step-by-step
solutions generated from the model
2. Second, an automatic "verifier" checks which of those is the correct solutions.
3. Third, the model is fine-tuned to maximize the likelihood of correct solutions, minimize the likelihood of wrong solutions.

The fundamental idea here is that instead of forcing the model to imitate human reasoning steps in SFT, RL-fine-tuning optimizes the model directly to generate the correct answer. The model parameters are optimized for the correctness of the final answer. It is kind of encouraging the model to discover its own "side" way as a more generalized reasoning path.

## Aggregation multiple answer

To further enhance the reasoning capability of the model, Zhou proposed an approach called "self-consistency" that selects the final answer from a majority voting.

The process is just two steps:

1. Generate multiple responses by randomly sampling
2. Choose the answer that appears most frequently

The key point in the above process: we should rely on chain-of-thought by asking the model to generate multiple different reasoning paths and then have a vote on the final answers they produce. Because if we just ask for the final and single answer, we are just sampling from the model's raw prob distribution, i.e., next token prediction.

## Combining retrieval and reasoning

To resolve the debate: is LLM truly reasoning or just "retrieving" a similar answer from its memory for all training data, Zhou gave his practical answer: no choice. Combine reasoning and retrieval.

He shows this with the following prompt:

> What is the area of the square with the four vertices at (-2, 2), (2, -2), (-2, -6), and (-6,
-2)? <span style="color: red;">Recall a related problem, and then solve this one.</span>

Without the simple prefix, GPT 3.5 model will fail for this geometry problem but with that code, the model is able to solve the problem by self-retrieving the related problem/knowledge.

Therefore, he pointed out that an intelligent system does not reason in a vacuum, it should know how to find and apply external knowledge to guide their reasoning path. 

## Conclusion

At the end, he has a quick summary for four golden rules:

1. Reasoning is better than no reasoning
2. RL finetuning is better than SFT
3. Aggregating multiple answers is better than one answer
4. Retrieval + reasoning is better than reasoning only.

And, he highlighted the next great challenge: how to solve tasks beyond unique verifiable answers. Because the above techniques all depend on one key component: an automatic "verifier" that knows if a final answer is correct". So it is not that surprising to see it solves ICPC problems. But how to build an intelligent system to solve a complex problem that even there is no deterministic definition for correctness.

The full lecture could be found [here](https://www.youtube.com/watch?v=ebnX5Ur1hBk)