---
layout: single
title: "Transformer: Attention is all you need, NeurIPS 2017"
use_math: true
toc: true
toc_sticky: true
comments: true
categories:
- Natural Language Processing
---

[Paper Review] Transformer: Attention is all you need, NeurIPS 2017

## Goal
sequence modeling and transduction problem을 잘 푸는 것

## Challenge (Motivation)
Constraint of sequential computation

## Solution
- Based on Self-Attention mechanism (discard recurrence and convolutions)
- Add (relative or absolute) positional information of sequence to the input embeddings

## Method
Scaled dot-product QKV multi-head attention
Positional Encoding

## Experiment
Evaluation on Translation task : BLEU
Comparing Training cost


![](https://images.velog.io/images/yenguage/post/92dc6ecb-dbdf-43d9-b69e-d9ba7f8afe80/KakaoTalk_Photo_2021-12-21-22-14-41.jpeg)

![](https://images.velog.io/images/yenguage/post/e9dbe598-a495-4266-8ecd-2f33ff0d528e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-12-21%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2010.16.03.png)

source:
Vaswani, Ashish, et al. "Attention is all you need." Advances in neural information processing systems. 2017.
https://proceedings.neurips.cc/paper/2017/file/3f5ee243547dee91fbd053c1c4a845aa-Paper.pdf
