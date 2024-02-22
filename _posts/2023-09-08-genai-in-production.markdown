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
Trying to learn from those who have, so I attended the [LLMs in Production conference](https://home.mlops.community/public/events/llms-in-production-conference-2023-04-13)
Need to keep in mind there are many biases in the views given, pessimistic ones from those fearful of change, optimistic ones from those selling picks and shovels in a gold rush, and overoptimistic ones from the future-gazers that come out in every hype cycle
I liked the conference I was at as it balanced those viewpoints well. 
This is an attempt to summarise what I learned, mostly with the viewpoint of a company who is well versed in ML but is feeling pressure to "do something with LLMs"

## Contents
- How can we build our initial versions of GenAI applications
  - V1 using public APIs
  - Initial roadblocks to using V1
  - V2 using finetuning and self-hosting
- What we need for supporting GenAI in production
  - Engineering: MLOps, UX, Observability
  - Science: Fine-tuning, Evaluation
  - Data: Data, data, data
- What might be useful strategies to proceed?

## Intro
LLMs are the most successful type of model known as a foundation model, trained on HUGE amounts of data, basically the entire internet
only to be able to predict what the next work in a sentence would be.
In learning how to do this simple task (and particularly with the addition of instruction fine-tuning), 
researchers discovered these models could reason, solve problems, and create new, creative-seeming pieces of work,
getting the label Generative AI, most commonly seen with ChatGPT, DALL-E and Copilot.

![img.png](/assets/images/genai-in-prod/context.png)

Beyond copilots, automation of these models are still being worked on, 
and the long term future is to develop agents, capably of achieving a task e.g. ”go buy me a bue couch for my living room for less than$1000)
It’s important to realise that although copilots are where we’ve all had the most interaction with generative ai,
it’s likely that the short-term successes for many non-data-rich companies will come from large language models,
and when we get to using GenAI it may not be as a copilot

## ML Product Development
The main benefit that kept being brought up with LLMs was the ability to reach Version 1 of your product much quicker

![img.png](/assets/images/genai-in-prod/lifecycle.png)

A traditional ML product has generally required the sourcing or creation of datasets, a long process of model training and evaluation,
plus hosting of the model for inference. There have been growing numbers of off-the-shelf solutions, but these have been 
for limited use cases, and so were difficult to use as differentiators.
Due to the zero-shot/few-shot capabilities of LLMs (and the fact they're hosted behind an inexpensive API),
suddenly we have a model capable of solving a huge variety of tasks with just a bit of prompt tuning (and maybe the addition of some context)
While this version of your product may be too expensive or high-latency to use in reality, it crucially allows you to
evaluate product-market fit i.e. does the customer even want to use this.
Too many times in the past we have gone through the traditional ML product lifecycle only to discover that the customer has 
no interest in the product!

#### What does V1 look like
![img_1.png](/assets/images/genai-in-prod/version-1.png)

The user's input (e.g. their query to your system) is wrapped in a prompt and sent to an LLM API (e.g. OpenAI). 
The response can then be parsed before sending the desired result to the user. 
Evaluation of the response allows for finding prompts that work best
Evaluation can be [quite tricky for complex tasks](https://eugeneyan.com/writing/llm-patterns/#evals-to-measure-performance),
so you can get away with [LGTM](https://en.wiktionary.org/wiki/LGTM)@few for V1, but the earlier you implement
evaluation the better your product will be.
What if the model doesn’t know anything about the task? 
For example, requires knowledge of data internal to your company or information from after the model was trained
(e.g. the score from today's football game).
Then you want to somehow retrieve that relevant information and provide it to the model.

#### Retrieval Augmented Generation (RAG)
It is useful to think of the model as a reasoning engine rather than a knowledge store. 
This shift in thinking can help get the best out of foundation models, 
as they are likely to respond with any answer at all rather than say "I don't know" i.e. hallucinate.
Therefore, we will get more success from asking a question and providing relevant context,
which is true of human interactions too. Compare the question "What is the best bed?" to the question
"What is the best bed out of these 100 options, given that I am a 36-year-old male redecorating my daughter's room"
![img_2.png](/assets/images/genai-in-prod/rag.png)
Providing relevant information allows the model to respond with information not in its training data and 
also helps prevent fabrication of plausible but non-existent information.
The main limitation is the quality of the search system, if you have low recall you will not be providing the 
model with the facts it requires for its reasoning. 

Most descriptions of RAG assume using a vector search engine for this step, presumably because practitioners familiar
with LLMs are more comfortable working with embeddings of text than using information retrieval methods. 
However, there's no good reason to prefer semantic search over keyword search for this application, and generally 
the best choice for V1 is the search system that's most prevalent at your company 
(in other words don't spend your [innovation tokens](https://boringtechnology.club/) on a new search system since you likely need to spend them elsewhere in this novel product)

#### Beyond V1
If you get to the lucky place that V1 is actually used by customers, there are likely to be two major issues to contend with:
  - Accuracy isn’t sufficient
  - Cost/latency isn’t sufficient 

The main solutions for these problems outlined in this [great panel discussion](https://home.mlops.community/public/videos/cost-optimization-and-performance) were:
  - Accuracy
    - Fine tuning
  - Cost/Latency
    - Use in-house model
    - Distill/quantize in-house model

#### Accuracy 
There are a lot of performance improvements to be obtained with just the base API model. 
Improvements to RAG context can be obtained with better search systems.
Prompt engineering (e.g. few shot prompting, chain-of-thought prompting) can make large improvements
but changes to the model behind the API can render this work obsolete very quickly.
At a certain point, fine-tuning is required, particularly in domain specific applications which may not be 
well represented in training data for the API models e.g. legal, finance, health

##### Fine-tuning
- Freeze most/all weights of the foundation model, fit an additional set of weights in
the model
- Can be done at Open AI/GCP, but in the context of cost/latency section maybe
doesn’t make sense
- Relatively cheap to finetune so could be a good V1.1
- Numerous steps
  - Labelling
  - Evaluation
  - Training
- Requires: Labeled examples (data) and evaluation metrics
- Nice to have: OS foundation model, self-hosting
  “unlocking the capabilities that the pretrained model already has but are hard for users to access via prompting alone”

#### Cost & Latency
Anecdotally, cost and latency of OpenAI APIs too high for production applications, 
particularly for high-volume automated tasks or non-chat user facing applications. 
High-volume automated tasks are also the tasks most suitable for training smaller expert models that can be self-hosted
And example task from this [great panel discussion](https://home.mlops.community/public/videos/cost-optimization-and-performance)  was summarizing web pages for a web search index
Open AI API allowed them to evaluate whether LLMs were capable of task.
Self-hosted, distilled open-source model allowed them to summarise every web page in their index.

##### In-house model hosting
- LLMs are BIG
- Naive self-hosting potentially increases the cost of latency
- Need techniques to reduce the size of model while retaining performance

Note even shrinking 1000x will require engineering prowess to host effectively
![img_4.png](/assets/images/genai-in-prod/nvidia.png)

##### Quantisation
- Keep model size, reduce resolution of model weights
- For example, float32to float16 or float32 to int8
- Recently been extended to allow this to work during fine-tuning, reducing 780GB memory yo a more manageable 48GB

##### Distillation
- Use ”teacher” model (billion parameter) to generate training data to train “student” model (million parameter)
- Note these “small” models are still FAR larger than anything currently hosted at many companies

### What does V2 and beyond look like
![img_5.png](/assets/images/genai-in-prod/version-2.png)
Using some or all of these techniques, we can reach a state where the accuracy, cost, and latency/throughput of the system is acceptable for production use cases.
Note that we have re-introduced many of the steps from the ML product lifecycle we were able to leave out in the creation of V1.0,
which highlights the fact that ...
This was a recurring theme of the conference, that current gen LLMs are excellent for generic applications and proof-of-concepts,
however non-generic domains/application and production workloads are going to require a lot of custom work.
The idea of simply throwing your problems to an LLM API and walking away is heavily marketed by the API providers,
but the current reality doesn't seem to bear this out.
![img_3.png](/assets/images/genai-in-prod/img_3.png)
*Get better pic and cite*

Insert a16z app stack image compared to ml in context
https://a16z.com/emerging-architectures-for-llm-applications/

This is a “asset light” stack i.e. assumes no fine-tuning or self-hosting
Fine-tuning adds an offline data pipeline and evaluation, self-hosting requires more again

What this has really done is accelerated the trend of ML products needing science as the lynchpin to needing engineering as the lynchpin.
If your company has been investing in its MLOps capabilities, none of the above should sound worrying to you,
however if they haven't your company is at risk of falling further behind.

## Generative AI in Production
- Good Science necessary but not sufficient
  - Designing good evaluation
  - Training and fine-tuning large models is a craft
- Mostly an Engineering problem
  - This is a trend in traditional ML as seen in the recent history of MLOps
  - Generative models exaggerates this due to greater capabilities and risks
- Data ...

In part 2, we will examine some of the details of the Engineering, Science, and Data needed for succeeding with GenAI products.
