---
layout: single
title: "GraphSAGE: Inductive Representation Learning on Large Graphs, Neurips 2017"
use_math: true
toc: true
toc_sticky: true
comments: true
categories:
- Graph Neural Network
---

[Paper Review] Inductive Representation Learning on Large Graphs, Neurips 2017
GraphSAGE



![alt](/assets/images/post_images/graphsage1.png)


## Goal
improving node embedding via inductive graph neural network

## Challenge
- GCN-based inductive node embedding problem
  - transductive models cannot generalize to unseen nodes. & real world evolving graph
  - unseen node에 대해 generalize가 잘 된다는 것은, 임베딩을 만들 때 모델이 새로운 subgraphs에 대해서도 잘 align 할 수 있어야 한다는 말.
  - inductive 모델은 structure 관점에서 각 노드의 local role과 global position 둘 다를 잘 학습할 수 있어야 함.
  


## Solution
- node feature (ex. text attributes, node profile, node degrees) 및 structural feature 를 학습에 활용
- a set of **aggregator functions**가 node의 local neighborhood를 aggregate하는 법을 학습하도록 해서, test 때 unseen node에 대해서도 embedding을 잘 뽑을 수 있도록 함.


## Method
- 각 iteration 마다 각 노드는 local neighborhood information을 aggregate 한다.
  - mini-batch training
  1) 각 노드는 1-hop 이웃 표현을 agg해서 하나의 벡터로 표현
    - GraphSAGE에서 이웃이란? 
    : fixed-size의 이웃 노드를 uniform sampling. 각 iteration마다 이웃이 달라질 수 있음.
  2) central node와 1)의 agg된 이웃들 표현(single vector)을 concat
  3) concat 된 표현은 fc layer를 거침
- A set of aggregators : Mean AGG, LSTM AGG, Pooling AGG
  - Mean AGG : 이웃 노드들 벡터 표현에 대해서 elementwise mean. Symmetric (O) / Trainable (X)
  - LSTM AGG : LSTM은 permutation invariant 하지 않기 때문에 (sequential한 순서가 존재함), 이웃노드들을 랜덤하게 셔플링한 다음에 LSTM에 넣어줌. Symmetric (X) / Trainable (O)
  - Pooling AGG : 각각의 이웃노드 벡터가 FC layer를 거친 다음에 elementwise max-pooling을 한다. Symmetric (O) / Trainable (O)

<details>
<summary>unsupervised Loss for GraphSAGE</summary>
<div markdown="1">

  ![alt](/assets/images/post_images/graphsage2.png)
  - $v$ : central node $u$ 근처에서에서 fixed-length random walk를 했을 때 co-occur하는 이웃노드
  - 즉, 랜덤워크 상의 이웃 노드들과 중심노드의 표현이 유사해지도록 학습. cluster assumption을 따라가도록.
  - 논문에선 기본적으로는 unsupervised loss를 소개해주는데, fully supervised도 당연히 가능하다. 


</div>
</details>



![alt](/assets/images/post_images/graphsage3.png)
- K는 depth를 의미하는데, GraphSAGE는 forward 과정에선 immediate node만을 neighbor로 여기고, depth를 늘려가면서 더 멀리 있는 (higher connectivity) 노드들의 information 을 함께 반영할 수 있게 된다. 


## Evaluation
- node classification for testing the ability to generate useful embeddings on unseen data
- test on evolving document graphs (Citation, Reddit)
- test on multi-graph generalization experiment (Protein-Protein graph)


## ETC.


- forward 할 때는 1-hop neighbors를 대상으로 하고, unsupervised loss 계산할 때는 fixed-length random walk 로 이웃을 정한다. 헷갈릴 수도 있을 듯.
- 레스코백 교수님 논문! 
- [PinSAGE 논문]이 GraphSAGE를 토대로 recommendation에 초점을 맞춰서 이어서 하신 연구인가봄. 모델이 거의 비슷하다.


#### Source
Inductive Representation Learning on Large Graphs, Neurips 2017
https://arxiv.org/pdf/1706.02216.pdf

