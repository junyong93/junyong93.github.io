---
title: "Binary Classification Scoring Methods(3)"
categories: 
  - ML
last_modified_at: 2020-03-25 12:00:00
toc: true #Table of Contents
comments: true
use_math: true # MathJax On
---

[지난번](https://ehyun0128.github.io/ml/Scoring_Methods_over_Binary_Classification_Problem(2)/)에 이진분류 문제에 대한 머신러닝의 성능을 측정하는 다양한 평가지표에 대해 알아보았다.

많은 사람들은 그러한 평가지표들을 거의 준필수적으로 사용하고있으며, 파이썬 sklearn 패키지에서도 그러한 지표들을 라이브러리로 제공하고있다.

그런데 어느날 문득 의구심이 들었다.

너무나 당연히 쓰고있지만, 그 평가지표들의 이름에 대해서 의문이 생긴것이다.

**정확도**의 경우는 매우 심플하게 정확함을 나타내는 척도로써 그 이름이 바로 평가지표라는 것을 가르키는데 지장이 없다.


<br>
그런데 **혼동행렬**를 생각해보자.

혼동행렬은 사뭇 들었을 때, 무슨 의미인지 쉽게 파악하기가 힘들다.

오히려 이름으로인해 혼동을 야기하기 위한 행렬이 아닐까 싶은 생각도 들었다.

더욱이 **정밀도**, **재현율**, **민감도**, **특이도**라는 4가지 평가지표는, 각각 어떤 것을 가르키는지 막상 이름만 놓고 보면은 고개를 갸우뚱하게 만드는 것이 사실이다. 

~~나만 그런가..?~~


<br>
여하튼, 해당 의문을 가지고 혼동행렬과 정밀도, 재현율, 민감도, 특이도라는 4가지 **평가지표의 유래**를 찾아보았다.

구글 검색결과 한글로 된 문서에서는 그 실마리를 찾기가 어려웠다.

그러나 다행히도, 영문 위키피디아에서 그 용어들의 어원들을 반간접적으로나마 추측해볼 수 있었다.

찾은 결과를 바탕으로 스스로 추측해본, 위 4가지 용어의 어원은 다음과 같다.


<br>
## 혼동행렬 (Confusion Matrix, 때론 오차행렬)

> The name stems from the fact that it makes it easy to see if the system is confusing two classes.

혼동행렬은 심플하게, **혼동하기 쉬운 두 가지 분류에 대하여 쉽게 확인할 수 있도록 정리된 표**라는 의미에서 유래되었다고 한다.

~~의외로 정직한 제목~~

[참고 - Wiki: Confusion_matrix](https://en.wikipedia.org/wiki/Confusion_matrix)

-----


<br>
## Precision (정밀도) ,  Recall (재현율)

이들은  **Information Retrieval(IR) 정보검색 분야**에서 유래된 말이라고 한다.

>
> 즉, 정보를 검색해서 *관련된 정보(Positive)*와 *관련없는 정보(Negative)*를 얼마나 잘 걸러내서 검색해냈는가를 나타내기 위해 고안된 용어로 볼 수 있겠다.

<br>
예를들어 구글에서 우리가 어떤 정보를 찾아보는 상황을 가정해보자. 

그러면 **실제 관련된 정보(Positive)**들과 **실제 관련없는 정보(Negative)**들이 무궁무진하게 존재할 것이다. 

이 가운데 우리가 입력한 검색어를 통해 딸려나온 수집된 정보들이 화면에 나타나게된다.

여기서 Precision은 *화면에 나타난 모든 검색결과* 중에서  *진짜로 관련된 정보(True Positive)*가 수집된 비율을 나타낸다. 

내가 수집한 정보들 중에서 실제 관련된 정보를 통해, 얼마나 **"Precision 정밀하게"** 검색해냈는가를 나타내는 것이다.

Recall은 *실제 관련된 정보(Positive)* 중에 내가 수집한 *진짜로 관련있는 정보(True Positive)*들로 훌륭하게 **"Recall 재현해내고"** 있는지를 나타낸다.

<br>
<center><img src="/assets/images/200325/001.png" width="500" ></center>
<center><font size="3em">그림1. Precision과 Recall</font></center>
<br>

[참고 - Wiki: Precision_and_recall](https://en.wikipedia.org/wiki/Precision_and_recall)

-----


<br>
## Sensitivity (민감도) , Specificity (특이도)

이들은 **Medicine 의학 분야**에서 사용되는 용어라고 한다. 

>
> 의학에서 사용되는 *아픈사람들 (Positive, 양성반응: 감염되었음, 아픔)*, 그리고 *건강한 사람들(Negative, 음성반응: 감염되지않았음, 건강함)* 이 두 가지 용어를 인지하고 바라보면 좀 더 쉽게 이해가 될 것으로 보인다.

<br>
가령 어떤 제약회사가 이번 코로나19 사태를 파악하기 위해, 새로 도입한 감염테스트기를 개발해낸 상황을 생각해보자.

Sensitivity는 새로 도입한 감염테스트기가 *실제 아픈사람들(Positive)* 사이에서 검사결과 얼마나 **"Sensitivity 예민하게(민감하게)"** *진짜로 아픈사람들(True Positive)*을 골라냈는지 나타내는 것으로 이해할 수 있다.

Specificity는 *실제 건강한사람들(Negative)* 중에 감염테스트기에서 아픈 것으로 판명된 **"Specificity 특이한 사람들"**인 *가짜로 아픈사람들(False Positiive)*을 걸러낸 비율을 나타낸다. 

그러면 이 테스트기에 **"Specificity 특이체질인"** 사람들이 얼마나 발생하는지 확인하는 용도로 쓰일 수 있을 것이다.

<br>
<center><img src="/assets/images/200325/002.png" width="500" ></center>
<center><font size="3em">그림2. Sensitivity와 Specificity</font></center>
<br>

[참고 - Wiki: Sensitivity_and_specificity](https://en.wikipedia.org/wiki/Sensitivity_and_specificity)

-----

이상 오늘은 이름만 들어서는 쉽게 무슨 지표일지 떠올리기 어려운 이진분류 문제의 평가지표들의 유래(어원)에 대해 알아보았다.


끝
