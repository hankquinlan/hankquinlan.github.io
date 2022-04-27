---
layout: post
title:  "Parameter Efficient Transfer Learning"
date:   2022-03-03 18:00:00 +0000
categories: Natural Language Processing
permlink: /:title
---

This post is based on a talk by Jordan Clive on Parameter Efficient Transfer Learning a few weeks ago. I had taken language model fine tuning for granted, but it is clear that in the case of large language models this approach is infeasible without huge resources. A few months ago I completed the Hugging Face NLP course, a lot of which involved fine tuning models. However, in some cases this proved to be impossible as the resources provided by Colab were unequal to retraining a huge model, so these assignments were left undone. What follows describes approaches to make reusing large models more feasible.

Until recently, language model adaptation was done via full fine-tuning, which requires storing and updating all of the model parameters. 
As the use of ever larger pre-trained models continues, this overhead is no longer feasible. Another problem is that an updated model needs to be saved, which can be a problem with a very large model.

This is a rapidly expanding research field and of increasing interest to industry and companies like OpenAI. A large language model can be potentially reused across many different tasks. This research is essentially about simplifying the adaptation of language models to downstream tasks.

There are three main kinds of tuning:

- Prompt Tuning
- Prefix Tuning
- Adapter Tuning

The Power of Scale for Parameter-Efficient Prompt Tuning by Lester et al explores prompt tuning, "a simple yet effective mechanism for learning 'soft prompts' to condition frozen language models to perform specific downstream tasks." The authors show that prompt tuning becomes more advantageous when used at scale.

In Prefix-Tuning: Optimizing Continuous Prompts for Generation by Li and Liang (2021), prefix-tuning is described as a "lightweight alternative to fine-tuning for natural language generation tasks, which keeps language model parameters frozen, but optimizes a small continuous task-specific vector (called the prefix)." Prefix tuning performs as well as fine tuning in the full data setting and even outperforms fine tuning in low-data settings.

UnifiedSKG: Unifying and Multi-Tasking Structured Knowledge Grounding with Text-to-Text Language Models by Xie et al discusses the limits systematic and compatible research on Structured Knowledge Grounding (SKG) by proposing the UnifiedSKG framework to unify 21 SKG tasks into a text-to-text format. They go on to demonstrate that multi-task prefix-tuning improves the performance on most tasks.

"Fine-tuning large pretrained models is an effective transfer mechanism in NLP. However, in the presence of many downstream tasks, fine-tuning is parameter inefficient: an entire new model is required for every task. As an alternative, we propose transfer with adapter modules. Adapter modules yield a compact and extensible model; they add only a few trainable parameters per task, and new tasks can be added without revisiting previous ones.
Techniques such as Prompt-Tuning and Adapters are at the forefront of a new paradigm in adapting large language models to downstream tasks." - Houlsby et al (2019)

Adapters are a light-weight alternative to full model fine-tuning, consisting of only a tiny set of newly introduced parameters at every transformer layer. AdapterHub https://adapterhub.ml/ is a central repository for pre-trained adapter modules.

BitFit: Simple Parameter-efficient Fine-tuning for Transformer-based Masked Language-models by Zaken et al introduces BitFit, a sparse fine tuning method where only the bias terms of the model (or a subset of them) are modified. BitFit on pre-trained BERT models is reported to be just as effective and aometimes better than fine-tuning the entire model. The findings of the paper seem to support the hypothesis that fine tuning is mainly about exposing knowledge induced by training the language model, rather than learning new task-specific linguistic knowledge.

Pre-train, Prompt, and Predict: A Systematic Survey of Prompting Methods in Natural Language Processing by Liu et al provides a survey of prompting methods in NLP. Prompt-based learning is based on language models that model the probability of text directly. This allows the language model to be pre-trained on massive amounts of raw text, and by defining a new prompting function the model is able to perform few-shot or even zero-shot learning, adapting to new scenarios with few or no labeled data.

It appears that prefix tuning is more powerful, providing better and more linguistically fluent results. However, this contention is based on experimental results and it is not clear why this should be the case.

Another approach to paramter efficiency is to use low rank methods for adapter tuning. In Compacter: Efficient Low-Rank Hypercomplex Adapter Layers by Mahabadi et al note that recent work has developed parameter-efficient fine-tuning methods, but these approaches either still require a relatively large number of parameters or underperform standard fine-tuning. They propose a method for fine-tuning large-scale language models with a better trade-off between task performance and the number of trainable parameters than previous approaches.

Another low rank approach is provided by LoRA: Low-Rank Adaptation of Large Language Models from Hu et al. This appoach reduces the number of trainable parameters by learning pairs of rank-decompostion matrices while freezing the original weights. This reduces the storage requirement for large language models adapted to specific tasks and enables efficient task-switching during deployment all without introducing inference latency. 

UniPELT: A Unified Framework for Parameter-Efficient Language Model Tuning by Mao et al provides an approach that incorporates different parameter-efficient language model tuning (PELT) methods as submodules and learns to activate the ones that best suit the current data or task setup. This seems to indicate that a mixture of multiple PELT methods may be inherently more effective than single methods.

The lastest research has focussed on unified frameworks that establish connections between the various parameter efficient transfer learning methods and provide new fine tuning methods that tune less parameters than previous methods in isolation.
