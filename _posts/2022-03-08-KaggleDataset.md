---
layout: single
title: "Kaggle Dataset 로컬에 다운로드하기"
use_math: true
toc: true
toc_sticky: true
comments: true
categories:
- Kaggle
---

Kaggle dataset 다운로드

1. kaggle api 다운로드

pip install --user kaggle

2. 토큰 발급
- Kaggle 사이트에서 Account 페이지로 가서 Create New API Token으로 토큰을 발급받는다. 
![](/assets/images/post_images/kaggle_dataset.png)

- kaggle.json이라는 파일이 다운로드 된다. kaggle 폴더로 옮겨주고 권한설정을 해준다. 파일을 열어보면 username과 발급받은 key 값이 들어있는 걸 확인할 수 있다.

mkdir ~/.kaggle
mv kaggle.json ~/.kaggle
chmod 600 ~/.kaggle/kaggle.json

3. 데이터셋 다운로드

kaggle datasets download <USER_NAME>/<DATASET_NAME>

예를들어, https://www.kaggle.com/mlg-ulb/creditcardfraud 에서 Credit Card Fraud Detection 데이터셋을 다운로드 하고 싶은 경우에는,
USER_NAME이 mlg-ulb가 되고, DATASET_NAME이 creditcardfraud이다.

kaggle datasets download mlg-ulb creditcardfraud

4. 다운로드 완료




https://www.kaggle.com/<USER_NAME>/account
{"username":<USER_NAME>,"key":<API_KEY>}
