---
layout: single
title: "Learning to Transfer Graph Embeddings for Inductive Graph based Recommendation, 2020 SIGIR"
use_math: true
toc: true
toc_sticky: true
categories:
- Graph Neural Network
- Recommendation
---

[Paper Review] Learning to Transfer Graph Embeddings for Inductive Graph based Recommendation, 2020 SIGIR

TransGRec

## Goal
Personalized video highlight Recommendation

## Challenge
- Difficult to tailor to users' unique preferences
- graph embedding models fail to generalize to unseen items
- cold-start user problem in training stage
	- Limited historical behavior record of a user to an item
- new item without any link structure at test time

## Solution
- personalized segment recommendation with many new segments in the test stage
- **Transfer the knowledge** learned from GNN to Transfer Network for the new item without any link information --> 이를 통해 **inductive learning이 가능**해짐!
- Joint training the <u>GNN</u> for cold-start problem (on training) and <u>Transfer Network</u> for missing link problem (on test)


## Method
<details>
    <summary>Bipartite Graph Construction</summary>
  
- User의 historical records를 attributed graph(Bipartite Graph)로 나타냄 : User-item(segment) rating matrix (1 if user prefer the item else 0; binary)
</details>

- Graph Neural Network
  - Learning user and item embedding with the users' historical item preferences
  - Alleviate cold-start problem
- Transfer Network
  - Aim to approximate the item embeddings from GNN by taking an item's visual content as input
  - Tackle the missing link for test (new) items

- Optimization
  - $\mathcal{L}=\mathcal{L}_{GNN}+\lambda\mathcal{L}_{T}$
 
<details>
    <summary>Optimization - GNN</summary>


    1. Graph Neural Network
  - Get visual feature from pretrained model(C3D; Convolutional 3D Network)
  - item embedding은 hybrid representation임. free item embedding과 visual embedding을 fusion(이 논문에선 단순히 adding)해서 사용함
    - free item embedding을 adding하는 것은 GNN으로 하여금 content에선 볼 수 없는 collaborative information을 학습할 수 있도록 함
  - 그 뒤엔 일반적인 GNN처럼 사용함. propagation layer에서 neighbors에 대해 pooling하고, central node에 대한 update수행. 마지막엔 prediction layer를 거쳐서 user와 item에 대한 최종 표현을 얻음.
  - 여기선 inductive하다고 볼 수는 없기에, test stage에서 unseen item에 대해 generalize를 한다고 말할 수 없음. 이 논문에선, 이를 아래의 Transfer Network를 따로 둠으로서 inductive learning을 가능케 함.
  - Preference Loss in GNN
    - BPR(Bayesian Personalized Ranking)
   ![](https://images.velog.io/images/yenguage/post/c9326788-3e18-4960-a842-90bff979900f/image.png)
     - $D_a=\{(i,j)|i \in R_a \land j \notin R_a\}$, $R_a$ represents the item set, $a$ is a user, $s$ is a sigmoid function.
     - 즉, $\hat{r}_{ai}$는 positive sample에 대한 user a 의 preference score이고, $\hat{r}_{aj}$는 negative sample에 대한 user a 의 preference score를 말한다. 
     - 따라서, negative sample에 대한 preference score가 0이고 positive sample에 대한 preference score가 1이 되면 loss가 0으로 떨어짐.
</details>


<details>
    <summary>Optimization - Transfer Network</summary>

2. Transfer Network (T)
- test stage에서 unseen node는, user의 rating이 없다. 즉, 그래프 상에선 isolation node라고 볼 수 있다. 
- item의 content feature를 transfer network로 바로 넘긴다. training 과정에서 **transfer network가 GNN의 item embedding을 approximate 할 수 있도록 학습**시킨다. 그래서 test stage에서 unseen node에 대해서도 embedding을 얻을 수 있게 되는 것.
- Transfer Network는 복잡한 구조라기 보단 단순한 MLP로 구성된다. 
- 이 논문에서 제안하는 Transfer Network optimize 방법으로 두가지를 제안한다.
  - Euclidean distance based Loss (TransGrec-E)
    - GNN으로부터 얻은 embedding과 Transfer Network로부터 얻은 임베딩을 비교하기 위한 목적
    
  ![](https://images.velog.io/images/yenguage/post/eca2dd7b-c421-4438-b22a-d57a0dd40cd1/image.png)
  - Adversarial Loss (TransGrec-A)
    - 이 부분이 굳이..? 싶기도 하면서 재밌는 부분인데 ~~(empirical하게 이게 성능이 더 좋아서일 가능성이 높겠지..?)~~, GAN처럼 discriminator와 generator 개념이 등장한다. transfer network로부터 얻은 item embedding이 (fake)가 되는 것이고, GNN으로부터 얻은 item embedding이 (real)이 되는 것.
    - 이를 위해서 discriminator network(D)를 따로 둔다. Transfer Network와 마찬가지로 MLP를 쓴다. 
    - Loss는 GAN loss와 똑같이 디자인했다.
</details>


### 기타
- inductive 셋팅에서 graph based recommendation하는 논문을 좀 더 읽어봐야 할 듯. 
- 상세하게 쓰려고 하지 말고 요약을 하자... 기억 안 나면 필기를 보면 됨. 지금은 투머치임.


### source:
https://arxiv.org/pdf/2005.11724.pdf
Learning to Transfer Graph Embeddings for Inductive Graph based Recommendation, 2020 SIGIR