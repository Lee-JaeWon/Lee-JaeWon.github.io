---
title : "Deep_Learning study Lec03 (TensorFlow)"
category :
    - Deep_Learning_Study
tag :
    - Deep_Learning
    - Boost Course
toc : true
toc_sticky: true
comments: true
---

Lec 03: Linear Regression and How to minimize cost

## Boostcourse의 '텐서플로우로 시작하는 딥러닝 기초'를 통한 공부와 정리 Post

## Goal of Study
> - 선형회귀(Linear Regression)의 비용을 최소화하는 방법을 알아본다.    

### Keyword
> - 선형회귀(Linear Regression)
> - 가설(Hypothesis)
> - 비용함수(Cost function)
> - 경사 하강법(Gradient Descent)
> - 볼록 함수(Convex function)
  
## 1. 강의 요약  
### Hypothesis and Cost
[이전 포스팅](https://lee-jaewon.github.io/deep_learning_study/Lec02/#hypothesislinear)에서 Hypothesis와 Cost에 대해 다루었다.  

 Cost가 작을수록 우리의 Hypothesis가 실제 데이터 더 잘 반영하고 있다고 할 수 있다.
그래서 이 cost를 어떻게 최소화 할 수 있는지 알아보는 것이 목표이다.

### Simplified Hypothesis
계산을 쉽게 하기 위해 수식을 아래와 같이 간략하게 만든다.
<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125598407-db6a7eb4-d1af-4b86-91e7-aacec53e6812.png" width = "400" ></p>

기존의 수식과 비교해보면 가설 함수의 b(bias)를 생략하였고, 이로 인해 Cost의 b도 사라진다.
내용은 동일하다.

### What cost looks like?
Cost 함수가 실제로 어떠한 형태로 이루어져있는지 살펴보기 위해
W값을 변화시켜가며 Cost를 살펴보자.

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125599509-8e3ce456-9c1f-408c-a4a3-f3e75670314e.png" width = "200" ></p>

위의 데이터에서
m = 3
- W = 0 일 때, cost(W) = ?  
    cost(W) = 4.67
- W = 1 일 때, cost(W) = ?  
    cost(W) = 0
- W = 2 일 때, cost(W) = ?  
    cost(W) = 4.67
- W = 3 일 때, cost(W) = ?  
    cost(W) = 18.67

이 값들을 그래프로 표현하면,  
<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125599856-1748cf3f-48e7-4367-8ecb-4d43843a8228.png" width = "300" ></p>

와 같이 나타낼 수 있고, 이를 더 조밀하게 나타내보면
<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125600081-0a730a4c-e6d1-4874-942c-f7ba0d89afa3.png" width = "400" ></p>
다음과 같은 형태가 나타난다.

### How to minimize cost?
[이전 포스팅에서 Cost function에 대한 결론을 설명할 때](https://lee-jaewon.github.io/deep_learning_study/Lec02/#cost-function) 러닝이라는 것의 의미는 Cost가 최저점이 되는 W를 찾아가는 과정이라고 했다.

위의 그래프에서 최저점은 W = 1인 지점이다. 우리는 이를 쉽게 보고 파악할 수 있지만 컴퓨터는 최저점을 찾는 연산을 통해 찾아야 한다.

그 대표적인 방법으로 널리 알려진 것이 Gradient Descent(GD)라는 기법이다.
경사 하강법이라고 부른다.

이 방법은 경사를 따라 내려가며 최저점을 찾도록 설계된 알고리즘이다. 이는 변수가 W, b 뿐만 아니라
이보다 더 많을때도 사용할 수 있는 알고리즘이다.

### How it(GD) works?






 
