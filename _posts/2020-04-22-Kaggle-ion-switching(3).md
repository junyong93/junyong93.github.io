---
title: "Kaggle-ion-switching(3)"
categories: 
  - Kaggle
last_modified_at: 2020-04-22 12:00:00
toc: false #Table of Contents
comments: true
use_math: true # MathJax On
---

- *파이토치* 랜덤시드 관련한 링크 모음

  - torch.Dataloader(... , shuffle=True.) : [참고링크](https://wikidocs.net/60324)
    - 이때 DataLoader에는 4개의 인자가 있습니다. 
    - 첫번째 인자인 DataLoader는 로드할 대상을 의미하며, 두번째 인자인 batch_size는 배치 크기, shuffle은 매 에포크마다 미니 배치를 셔플할 것인지의 여부, drop_last는 마지막 배치를 버릴 것인지를 의미합니다.

  - torch.manual_seed(0) : [참고링크:랜덤샘플링](https://pytorch.org/docs/stable/torch.html#random-sampling)


  - 사용사례 : [참고링크](https://m.blog.naver.com/PostView.nhn?blogId=atelierjpro&logNo=221175394278&proxyReferer=https:%2F%2Fwww.google.com%2F)
