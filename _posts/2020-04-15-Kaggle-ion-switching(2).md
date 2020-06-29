---
title: "Kaggle-ion-switching(2)"
categories: 
  - Kaggle
last_modified_at: 2020-04-15 12:00:00
toc: false #Table of Contents
comments: true
use_math: true # MathJax On
---

Ion Switching Competition: Signal EDA
- 배경지식 설명
	- Electrophysiology(전기생리학)적 세포전기신호 측정방식
	- Ion Channel 열림/닫힘과 그 사용처

- EDA : Signal Denoising
		- 변동이 심한 Signal 데이터에서 보다 명확한 특성(Trend)을 추출하기 위해 Denoise작업이 필요.
		- 원본 데이터의 일부 정보를 잃어버릴 수도 있지만, 시계열분석에서 Trend를 추출하는데엔 유용한 작업.
	- Wavelet denoising
		- "Wavelet coefficients"로 불리는 계수를 산출
		- 이 계수는 어떤 정보에 대해, 남길 정보(Signal)와 버릴 정보(Noise)를 구분하는데 사용
		- MAD(mean absolute deviation) 값을 사용하여, threshold를 산출
		- signal -> wavelet decompose -> MAD값 Treshhold 적용(Keep or Discard) -> wavelet reconstruct
	-Average Smoothing
		- Window 사이즈와 Stride를 조절하여 평균값 적용
		- 그러나 효과가 Wavelet denoising 에 비해 좋지못함

- EDA: Open_Channels 
	- 전체 Open_Channels
		- Open Channels 개수 분포를 보면, 여러 채널이 열려있는 경우보다 안열린 경우가 훨씬 빈번
		- 이는 채널이 닫혀있는 상태가 Stable한 상태일 수 있음 (닫혀있을 확률에 가중치를 둔 예측모델이 있을까?)
	
	- Open_channels 배치(50초, 50만Row)
		- Open Channels 개수 분포가 배치별로 다양한 모습

	- Signal vs Open_Channels
		- Open_Channels 개수에 따른 Signal값의 평균치 추세를 확인
		- Open_Channels가 1 또는 2인 경우를 제외하면, 대체로 높은 Signal에서 많은 Open_Channels 임을 확인

- 복잡도 계산(entropy and fractal dimension)
	- signal의 "Roughness"와 "Complexity"를 측정하기 위한 방법

	- Permutation Entropy
		- 시계열분석에 대한 복잡도 계산법
		- 연속된 값에 대한 비교를 통해 정보를 도출
		- 1000개의 연속된 Row의 Signal 데이터의 Entropy를 도출->1000번째 있는 Open_Channels 값과 비교
		- Open_channels가 많을수록, signal의 복잡도나 변동폭이 줄어드는 트렌드가 있음
		
	
	-Approximate Entropy
		- 시계열분석에 대해 측정하는 방식
		- 낮은 값이 나올수록 정규화되고 예측가능함
		- 측정결과, Open_Channels에 따른 특이점이 도출되지않음

	
	- Higuchi fractal dimension
		- 2D상에 나타난 곡선을 통해 값을 측정
		- 높은 값이 나타날수록 복잡해짐을, 낮은 값일 수록 단순해짐을 나타냄
		- Open_Channel이 0일 때 가장 높은 값을 보여 복잡하다는 정보
		- 나머지에선 비슷한 값을 나타내고있지만, 대체로 Open_channels가 많아질수록 덜 복잡해지는 트렌드

	- Katz fractal dimension
		- Higuchi 와 비슷한 결론

> 요약
 - 배경지식 설명(Signal 측정방식, Ion Channel 예측결과 활용처)
 - Denosing 기법(Wavelet Denoising, AverageSmoothing보다 우수)
 - Open_Channels 트렌드(Signal과 상관성 짐작)
 - 복잡도 계산(대체로 많은 Channel이 열려있는 경우에 Signal의 복잡도가 작아지는 추세를 보임 -> 열린 채널이 없는 경우, 오히려 Signal의 변동폭이 크다)

 - 활용
	- Wavelet Denoising 방식을 Baseline 필터링 기법으로 활용
	- 복잡도 계산결과를 필터링에 가중치처럼 활용하는 기법 탐색(덜 복잡한 데이터는 덜 필터링 하는 방식?)
