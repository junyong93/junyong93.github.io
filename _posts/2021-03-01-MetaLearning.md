---
title: "1. Meta-Learning:Learning to Learn Fast"
categories: 
  - AI
last_modified_at: 2021-03-21
toc: false #Table of Contents
comments: true
use_math: true # MathJax On
---

## 들어가며

연구실에서 진행하는 건설 하자데이터 처리 과제에 참여하게 될 것 같고, Few-Shot Learning 부분을 담당하게 될 것 같다. 
Few-Shot Learning은 적은 사진으로 학습하여 좋은 성능을 내는 이미지 처리 방식이다. 일반적인 CNN같은 이미지 처리 방식에서는
한개의 모델을 학습하기 위해 대량의 데이터가 필요하지만 건설 산업은 일회성 산업(한개의 건물을 지으면 끝)이라 데이터를 획득한다
한들 다른 프로젝트에 적용하는게 다른 산업보다는 힘들다. 그렇기에 적은 사진으로도 학습이 가능한 Few-Shot Learning 기술을 
사용하게 되는데 이 때 사용되는 Meta-Learning에 대해서 첫 블로그 글을 쓰고자 한다.

이 글은  GPT를 만든 OpenAI Enigneer인 Lilian Weng의 Meta-Learning:Learning to Learn Fast을 참고했다.

>
> "Learning To Learn"이라고 알려져 있는 Meta-Learning은 몇몇 training 예제를 통해서 모델로 하여금, 새로운 기술을 배우거나, 새로운 환경에 빠르게 적응하는 것을 나타낸다.
> 1. Metric 기반의 **Efficient Distance Metric**을 학습하는 방식
> 2. Model 기반의 **External/Internal Memory를 통한 Recurrent Network을 사용**하는 방식
> 3. Optimization 기반의 **Fast Learning을 위한 Model Parameter를 최적화** 하는 방식
>
>

머신러닝을 통해서 좋은 모델을 만들기 위해서는 일반적으로 수많은 샘플을 활용하여 학습을 시키는 것이 필요하다. 이와 반대로 인간의 경우에는 훨씬 빠르고 효율적으로 새로운 개념이나 기술들을 학습한다. 고양이나 새를 많이 보지 않은 아이들이라고 할지라도 빠르게 그걸 구분할 수 있다. 자전거를 타는 방법을 아는 사람들은 시연 같은 과정 없이도 빠르게 오토바이를 타는 방법을 반견하곤 한다. 머신러닝에서도 이와 같이 적은 샘플만 가지고도 새로운 개념과 기술을 빠르게 학습하는 것이 가능할까? 이것이 바로 본질적으로 Meta-Learning이 풀고자 하는 문제점이다.

보통 좋은 Meta-Learning Model이라고 하면, Training Time동안에 접하지 않았던 새로운 Task나 Environment에 대해서 잘 적응하거나, 일반화가 잘 되는 것을 말한다. Adaptation Process(본질적으로 말하지면 Mini Learning Session)은 Test 과정에서 일어나게 되는데, 사실 이때에는 새로운 Task Configuration에 대해서 제한적으로만 노출되어 있다. 어째뜬, 이렇게 적응된 모델은 새로운 Task를 잘 수행할 수 있다. 이 것이 바로 Meta-Learning이 **Learning to Learn** 이라고 알려져 있는 이유이기도 하다.

여기서 Task란 Supervised Learning이나 Reinforcement LEarning과 같이 Machine Learning으로 정의될 수 있는 모든 문제들이 될 수 있다. 예를 들어 Meta-Learning Task를 나열하면 다음과 같다.
- 고양이가 없는 이미지를 학습시킨 Classifier도 몇개의 고양이 사진을 본 후에는 Test Image상에 고양이가 있는지 여부를 판단할 수 있다.
- 게임 봇이 새로운 게임에 대해서 빠르게 마스터 할 수 있다.
- 평평한 지면 환경에서만 학습해온 미니 로봇이 경사진 환경에서도 Task를 수행할 수 있다.





다양한 사람들이 만나게되는 스터디에서는, 다양한 사람들의 개성처럼 협업스타일도 다양하게 진행된다.

예를들면 직전에 진행했던 캐글스터디에서는 각자 분석을 진행한 후, 캐글에서 제공하는 노트북을 통해 코드를 공유하였다.

필요 시 다른 팀원의 노트북을 Fork하여 재사용했으며, 노트북 아웃풋 데이터를 새로운 본인의 노트북에 연결하여 파이프라인을 구성하였다.

그리고 이번 새로운 스터디에서는 Github와 Docker를 활용하여 협업을 진행하게되었다.

문제는, 나 자신이 Github와 Docker를 제대로 사용해 본 적이 없다는 것이다.

오늘은 나의 **Windows 10 로컬 환경**에서 **Docker**와 **Git**을 설치하고 환경을 구성한 과정을 간략하게 정리해보고자 한다.

## Docker 설치

아래 내용은 [링크](https://steemit.com/kr/@mystarlight/docker)에 잘 정리되어있으니 참조하면 좋을 것 같다.

진행도중 재부팅할 사항이 있으니, 다운로드 받으면서 중요한 작업들은 미리 저장해 놓도록 하자.

#### 1. Windows 10 의 경우 Docker Desktop을 다운로드 받는다. 

#### 2. `제어판 > 프로그램 > 프로그램 및 기능 > Windows 기능 켜기/끄기` 에서 "Hyper-V" 하위항목을 전부 체크 후 확인한다.(재부팅 필요)

<br>
<center><img src="/assets/images/200409/000.png" width="500" ></center>
<br>


#### 3. 다운로드된 Docker Desktop 파일을 설치한다.

#### 4. 설치 완료되면 CMD 창을 열고 `docker --version`을 입력하여 설치완료여부를 확인한다.

<br>
<center><img src="/assets/images/200409/001.png" width="500" ></center>
<br>


## Docker를 이용해 Kaggle Python 이미지를 로딩

#### 1. CMD 창에서 `docker pull gcr.io/kaggle-images/python`을 입력한다. 

이는 [Kaggle에서 이미 docker 허브에 올려놓은 이미지](https://console.cloud.google.com/gcr/images/kaggle-images/GLOBAL/python)를 pull 명령어로 다운받는 명령어이다.

*참고. CPU 전용 파이썬 이미지와 GPU 가능 이미지는 별도로 구성되어있다.
	- CPU-only: gcr.io/kaggle-images/python
	- GPU: gcr.io/kaggle-gpu-images/python

이미지가 다운로드 되었다면, 이제 docker에서 해당 파이썬을 띄울 준비가 다 된것이다.

#### 1-1. 다음 명령어를 입력하기 전, 컨테이너와 공유할 다른 하드디스크 볼륨에 있는 디렉토리를 사용하고자 하는 경우에 다음 절차를 통해 디렉토리를 도커에서 엑세스 허용하게 해야한다.

<br>
<center><img src="/assets/images/200409/002.png" width="500" ></center>
<center><font size="3em">Docker setting에 들어간 뒤</font></center>
<br>


<br>
<center><img src="/assets/images/200409/003.png" width="500" ></center>
<center><font size="3em">Resources > FILE SHARING > 엑세스할 하드디스크 볼륨 체크 > Apply & Restart 클릭</font></center>
<br>

#### 2. CMD 창에서 아래 명령어를 입력하면, docker 위에서 해당 이미지를 띄움과 동시에 명령어로 입력한 Jupyter notebook을 실행하게된다.

```
docker run -v D:/kaggle/tmp/working:/tmp/working -w=/tmp/working -p 8888:8888 --rm -it gcr.io/kaggle-images/python bash -c "pip install jupyter_contrib_nbextensions; pip install jupyter_nbextensions_configurator; jupyter contrib nbextension install --user; jupyter notebook --NotebookApp.token='' --notebook-dir=/tmp/working --ip='*' --port=8888 --no-browser --allow-root"
```

* *참고. docker에 익숙하지 않은 사람을 위해.* ([참고](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter20/28))
- `docker run` : 도커 컨테이너를 실행한다
- `-v [호스트디렉토리]:[컨테이너디렉토리]` : 호스트 디렉토리의 Disk Volume을 컨테이너 디렉토리와 공유한다.
- `-w=[컨테이너디렉토리]` : 도커 컨테이너 안에서 프로세스가 실행될 디렉토리.
- `-p [호스트Port]:[컨테이너Port]` : 호스트와 컨테이너의 포트를 연결(포워딩). 해당 포트로 컨테이너에서 띄운 서버(Jupyter notebook)로 접속가능
- `--rm ` : 프로세스 종료 시 컨테이너 자동 제거
- `-it ` : 상호입출력(i) 및 bash 쉘 사용(t) 할 수 있게 해주는 명령어. 리눅스 쉘(bash)를 사용하겠다는 뜻
- `gcr.io/kaggle-images/python` : 컨테이너로 띄울 이미지(*REPOSITORY* 또는 *Image ID*) 이름. CMD 에서 `docker images`를 입력하면 확인할 수 있다.
- `bash` : bash. 리눅스 쉘(bash)를 사용.
- `-c` : *--cpu-shares=0:* CPU 자원 분배 설정값. 설정의 기본 값은 1024이며 각 값은 상대적으로 적용됨.
- `그 뒤의 명령어~ ` : 실행한 컨테이너에서 입력할 명령어. 여기서는 jupyter notebook 패키지를 설치 및 실행하는 명령어를 가르킨다.

Jupyter notebook이 컨테이너에 설치가 완료되면 서버를 띄웠다는 표시가 뜰것이다. 

#### 3. chrome 등 웹브라우저를 띄운 뒤, `http://localhost:8888` 로 접속해보자. 정상적으로 실행된다면 성공.


## Kaggle API 연결

*아래 내용은 [다음 링크](https://shakeratos.tistory.com/34) 를 참조하였음

#### 1. kaggle api 토큰 생성

- 다음과 같은 링크 `https://www.kaggle.com/<username>/account` (<username>은 자기 아이디로 대체)에 접속
- 페이지 중간에 'Create New API Token' 버튼을 클릭
- 'kaggle.json' 파일을 다운로드 (컨테이너와 공유되는 호스트디렉토리에 옮겨준다.)

#### 2. 실행중인 컨테이너 쉘에 접속한다.

여러 방법이 있을 수 있지만, 다음 방법을 사용해볼 수 있다.

<br>
<center><img src="/assets/images/200409/004.png" width="500" ></center>
<center><font size="3em">실행중인 docker 아이콘을 우클릭하여 대시보드 실행</font></center>
<br>

<br>
<center><img src="/assets/images/200409/005.png" width="500" ></center>
<center><font size="3em">실행중인 컨테이너에 마우스를 올리면 나타나는 아이콘 중, 쉘 모양의 아이콘을 클릭</font></center>
<br>

그러면 컨테이너에서 동작하는 bash 쉘을 제공받을 수 있다.

#### 3. 컨테이너 bash 쉘에 다음 명령어를 입력한다.

```
mkdir ~/.kaggle
mv kaggle.json ~/.kaggle/kaggle.json
chmod 600 ~/.kaggle/kaggle.json
```

차례로 kaggle 접속 디렉토리를 만들고, kaggle.json 토큰을 디렉토리로 전송, 권한부여하는 과정이다.

#### 4. 마지막으로, `kaggle competitions download -c [COMPETETION NAME]` 명령어를 입력하여 데이터를 다운로드한다.

- *COMPETITION NAME* 은 kaggle 홈페이지에서 참여중인 COMPETETION 페이지 > Data 탭 에서 다운로드 관련 API에서 확인 가능하다.

#### 5. COMPETETION용 데이터 zip 파일이 다운로드 된다. `unzip` 명령어를 사용해 압축해제 후 분석을 진행한다.


## git 설치

다음 내용은 [링크](https://wonderbout.tistory.com/64)를 참조하였음

1. git 을 다운로드 받는다. ([여기서 받을 수 있다](https://git-scm.com/))

2. 설치한다. 설치 완료 후 CMD창이나 Git bash 쉘에서 다음 명령어로 설치완료를 확인한다. `git --version`

3. 다음 명령어를 통해 Github 계정을 저장한다.

```
git config --global user.name "사용자" 
git config --global user.email "사용자 이메일"
```

## git 활용

[링크](https://midas123.tistory.com/224)를 참조.

1. 시작할 디렉토리에서 `git init`

2. `git pull` 명령어를 통해 Github repository를 다운로드

3. 업데이트할 파일들을 `git add`로 추가

4. `git push`로 Github에 업로드

-----

사실 git은 세팅만 하고 사용하지 않았다. 

git의 핵심기능인 branch 생성 및 merge 하는 절차가 현재 협업스타일에서 사용하지않기 때문이다.

그냥 각자 작성한 파일 공유 및 회의내용을 Github wiki 기능으로 사용하고 있는데,

파일 업로드도 굳이 git을 거칠 필요가 없어 직접 업로드하려 한다.

<br>
<br>
그러면 오늘은 여기까지 알아보도록 하겠다.


REFERENCE
<br>
[1. Meta-Learning:Learning to Learn Fast](https://lilianweng.github.io/lil-log/2018/11/30/meta-learning.html)
<br>
[2. Meta-Learning:Learning to Learn Fast(Korean)](https://talkingaboutme.tistory.com/entry/DL-Meta-Learning-Learning-to-Learn-Fast)

