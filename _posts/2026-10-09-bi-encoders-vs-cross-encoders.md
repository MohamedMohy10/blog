---
layout: post
title: "Bi-Encoders vs Cross-Encoders: Why Retrieve + Rerank Works"
date: 2026-10-09
description: "Understanding bi-encoders, cross-encoders, and reranking and why combining them can significantly improve search and RAG quality"
---

If your RAG / semantic search results feel "almost right but not quite as I expected" .. you probably suspect the embedding model, like many people do. But often the problem isn't the model, it's that your pipeline needs an extra step beyond basic semantic search. 
In this post I explain what that step is, why it works, and why it's so widely used now in production systems.

<img src="https://github.com/user-attachments/assets/84784fda-e623-4a39-8a7d-1945617ae99b" />


## 1. Bi-encoders: fast, but they never let the query and document "see" each other

Standard semantic search uses a bi-encoder. It encodes the query and each document separately into fixed-size vectors, then compares them using cosine similarity (or a similar metric).

The big advantage is speed .. you can precompute the embeddings of millions of documents once, and at query time you only encode the query and do a nearest-neighbor lookup. No need to run the model over every document again.

But there is a catch, The query and the document never directly interact inside the model, as each one is compressed into a single vector before any comparison happens.

Think of it like summarizing two essays separately, in one sentence each, and then trying to judge how well one answers the other using only those two sentences. You'll probably get the general idea right, but you can easily miss the details that matter.

A classic failure example: in a search for "cheap hotels in Vienna", a "€400/night luxury hotel" can outrank a "€25/night budget hostel" .. because the model never directly checks €400 against "cheap", it mostly sees overlapping words like "hotel" and "Vienna".

## 2. Cross-encoders: a much more precise comparison

A cross-encoder works differently. It takes the query and the document together as one input and runs them through a single transformer, so every query token can attend to every document token. The output The output is a direct relevance score for that specific query-document pair rather than an embedding as in bi-encoders. 

That's how it can catch things a bi-encoder can't .. when the query says "cheap" and the document says "€400 per night", the model can evaluate those two details together and learn that this is probably not a good match. 
It's widely used in professional settings and has been discussed in many papers, but I still find many people do not know about it !!

But of course, there is a trade-off: cost.

Bi-encoders precompute all document embeddings once. Cross-encoders can't precompute anything, because the score depends on the query. You need a full forward pass for every query-document pair, and running that over a million documents for every query is a non-starter, it's computationally very expensive.

## 3. The solution: retrieve first, rerank after

Instead of choosing one or the other, the industry standard is to combine both in a two-stage pattern:

- **Stage 1: Retrieve** .. a cheap bi-encoder (or BM25) pulls the top 20-100 candidates from the full corpus. The goal here is recall, don't miss the relevant documents.
- **Stage 2: Rerank** .. a cross-encoder scores only those candidates against the query and reorders them by true relevance. The goal here is precision.

Optionally, there's a third stage .. an LLM can rerank the final top ~10 candidates. Unlike a cross-encoder (which scores each pair independently), an LLM can see all the candidates together and reason comparatively, like "A is more relevant than B because..". It's slower and more expensive per call, but it only touches a handful of documents, so it can be worth it for important use cases.

The idea is simple: instead of asking an expensive model to examine millions of documents, you let it focus on the few dozen that are already likely to be relevant.

## 4. A note on training (if you ever fine-tune one)

You usually don't train a cross-encoder from scratch .. you fine-tune a pretrained language model (BERT, RoBERTa, MiniLM, etc.) on query-document pairs labeled as relevant or not. MS MARCO is a common base dataset for this.

Domain fine-tuning can matter a lot. A model trained on general web search doesn't necessarily know that "force majeure" is a contract term, not a military phrase.

And one thing that matters: **hard negatives** .. These are distractors from the same domain that share vocabulary with the query but differ in meaning (like a contract-dispute document for a tort-law query). Easy, unrelated negatives (recipes vs legal text) teach the model almost nothing, because it can already tell those apart trivially.

## 5. The middle ground: ColBERT

There's also ColBERT (Contextualized Late Interaction over BERT), which sits in between. Instead of compressing a document into one vector, it keeps a separate embedding for each token (no pooling), with the query and document still encoded independently. The comparison only happens at the end, at the token level: for each query token, it finds the best-matching document token, then combines those scores.

Because documents are still encoded independently, you can precompute and index them in advance, while getting quality that's often close to a cross-encoder.

So conceptually, you have three approaches:

- **Bi-encoder:** encode separately, compare single vectors. Fastest, precomputable, lowest precision.
- **ColBERT (late interaction):** encode separately, compare at token level. Documents can be precomputed, with quality closer to a cross-encoder.
- **Cross-encoder:** process the query and document together. Most precise, but nothing can be precomputed, so it's the most expensive.

## My takeaway

You do not always need to replace your embedding model to improve retrieval quality. 
Don't try to make one model do everything. Use a cheap model to narrow down millions of documents to a few dozen, then a precise model to rank those few.

In many cases, adding a reranking step gives a bigger quality win than switching to a different embedding model. So next time your retrieval results feel "almost right", before swapping embedding models, try adding a reranker first.
