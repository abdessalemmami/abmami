---
title: "Automated Extraction of Insights from Educational Videos"
classes: wide
excerpt: "An application that extracts insights from recorded lectures or videos using natural language processing."
header:
  teaser: "assets/images/projects/vi-1.jpg"
tags: 
  - projects
  - web application
  - transcription
  - summarization
  - sentiment analysis
  - keyword extraction
  - question answering
categories: projects

---

# Introduction 

As the world adapted to the challenges of the COVID-19 pandemic, educational institutions embraced online learning through platforms like Google Meet and Zoom. To assist students in their studies, many online lectures were recorded by university professors for future reference during exam preparation. In the post-pandemic era, the reliance on these recorded lectures persists. Additionally, engagement with educational content on platforms like YouTube or Coursera has become increasingly prevalent, reflecting the integral role that online learning plays in our lives today.

However, many challenges arise for students and learners: the often extended duration of these videos. This issue is further compounded for individuals with restricted access to a reliable internet connection. Frequently, learners encounter specific queries about a particular course topic and seek precise answers. Additionally, the need for quick summaries of entire chapters or concise keywords to grasp fundamental concepts becomes apparent. These challenges underscore the evolving landscape of online education, emphasizing the importance of addressing accessibility and facilitating efficient learning.

In recent years, Artificial Intelligence has emerged as a valuable solution to tackle such challenges. With the integration of AI and automation, we can derive insights and extract pertinent information from recorded lectures and educational videos available on the internet. By leveraging recent advancements in text summarization, transcription, keyword extraction, sentiment analysis, and other natural language processing tasks, our aim has been to create a tool for online learners. This tool is designed to support individuals in their learning journey by providing assistance in navigating educational content. All of this was part of an academic project within the context of the Natural Language Processing course at ESPRIT School of Engineering in 2023.

# Project Overview 

The purpose of this academic project is to create a web application that is able to:

- Transcribe the spoken content of the lecture, converting speech into a readable text format.
- Identify and extract keywords from the lecture.
- Analyze the tone and sentiment expressed in different parts of the lecture.
- Provide relevant answers to users’ questions based on the lecture content.

Users initiate the process by uploading a video file or entering a YouTube URL containing the desired lecture. The key features of this application are transcription, keyword extraction, sentiment analysis and question answering. To technically achieve that, we will train or fine-tune natural language processing models for those different tasks.

# Technical Overview

## ⚫️ Transcription

## ⚫️ Summarization

After transcribing the video file to text, we can now work on the several functionalities that our project englobes. Starting with Text Summarization, we are essentially inputting the transcribed text to a pretrained summarization model called Pegasus with over 568M parameters. The output is a sequence of tokens selected by the model using beam search decoding algorithm. The selected sequence of tokens forms the final output summary with a fixed maximum length of 128 tokens.
For the fine-tuning part, we will use ROUGE to evaluate the Pegasus model performance after fine-tuning it on the CNN/Daily Mail dataset.

## ⚫️ Keyword Extraction

Keyword extraction is the task of automatically extracting the most relevant words and phrases from a document. These keywords can be used to summarize and understand the content of the lecture. In this project, the extracted keywords can be used to summarize the lecture and provide a quick overview of the content. Additionally, the keywords can be used to search for specific topics in the video. Our investigation into optimizing the process of keyword extraction has led us to uncover several approaches. We have identified the following methods: KeyBERT, YAKE, RAKE, and fine-tuning CamemBERT with a custom dataset. These methodologies shows potential in extracting keywords from various types of textual data.
- KeyBERT stands out as a powerful technique that utilizes BERT-based embeddings to generate representative keywords. By leveraging the contextual information captured by BERT, KeyBERT offers a comprehensive and nuanced approach to keyword extraction. 
- YAKE (Yet Another Keyword Extractor) presents a noteworthy alternative, employing an unsupervised approach based on statistical measures such as term frequency and inverse document frequency (TF-IDF) to identify salient keywords.
- RAKE (Rapid Automatic Keyword Extraction) is another notable method that relies on statistical heuristics, such as word frequency and co-occurrence, to extract keywords from text. This unsupervised technique is particularly adept at handling longer documents and can produce informative and concise keyword sets.
- Fine-tuning CAMEMBERT, a variant of BERT designed specifically for French language processing, on a custom dataset shows promise for keyword extraction tasks in French text. By adapting the pre-trained model to domain-specific or task-specific data, we can enhance its ability to accurately extract relevant keywords from French transcripts.

In this project, we will be focusing on the last approach which is fine-tuning CamemBERT on a custom dataset for keyword extraction.

### 📊 Dataset

We created a dataset by merging data from transcribed YouTube videos with the WikiNews French keywords dataset. This dataset contains approximately 340 documents and consists of two columns: "text" and "keywords." To collect the dataset, we created a tool to scrape and transcribe youtube videos using Faster-Whisper, PyTube, and yt-dlp.

📁 Github Repository: [Youtube Scraper & Transcriber Tool](https://github.com/abmami/Scraper-from-Youtube-Playlists)

We used CamemBERT, a state-of-the-art language model for French, to perform keyword extraction. CamemBERT is a pre-trained language model based on the RoBERTa architecture. It was pre-trained on 138GB of French text, which is 2.5 times the size of the French Wikipedia. CamemBERT is a powerful language model that can be fine-tuned for a variety of tasks, including keyword extraction.

Transformer-based models are the current state-of-the-art for NLP tasks. It's a neural network architecture that uses attention mechanisms to learn contextual relations between words in a text, and it was introduced in 2017 in the paper [Attention Is All You Need](https://arxiv.org/pdf/1706.03762.pdf). It can be used for a variety of tasks, including text classification, question answering, and summarization. CamemBERT-base is composed of an embedding layer to represent each word as a vector, followed by 12 hidden layers composed mainly of two types of transformations: self-attention transformations and dense transformations. The model has 12 hidden layers, 768 hidden size, 12 attention heads, and 110 million parameters.

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/projects/project-1/camembert-architecture.png" alt="CamemBERT Architecture">
</p>

You can find more details about CamemBERT [here](https://camembert-model.fr/).

### 🎯 Approaches

**Approach 1: Pre-finetuning on Sentence Similarity, followed by Fine-tuning for Keyword Extraction** 

In this approach, we first pre-finetune CamemBERT on the task of sentence similarity. By training the model to understand the semantic relationships between sentences, it gains a deeper understanding of contextual information. Subsequently, we fine-tune the pre-finetuned model specifically for keyword extraction. This two-step process allows CamemBERT to leverage its enhanced comprehension of language semantics to accurately identify and extract keywords.

**Approach 2: Direct Fine-tuning for Keyword Extraction** 

In this streamlined approach, we directly fine-tune CamemBERT for the keyword extraction task without pre-finetuning on sentence similarity. By focusing solely on the target task, we optimize the fine-tuning process and enable CamemBERT to learn directly from the keyword extraction data, and just benefit from its language representation capabilities.

In both approaches, we fine-tune CamemBERT on our custom dataset and evaluate the model performance using the F1-score metric. We also produce quantized versions of the fine-tuned models to reduce their size and facilitate deployment.

📁 Github Repository: [Approaches](https://github.com/abmami/Fine-tuning-CamemBERT-for-Keyword-Extraction).

## ⚫️ Sentiment Analysis

Next up, we have the sentiment analysis part which is used to predict the tone and motive of the speaker in the video, this will help provide more context to the situation as well as understanding the scenario. As usual, using the transcribed text as input, this time into the BERT-ABS for abstractive sentiment analysis with over a 140M parameters. The output of BERT-ABS can be represented as a set of (aspect_term, polarity_label) pairs, where aspect_term is a string representing the aspect term and polarity_label is a string representing the predicted sentiment polarity label for that aspect. The polarity label can take one of three values: positive, negative, or neutral. For the fine-tuning part, we will be using MAMS (Multimodal Aspect-based Sentiment Analysis) dataset, which provides aspect-based sentiment annotations for YouTube video comments in order to perform fine-tuning of BERT-ABS and we will evaluate the final results using F1-score.

## ⚫️ Question Answering

For this task, we will be dealing with the question answering functionality, more precisely reading comprehension of the transcribed text. We input the text and pose a question to the model and see if it gets the answer correctly. The model is based on BERT architecture but specifically trained on a large corpus of French text (138 Gb of French text) -> 110 million parameters. The output is the answer to the question asked. For the fine-tuning part, we used the camemBERT model which was fine-tuned on the FSQuAD (French Squad) dataset and fine-tuned it on the downstream task of open question answering. 

# Deployment

# Conclusion