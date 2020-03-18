---
title: "Binary Classification Scoring Methods(2)"
categories: 
  - ML
last_modified_at: 2020-03-18 12:00:00
toc: true #Table of Contents
comments: true
use_math: true # MathJax On
---

지난 시간엔 정답의 형태에 따른 예측 문제의 구분을 알아보았다. 

([지난 게시물-Scoring Methods over Binary Classification Problem(1)](https://ehyun0128.github.io/ml/Scoring_Methods_over_Binary_Classification_Problem(1)/))

그리고 이진분류 예측문제에서 학습된 머신러닝 모델이 얼마나 예측값을 잘 나타내고 있는지 판별할 수 있는 점수에 대해서도 알아 볼 수 있었다.

그러나 정확도는 완벽한 점수라고 하기엔 데이터 편향성에 따른 약점이 존재하였고, 보다 데이터를 디테일하게 확인할 수 있는 혼동행렬에 대해서도 알아보았다.


>
$$
정확도(Accuracy) = \frac{TN + TP}{TN + FN + FP + TP}
$$


<br>
<center><img src="/assets/images/200318/000.jpg" width="500" ></center>
<center><font size="3em">혼동행렬로 표현한 정확도(Accuracy)</font></center>
<br>


혼동행렬은 그 자체로 평가 기준이 될 수 있지만 단일값으로 나타낼 수 없다는 단점이 있다.

하지만 단일값들로 나타낼 수 있는 다른 평가점수들을 알기쉽게 표현할 수 있다는 장점도 가지고 있다.

오늘은 그러한 단일값으로 나타나는 다른 평가점수들을 알아보겠다.

-----

>
> 이진분류문제, 즉 *Positive(1), Negative(0)*인 문제라고 생각해보자.
> - Positive : 1
> - Negative : 0
>

## Precision (정밀도)

정밀도는 머신러닝 모델이 **1**이라고 예측한 것 데이터 중 실제로 **1**인 데이터의 수 이다.

>
$$
정밀도(Precision) = \frac{TP}{TP + FP}
$$


<br>
<center><img src="/assets/images/200318/001.jpg" width="500" ></center>
<center><font size="3em">혼동행렬로 표현한 정밀도(Precision)</font></center>
<br>


## Recall (재현율) = Sensitivity (민감도)

재현율(또는 민감도, 둘은 이음동의어다)은 실제 **1**인 데이터 중 머신러닝모델이 **1**이라고 예측한 데이터의 수 이다.

>
$$
재현율 or 민감도(Recall or Sensitivity) = \frac{TP}{TP + FN}
$$


<br>
<center><img src="/assets/images/200318/002.jpg" width="500" ></center>
<center><font size="3em">혼동행렬로 표현한 재현율(Recall) 또는 민감도(Sensitivity)</font></center>
<br>


## Specificity (특이도)

특이도는 실제 **0**인 데이터 중 머신러닝모델이 **0**이라고 예측한 데이터의 수 이다.

>
$$
특이도(Specificity) = \frac{TN}{TN + FP}
$$


<br>
<center><img src="/assets/images/200318/003.jpg" width="500" ></center>
<center><font size="3em">혼동행렬로 표현한 특이도</font></center>
<br>

-----

## F1 Score

F1 Score는 실제 **정밀도**와 **재현율(민감도)**의 **[조화평균](https://ko.wikipedia.org/wiki/%EC%A1%B0%ED%99%94_%ED%8F%89%EA%B7%A0)**이다.

>
$$
F_1 = 2*\frac{precision*recall}{precision + recall}
$$


>
> F1 score는 정밀도와 재현율이 비슷할 때 점수가 크다. 
> 하지만, 상황에 따라 정밀도와 재현율의 중요도가 다를 수 있으므로, 자신의 문제에서 무엇이 더 중요한 지 생각해 보아야 한다.
>
([출처:njy568의 블로그](https://jy-notepad.tistory.com/m/3))

-----

끝
