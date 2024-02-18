---
layout: post
title:  "Generative AI in Production"
date:   2023-09-08 17:26:36 +0000
categories: []
---



>People without dirty
hands are wrong.

_Cult of Done Manifesto_
I haven’t built any gen ai
Trying to learn from those who have
Need to keep in mind there are many biases in the views given, pessimistic ones from those fearful of change, optimistic ones from those selling picks and shovels in a gold rush, and overoptimistic ones from the futergazers that come out in every hype cycle
I liked the conference I was at as it balanced those viewpoints well
## What will we learn
- How can we build our initial versions of GenAI applications
  - V1 using public APIs
  - Initial roadblocks to using V1
  - V2 using finetuning and self-hosting
- What we need for supporting GenAI in production
  - Engineering: MLOps, UX, Observability
  - Science: Fine-tuning, Evaluation
  - Data: Data, data, data
- What might be useful strategies to proceed?

LLMs most successful type of model known as a foundation model, trained on HUGE amounts of data, basically the entire internet only to be able to predict what the next work in a sentence would be
In learning how to do this and with more instruction fine tuning, we discovered these models could reason and solve problems and got the label generative AI, most commonly seen with ChatGPT and copilot
Beyond copilots, automation of these models are still being worked on, and the long term future is to develop agents, capably of achieving a task e.g. ”go buy me a bue couch for my living room for less than$1000)
It’s important to realise that although copilots are where we’ve all had the most interaction with generative ai, it’s likely that the short-term successes we’ll see will come from large language models, and when we get to using GenAI it may not be as a copilot​

## ML Product Development
![img.png](/assets/images/genai-in-prod/img.png)
V1 allows you to evaluate product market fit -> Does the customer even want to
use this

LLMs allow you to get to V1 much quicker

### What does V1 look like
![img_1.png](/assets/images/genai-in-prod/img_1.png)
- Evaluation of the output allows for finding prompts that work best
  - Evaluation can be verrry tricky for complex tasks, so can get away with LGTM for V1
- What if the model doesn’t know anything about the task?
  - For example, requires knowledge of internal data or information from 2023

#### Retrieval Augmented Generation (RAG)
Think of the model as a reasoning engine rather than a knowledge store.
More success from asking a question and providing relevant context
![img_2.png](/assets/images/genai-in-prod/img_2.png)
Providing relevant information allows the model to respond with information not in
its training data. 
Limited by the quality of the search system, with low recall you’re in trouble. 
Imagine using Teams search vs Slack search for a documentation assistant

### Beyond V1
- Likely to be two major issues to contend with if V1 is used by customers
  - Performance isn’t sufficient: Not accurate enough
  - Cost/latency isn’t sufficient: Calling out to an external API is expensive and slow
- Solutions for these problems
  - Performance
    - Fine tuning
  - Cost/Latency
    - Use in-house model
    - Distill/quantize in-house model


![img_3.png](/assets/images/genai-in-prod/img_3.png)
*Get better pic and cite*

#### Performance 
*Need better word than performance, too close to latency*
▪ There are a lot of performance improvements to be obtained with just foundation
model
– RAG and improvements to context
– Prompt engineering: Few shot prompting, chain-of-thought prompting
▪ At a certain point, fine-tuning is required, particularly in domain specific
applications which may not be well represented in training data
– Particularly true for specialized domains e.g. legal, finance, health
– Likely to be true for “backroom” aspects of e-commerce

Fine-tuning
▪ Freeze most/all weights of the foundation model, fit an additional set of weights in
the model
▪ Can be done at Open AI/GCP, but in the context of cost/latency section maybe
doesn’t make sense
– Relatively cheap to finetune so could be a good V1.1
▪ Numerous steps
– Labelling
– Evaluation
– Training
▪ Requires: Labeled examples (data) and evaluation metrics
▪ Nice to have: OS foundation model, self-hosting
“unlocking the capabilities that the pretrained model already has but are hard for users to access via prompting alone”
#### Cost & Latency
▪ Anecdotally cost and latency of OpenAI APIs too high for production applications,
particularly for high-volume automated tasks
▪ These are also the tasks most suitable for training smaller expert models that can
be self-hosted
▪ Example task was summarizing web pages for a web search index
– Open AI API allowed them to evaluate whether LLMs were capable of task
– Self-hosted open-source model allowed them to summarise every web page in their index

In-house model hosting
▪ LLMs are BIG
▪ Naïve self-hosting potentially increases the
cost of latency
▪ Need techniques to reduce the size of model
while retaining performance
Note even shrking 1000x will require engineering prowess to host effectively
![img_4.png](/assets/images/genai-in-prod/img_4.png)

Quantisation
▪ Keep model size, reduce resolution of model
weights
▪ For example, float32to float16 or
float32 to int8
▪ Recently been extended to allow this to work
during fine-tuning, reducing 780GB memory
to a more manageable 48GB

Distillation
▪ Use ”teacher” model (B parameter) to generate training data to train “student”
model (M parameter)
Note these “small” models are still FAR larger than anything we currently host at overstock

### What does V2 and beyond look like
![img_5.png](/assets/images/genai-in-prod/img_5.png)

Insert a16z app stack image compared to ml in context
https://a16z.com/emerging-architectures-for-llm-applications/​

This is a “asset light” stack i.e. assumes no fine-tuning or self-hosting​

Fine tuning adds an offline data pipeline and evaluation, self-hosting requires more again
## Generative AI in Production
- Good Science necessary but not sufficient
  - Designing good evaluation
  - Training and fine-tuning large models is a craft
- Mostly an Engineering problem
  - This is a trend in traditional ML as seen in the recent history of MLOps
  - Generative models exaggerates this due to greater capabilities and risks
- Data ...

### Engineering: Pipelines
### Engineering: Monitoring & Observability
### Engineering: UX
### Engineering: Guardrails
### Science: Fine-tuning
### Science: Evaluation
### Data: Training and evaluation
### Data: Generatively Generated

## Practical next steps
Chip huyen link
https://huyenchip.com/2023/06/07/generative-ai-strategy.html

## Conclusion
A generative culture
- Big Data infrastructure has always been rock solid here
- Addition of ML workloads created new valuable datasets
- Creation of infrastructure allowed new ML workloads
- Ultimately the value from all this is generated from the use cases
- Success in areas drove improvements in other areas
- Even failures lead to learnings that have improved later use cases

▪ Generative AI allows getting to proof of concept much quicker
▪ Data data data
– “Private data is a durable moat”
▪ Engineering problem more than a science problem
▪ We’re lucky that Overstock has a deep data and ML infrastructure
▪ What are the use cases?
– The more use cases we work on, the more we understand the powers and limitation of these
systems