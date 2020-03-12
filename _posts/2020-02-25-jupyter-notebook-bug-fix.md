---
title: "[개념정리/Bug Fix]-Jupyter Notebook과 사용중 발생했던 버그이야기"
categories: 
  - Miscellaneous
last_modified_at: 2020-02-25 12:00:00
toc: true #Table of Contents
comments: true
---

데이터분석을 할때에 대표적으로 사용되는 두 가지 언어가 있다.

R과 Python이 그것이다.

나는 학생시절에는 R을 자주 사용했지만, 언제부턴가 Python으로 넘어와서 현재는 Python을 익숙하게 사용하고 있다.

분석언어로써 R과 Python을 비교하는 글들은 인터넷상에 많이 올라와있는데, 나도 언젠가 둘을 비교하는 내용의 글을 정리해봐야겠다.

이야기가 잠시 샜는데, 오늘 정리할 내용은 분석가들이 Python으로 데이터분석 시 즐겨 사용하는 IDE(Integrated Development Environment:통합개발환경) 툴인 Jupyter Notebook과 이를 사용하다 겪은 오류를 Fixing해가는 이야기를 담고자 한다.
 


### Jupyter Notebook(주피터* 노트북)
*파이썬의 느낌을 강조하려 '주<파이>터'로 읽는 경우가 있다고 한다*

<br>
<center><img src="/assets/images/200225/000.png" width="500" ></center>
<center><font size="3em">Jupyter의 Logo</font></center>
<br>

- Jupyter Notebook은 Notebook이라는 파일안에서 코딩 결과를 바로바로 결과를 확인하며 작업할 수 있게해주는 Interactive한 기능을 가지고 있다.(개인적으로 Jupyter Notebook의 핵심기능이라고 생각하는 것이 바로 이 Interactive 기능이다)

- Interactive 기능을 통해 데이터분석가는 그래프나 결과테이블을 코딩한 코드와 함께 놓고 볼 수 있으며, 이 자체로 보고서의 포맷이 완성될 수 있다는 편리함도 가지고 있다. 

- 덤으로 Markup Language 형식도 지원하기에 html, image, video, Latex 수학기호 등 단순 텍스트보다 많은 정보를 담을 수 있다는 점도 보고서 형식에 더 적합하다는 특징을 가져간다.

- 개발 지원기능에서 필수요소로 생각하는 변수 또는 패키지 이름 자동완성 기능도 거뜬하다.

- 사실 Jupyter Notebook은 Python 뿐 아니라 R, Julia, Scala 등 40여개의 언어를 지원하는 개발툴이라고 한다.


<br>
<center><img src="/assets/images/200225/001.png" width="500" ></center>
<center><font size="3em">Jupyter Notebook Preview</font></center>
<br>


### 내가 겪은 Bug Story
------

<br>
<center><img src="/assets/images/200225/002.png" width="500" ></center>
<center><font size="3em">윈도우10 CMD - Jupyter Notebook 실행 시 Error 메시지</font></center>
<br>

- Jupyter Notebook은 Web Application 형식 서비스로서, 실행시에 위 그림과 같은 cmd 창과 함께 웹브라우저(크롬..) 창을 통해 실행화면이 뜨게된다.

- 위 cmd 창에는 Jupyter가 구동되면서 발생하는 이슈 로그들이 기록되는 일종의 콘솔로 볼 수 있다.

- 그런데 해당 cmd 장을 보면 Jupyter Notebook 실행 명령어 바로 다음라인에 Error 메시지를 확인할 수 있다. (`Error loading server extension ipyparallel.nbextension ...`)

- 이와 같은 현상의 원인은 __1)분석에 사용되는 라이브러리 패키지 설치를 잘못했을 때__, __2)Path 경로 등 환경설정이 잘못했을 때__ 발생하는 것으로 추정된다.

- 내가 겪었던 상황은 __Jupyter Notebook을 종료하지 않은채, 패키지를 여러벌 설치했던 것__ 때문에 1)라이브러리 패키지 설치가 잘못되어 문제가 발생하지 않았나 추측하고 있다.

- 일단 위에 발생한 에러메시지를 보면 ipyparallel 모듈을 찾을 수 없다고 나온 것으로 보아, 해당 라이브러리 패키지 모듈을 pip를 이용하여 설치한 것으로 에러는 해결했다.

- 그러나 새로운 문제들이 발생하는데, 기존에 잘 실행되던 Python 시각화 라이브러리 모듈인 'Seaborn' 그리고 머신러닝 필수 패키지인 'Sklearn' 등이 제대로 동작하지 않는다는 점이었다.

~~버그는 언제나 꼬리를 물고..~~

- 이러한 이슈들로 인해 환경설정 부담으로 인해 우회적인 해결책을 선택할 수 밖에 없었는데...

- 결론적으로 이 문제를 해결한 방법은 기존에 사용하던 방식인 윈도우 cmd 창에서 jupyter notebook을 실행하는 방식 대신, Anaconda를 이용하여 새로운 환경을 적용하고 jupyter notebook을 부르는 방식으로 대체한 것이다.

~~설치나 환경설정에서 발생한 문제의 근본적인 해결방법은 다 밀어버리고 다시 설치하는 방법일 뿐~~

>
> 해결 : cmd 창에서 아래 명령어들 입력하여 실행
> > conda activate [실행하려는 콘다가상환경이름]
> > start jupyter notebook
> 결과 : --정상동작--
>

<br>

그럼 이번 시간은 여기까지. 이만 총총
