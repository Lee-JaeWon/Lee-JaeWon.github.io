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

이에 더해, 왜 경사하강법을 사용하는지에 대해 다뤄보자면,
그냥 미분 계수가 0인 지점을 찾을 수도 있지만, 함수가 닫힌 형태가 아닌 경우, 미분 계수를 구하기 어려운 함수의 형태일 경우  

GD를 구현하는게 미분 계수를 구하는 것보다 더 쉬운 경우를 대표적으로 예를 들 수 있다.

혹은 데이터 양이 많은 경우 효율적으로 계산하기 위함도 있다.

### How it(GD) works?
1. 먼저 최초의 추정을 통해 W와 b의 값을 정해준다. 이 때, 이 값이 0, 0이든, 랜덤 값이든 상관없다.

2. 그 다음, W와 b값을 Cost가 줄어들 수 있도록 W와 b값을 지속적으로 바꿔준다.

3. W, b값을 지속적으로 업데이트할 때, Cost를 줄일 수 있는 기울기를 구해 업데이트를 해나간다.

위의 과정을, 최소점에 도달했다고 판단할 때까지 반복한다.
이것이 Gradient Descent가 동작하는 방식이다.

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125607581-4118db0a-9589-4090-a40b-cb10bb4307b2.png" width = "400" ></p>
그림으로 살펴보자.

파란색선은 Cost function이다. 이 곡선 상에 임의의 한 지점을 지정한다.

지정한 점에서의 기울기(Gradient)를 구해주고, 그 지점에서의 learning rate 값에 gradient를 곱하고 이를 W 값에서 빼준다.
(Gradient는 미분을 통해 구한다. )

이 때, 그 다음 스텝의 Weight 지점이 정해진다.  
이 지점에서 다시 동일한 작업을 반복하여 한 스텝씩 반복하다 보면 언젠가 기울기가 굉장히 작은 지점에 도달할 것이다.  

기울기가 0에 가까워지면 learning rate 값을 곱하고 W에서 빼주는 과정을 반복해도
이 지점에서는 더 이상 최소화가 되지 않는다. 이 지점이 바로 비용이 최소화가 되는 지점이다.

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125610394-173b75bb-5d9c-498c-84b4-13ee2fd1154a.png" width = "400" ></p>

이 W값은 어느 점에서 시작을 해도 동일한 결과를 나타낸다.

### Formal definition
<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125608935-53058982-6502-4dda-8793-7949907e026e.png" width = "400" ></p>

살펴보았던 cost function에서 1/m , 1/2m, ... 이를 얼마나 나누는지는 Cost의 특성에는 영향을 주지 않는다.

그래서 미분을 위해 m 대신에 2m을 사용할 것이다. (미분 과정에서 지수가 내려올 때 간략하게 하기 위해)

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125609354-af4b054a-852c-4655-a08b-5549ab6ddc10.png" width = "400" ></p>
이 수식이 Gradient Descent 알고리즘이다. 알파는 Learning rate이다. 위에서 cost값을 최소화하기 위해 step을 진행했던 과정과 같다는 것을 쉽게 파악할 수 있을 것이다.

Cost function을 W에 대해 미분(편미분)해주고 Learning rate를 곱해준 후 기울기가 증가하는 방향의 반대로 step이 진행(update)할 수 있도록, W에서 이를 빼준다.  

최종 요약하자면, Cost function을 1/m에서 1/2m으로 바꿔주고, 이를 미분한 값에 Learning Rate를 곱한 후 W에서 이를 빼준 값을 W에 assign 하는 과정을 반복하며 Cost가 최소화 하도록 한다.

### Convex function
Local minimum과 Global minimum이 일치하지 않는 복잡한 함수의 경우일 때, 가장 낮은 지점으로 도달할 수 있을 것이라는 보장을 할 수 없다.  

이러한 경우에서는 Gradient Descent를 사용할 수 없다.

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125616206-75d9efde-9233-45df-8fc4-77aadd635890.png" width = "400" ></p>
그래서 Local minimum과 Global minimum이 일치하는 Convex function에서 사용하게 된다.  
Cost function이 Convex function(볼록 함수)이라면 항상 최저점에 도착한다는 것을 보장할 수 있기 때문에 Gradient Descent 알고리즘을 적절히 사용할 수 있다.











 
