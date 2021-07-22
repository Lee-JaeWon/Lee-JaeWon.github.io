---
title : "Lec 01: 기본적인 Machine Learning 의 용어와 개념 설명"
category :
    - Deep_Learning_Study
tag :
    - Deep_Learning
    - Boost Course
toc : true
toc_sticky: true
comments: true
---

Deep_Learning study Lec01 (TensorFlow)

## Boostcourse의 '텐서플로우로 시작하는 딥러닝 기초'를 통한 공부와 정리 Post

## Goal of Study
> - 기본적인 머신러닝(Machine Learning)의 용어와 개념에 대해 알아본다.  

### Keyword
> - 머신러닝(Machine Learning)
> - 지도학습(Supervised Learning) / 비지도학습(Unsupervised Learning)

## 1. 강의 요약
### What is ML(Machine Learning)?
일종의 소프트웨어(프로그램)이다.  

굉장히 좋은 프로그램들이 많지만, 어떠한 경우에서는 너무 많은 규칙을 가지고 있는 경우(자율주행, 움직임 추정, 번역 등)가 있기에 개발자의 입장에서 이 모든 경우를 다루기는 쉽지 않을 수 있다.  

그래서 이를 모두 프로그래밍하는 것이 아닌(개발자가 모든 경우의 수, 예외를 정해주지 않는다) 프로그램 자체가 데이터를 통해 학습하는 능력을 갖는 것을 '머신러닝'이라 한다.  

### What is Learning?
학습하는 방법에 따라 아래의 두 가지 경우로 나뉜다.

- Supervised Learning:    
  
Supervise -> 감독하다.  
말 그대로 정해져 있는(정답이 있는) 데이터를 가지고 학습하는 방법.  
Learning with labled examples - training set

그 예시로는 개와 고양이를 분류한다고 했을 때 사전에 고양이 사진과 강아지 사진을 명시적으로 제공하며, 이를 통해 학습한다. 이 때, 이러한 방식을 Supervised Learning 이라고 한다.  

- Unsupervised Learning:  
=> 비지도학습

레이블을 정해주기 어려운 경우라면 un-labeled data를 통해 모든 데이터를 학습하여 유사한 점들을 군집화 해주거나 관계등을 스스로 학습할 수 있다. 이렇게 비슷한 특징끼리 군집화 하여 새로운 데이터에 대한 결과를 예측하는 방법을 비지도학습 이라고 한다.

### Training Data Set  
  
Lable(Y)을 가지고 학습을 하면 모델이 생성된다. 이를 통해, 모르는 데이터인 X를 분류해주는 것이 일반적인 supervised 머신러닝의 예이다. 이 과정을 Training Set이라고 한다.  
위 개념에 대한 고찰 : 그렇다면 머신러닝을 아직 실제 구현을 통해 접해보지는 않았지만, 아마도 머신러닝이라는 과정은 이 Data Set의 품질이 굉장히 중요할 것이라 예상된다. 

- AlphaGo  
  
알파고의 경우도 바둑 대국의 기보들(Training Data Set)을 모두 학습하여 새로운 경우(새로운 대국)에서 학습한 데이터 모델을 기반으로 대국을 진행한다(Supervised Learning).

### Types of supervised learning  

- Regression  
=> 회귀  

데이터들의 특징을 기준으로, 어떠한 값들의 패턴(시험 점수)나 그 경향, 트렌드 등을 예측할 때 사용된다. 그렇기에 어떤 수나 실수 형태로 예측될 수 있다.

ex) 시간(데이터의 특징, 분류기준)에 따른 점수의 경향(주어진 데이터)을 학습하여 시간을 대입하면 점수를 예측할 수 있다.

- Pass/non-pass  

1, 0 / 패스, 논패스로 분류한다.

- Letter Grade

이 또한, 주어진 등급들에 따라 분류한다.(Multi-lable classification)
















