---
title: "Kaggle-ion-switching(1)"
categories: 
  - Kaggle
last_modified_at: 2020-04-10 12:00:00
toc: false #Table of Contents
comments: true
use_math: true # MathJax On
---

- [Kaggle Ion Switching 과제](https://www.kaggle.com/c/liverpool-ion-switching)를 수행하기 위한 방법론으로써 `다중변수 Time Series LSTM` 도입 고민 [참고링크](https://qwerty1434.github.io/%EB%8B%A4%EC%A4%91%EB%B3%80%EC%88%98%EC%9D%98-%ED%83%80%EC%9E%84%EC%8B%9C%EB%A6%AC%EC%A6%88-LSTM%EC%97%B0%EC%8A%B5)

- signal 데이터를 가지고 시계열분석 -> 도출되는 값(y) : Open_Channels 수 (0~10)

- Hidden Markov Chain : open_chennels 기반, 다음 change에 대해 기존 확률값을 기반으로 추출하는 방법 
(이때 signal 신호세기는 어떻게 활용되는건가??)

- Description 보면, 해당 데이터가 50초 단위, Row로 보면 50만개 단위로 배치되었다고 함. (50만 -> 50만+1 : 둘 사이는 끊김이 있다고 봐야함, discontiouns)

- 배치 안에서의 값은 Continous 하지만, 배치 사이에는 Discontinous

- Train 기준 총 10개의 배치 : 총 500초의 데이터이며, 1초당 1만번의 주기 (1kHz) 를 가지고 있는 Signal 데이터임

- data = generated + noise + drift
  - noise와 drift는 제어할 수 없으면서 측정변수에 영향을 끼치는 변수
  - drift는 들쭉 날쭉한 신호에서 평균적으로 천천히 한쪽 방향으로 치우치는 경향, 즉 bias를 의미

