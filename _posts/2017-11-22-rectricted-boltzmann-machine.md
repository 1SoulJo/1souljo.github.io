---
layout: post
title: Restricted Boltzmann Machines
date: 2017-11-22 16:00:00
categories: study machinelearning
---
## 제한된 볼츠만 머신

데이터를 재구성(reconstruct) 함을 통해 입력 데이터의 패턴을 찾아내는 모델.

+ Geoff Hinton 교수가 제안
+ Vanishing Gradient 에 대한 해결 방안으로 사용 가능
+ Unsupervised
+ Feature extractor 의 일종이라고 할 수 있다.
  + 데이터에 내재한 패턴을 찾아냄.
+ auto encoder 와 유사함.
+ Shallow 2-layer net 이다.
  + visible layer
  + hidden layer

!["RBM"](/public/img/RBM_1.jpg)


## Recontruction

forward / backward 를 반복적으로 학습시켜 data 의 숨겨진 feature 들을 학습시킨다.  
loss 는 [KL Divergence]{:target="_blank"} 를 사용.

!["RBM"](/public/img/RBM_2.jpg)

[KL Divergence]: https://ko.wikipedia.org/wiki/%EC%BF%A8%EB%B0%B1-%EB%9D%BC%EC%9D%B4%EB%B8%94%EB%9F%AC_%EB%B0%9C%EC%82%B0
