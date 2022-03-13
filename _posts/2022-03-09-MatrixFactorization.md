---
layout: single
title: "Matrix Factorization Techniques for recommender systems"
use_math: true
toc: true
toc_sticky: true
comments: true
categories:
- Recommendation
---

Goal : Personalized recommendation

Challenge : 
- Sparse matrix of explicit feedback 
- scalability
- Generalize previous ratings to predict unknown ratings
- not all observed ratings deserve the same weight or confidence

Solution : 
- incorporate additional information to overcome cold-start issue - leverage explicit & implicit feedback
- Imputation to fill missing ratings and make dense matrix by directly observed ratings only.
- avoid overfitting the observeddata by penalty regularization
- Compute/Observe confidence on recurring events to reflect user opinion

## Recommender System
- Content based Filtering
user나 item을 그 자체로 특징지을 수 있는 profile을 만드는 것을 말한다.
예를들어, 아래와 같은 정보를 토대로 profile 생성할 수도 있다.
    - user profile : 인구통계학 정보, 사전에 주어진 질문에 대한 답변 
    - item profile (movie) : 영화의 장르, 배우들, 박스오피스 순위 

장점 : profile을 기반으로 user-item 매칭을 용이하게 할 수 있다. 또한, Collaborative Filtering과 비교해봤을 때, 새로운 user나 item
단점 : profile을 생성하기 위한 external information이 필요하다.

- Collaborative Filtering
user의 과거 behavior를 토대로 추천을 하는 방법이다. 
예를들어, 과거 item 구매 이력이나 item에 매긴 rating이 흔히 사용된다. 핵심은 user-item 사이의 interdependency를 파악함으로써 그들 간의 relationship을 활용하여 추천을 진행하는 것이다. 

장점 : (content-based filtering과는 다르게) explicit profile 생성을 위한 external information이 요구되지 않는다. 따라서, external information이 부족한 상황에서도, 구매 이력 등의 로그를 사용해서도 추천을 할 수가 있어진다.
단점 : cold-start problem에 취약하다. 예를들어, 서비스에 신규 가입자인 user 혹은 새롭게 등록된 item은 구매/판매 이력이 존재하지 않고, 이는 relationship이 아직 없다는 말과 동일하다. 따라서, user-item relationship에 dependent한 collaborative filtering을 통해선 적합한 추천이 이루어지지 않을 가능성이 높고, 오히려 이 경우에는 content-based filtering의 성능이 더 우수할 것이라고 자연스럽게 예상할 수 있다.

## Collaborative Filtering

### 1. neighborhood methods

- item-oriented approach

user가 rating을 매긴 다른 neighboring items들을 토대로 새로운 아이템에 대한 user's preference를 예측하는 것이다. 이때 neighboring items란, 동일한 user로부터 비슷한 평점(rating)을 받은 다른 item들을 말한다.

예를들어, 애플 전자기기 item들에 높은 평점을 준 user A가 있다고 하자. 만약 신상 애플워치 item이 나온다면, 이 user A는 지금까지 그랬듯이 비슷하게 높은 평점을 줄 가능성이 크다고 예측할 수 있다. 즉, **평소 아이폰, 아이패드, 맥북을 좋아했다면, 신상 애플워치도 좋아할 것이라고 추측**하는 것이다.

- user-oriented approach

위의 item-oriented approach와 비교하자면, item 중심에서 user 중심으로 대상이 변한다는 것의 차이라고 볼 수 있다. 

예를들어, 같은 영화 취향을 공유하는 user A,B,C,D가 존재한다고 가정하자. user A에게 새로운 영화를 추천해주려고 하는 상황에서, user B,C,D 중 상당수가 공통으로 좋아하는 영화 X를 추천해주는 방법이다. 지금까지 그래왔듯이, **이 4명의 유저들은 비슷한 영화 취향을 갖고 있으니까, 집단 내 다수가 좋아하는 영화 X가 user A의 취향을 저격할 확률도 높을 것**이다. 

![](/assets/images/post_images/MF_fig1.png)

### 2. latent factor models

user와 item에 대한 특징들을 뽑아내서 rating을 예측하는 방법이다. 

![](/assets/images/post_images/MF_fig2.png)
Figure 2에서는 간단하게 2차원으로 예시를 보여주고 있는데, x축은 females vs. males
