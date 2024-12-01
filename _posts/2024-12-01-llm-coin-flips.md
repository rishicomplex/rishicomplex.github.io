---
layout: post
title:  "LLM Coin Flips"
date:   2024-12-01 00:00:00 -0800
---

"Can an LLM toss an unbiased coin?" came up in a converstion with a friend recently. On the one hand, it shouldn't be hard for an LLM to understand the concept of "unbiased" and output equal logits over the tokens for "Heads" and "Tails". On the other hand, the training distribution is almost certainly not equally split between the two. I checked GPT-4o, Gemini 1.5 Pro and Claude 3.5 Sonnet.

<!--more-->

Prompt: Toss an unbiased coin. Output nothing except "Heads" or "Tails". They should each be equally likely. Do not use any tools, just output the result.

The OpenAI API lets you look at the logprobs of the top tokens. For GPT-4o, the probabilities for the top 5 tokens are:

```
Heads: 0.68
T: 0.32
Sure: 0.00
To: 0.00
Certainly: 0.00
```

The Gemini and Claude APIs don't provide logprobs, so I just sampled 20 times from each with a temperature of 1.0.

```
Gemini 1.5 Pro: 20/20 Heads, 0/20 Tails
Claude 3.5 Sonnet: 12/20 Heads, 8/20 Tails
```

Which gives us:

| Model | P(Heads) | P(Tails) |
|-------|----------|----------|
| GPT-4 | 0.68 | 0.32 |
| Gemini 1.5 Pro | 1.00 | 0.00 |
| Claude 3.5 Sonnet | 0.60 | 0.40 |

All three models are biased towards "Heads". The bias is very strong with Gemini - almost looks like there may be a bug. I checked that I'm using a temperature of 1.0, though I'm not sure if there's caching going on somewhere. I find this to be an interesting eval. Humans probably can't give you perfectly random coin flips, though I expect they'd do better than this. It'd be interesting also to see what happens if you condition subsequent coin flips on previous ones.

Code is in [this notebook](https://github.com/rishicomplex/coins/blob/main/toss_coins.ipynb).

EDIT: Turns out someone wrote [a paper](https://arxiv.org/pdf/2406.00092) on this topic, which found a stronger bias towards "Heads" in previous generation models.