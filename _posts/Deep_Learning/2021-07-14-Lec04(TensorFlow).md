---
title : "Deep_Learning study Lec04 (TensorFlow)"
category :
    - Deep_Learning_Study
tag :
    - Deep_Learning
    - Boost Course
toc : true
toc_sticky: true
comments: true
---

Lec 04: Multi-variable Linear Regression

## Boostcourse의 '파이토치, 텐서플로우로 시작하는 딥러닝 기초'를 통한 공부와 정리 Post

'파이토치로 시작하는 딥러닝 기초'와 '텐서플로우로 시작하는 딥러닝 기초'를 함께 공부하며 개념적인 부분에서 상호보완하고 있기 때문에 다른 포스트에서 같은 내용을 다루는 부분이 있을 수 있습니다.

## Goal of Study
> - 다중 선형회귀(Multi-variable Linear Regression)의 개념을 알아본다.   

### Keyword
> - 다중 선형회귀(Multi-variable Linear Regression)  
> - 가설(Hypothesis)  
> - 비용함수(Cost function)  
> - 경사 하강법(Gradient Descent)  
> - 행렬(Matrix)  
  
## 1. 강의 요약  
### Hypothesis in Multiple Variables  
변수가 여러 개일 때의, Hypothesis를 살펴보자.  

조금 더 다양한 변수를 가지고 예측하는 것이 훨씬 더 예측을 잘할 수 있을 것이다.  
이것을 Prediction Power, 예측력이라고 한다.  

여러 개의 변수를 사용하는 경우를 Multi-variable, Multi-feature라고도 얘기한다.  

이 때, Hypothesis가 어떠한 형태인지 살펴보자.  
<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125640729-bba3c5e9-b847-4e16-9f65-75b2aaff83e1.png" width = "400" ></p>  
변수가 여러 개면 늘어난 변수만큼 기술 하게 된다.  
그리고 변수가 늘어난 개수만큼 가중치를 필요로 하게 된다.  

즉, 변수가 3개면 가중치도 3개가 된다.  

### Cost function
Cost function은 동일하다.  
<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125644042-3ecd8288-c0ab-404d-8662-9b5782c88419.png" width = "400" ></p>  

### Matrix
변수가 늘어남에 따라 위 식처럼 Hypothesis를 일일이 써주는 것이 번거로운 작업이 될 수 있다.  

이 때, Matrix를 활용하면 이것을 간단하게 풀어낼 수 있다.  
(이를 위해, Matrix의 곱셈 연산을 알아야한다.)  

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125644785-d90ef8fc-6a24-48a3-b982-eb4aac40365c.png" width = "300" ></p>  
행렬 곱 연산의 dot product의 성질은 다음과 같으며, Hypothesis를 간단하게 표현할 수 있다. 

또한, 공학수학이나 대학수학에서 배우는 행렬 곱의 성질을 다시 생각해보면,  
곱을 할적에 앞에 있는 행렬의 열의 수와 뒤에 있는 행렬의 행의 수가 일치해야 한다는 것을 기억할 수 있을 것이다.  
(5by3 * 3by1 = 5by1)  

x항들의 Matrix를 X, w(가중치)항들의 Matrix를 W라고 하여, H(X)를 간단하게 표현할 수 있다.(대문자로 표현)
<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125645147-fef2a51e-f450-4c4f-950a-245404f2108b.png" width = "300" ></p>  
여기서는 X를 앞에, W를 뒤에 쓰고 있다.  
(행과 열을 계산하기 때문에 X matrix가 앞에 오게 된다.)  

또한, Matrix의 장점으로는 데이터의 개수(Instance)가 얼마든지 동일하게 표현할 수 있다.(데이터 개수와 무관)  
<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125647689-469cd724-11d6-4993-93d5-7dd2bb0acb61.png" width = "400" ></p>  
위에서 Instance는 5개이다.  

어찌되었든 Hypothesis는 위와 같은 형태이지만  
우리는 H(X)로 간단하게 표현할 수 있다는 것을 알고 있다.  

### 출력이 여러 개인 경우
출력이 여러개인 경우는 Weight의 크기를 어떻게 정할까?  
<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125649064-c643a7bb-8aec-4906-9b63-120c23035064.png" width = "400" ></p>   

결국, Weight의 크기는 데이터의 개수와 상관없이 X의 열(변수의 개수)과 출력의 열(column)에 의해 결정된다.  

### WX versus XW
일반적인 이론 설명에서는 W가 계수이기에 WX로 표현을 하지만  

실제 코드에서는 X를 앞에 쓴다.  
(이 이유는 위에서 이야기한 것처럼 행렬 연산을 하기 위함)

## 2. 실습
그렇다면 Hypothesis 연산을 Pytorch에서 어떻게 할 수 있을까?

### 1. Data
많은 양의 데이터를 다루기 위해 Pytorch에서 제공하는 matmul()을 이용한다.  

```python
x_train = torch.FloatTensor([[73, 80, 75],
                             [93, 88, 93],
                             [89, 91, 90],
                             [96, 98, 100],
                             [73, 66, 70]])
y_train = torch.FloatTensor([[152], [185], [180], [196], [142]])
```  
다음과 같은 데이터에서 행렬 연산을 위해

```python
hypothesis = x_train.matmul(W) + b
```  
과 같이 matmul()을 사용해 한번에 계산할 수 있다. (간결, 속도향상, 코드 수정x)  

### 2. Cost function
Cost function은 언급했던 것처럼 동일하게 구한다.([MSE](https://lee-jaewon.github.io/deep_learning_study/Lec02/#3-compute-loss))  

### 3. 학습
학습 방식 또한 동일하다.([Gradient Descent](https://lee-jaewon.github.io/deep_learning_study/Lec03(TensorFlow)/#formal-definition))  

### 4. Full code
```python
import torch
from torch import optim
# Data
x_train = torch.FloatTensor([[73, 80, 75],
                             [93, 88, 93],
                             [89, 91, 90],
                             [96, 98, 100],
                             [73, 66, 70]])
y_train = torch.FloatTensor([[152], [185], [180], [196], [142]])
#Init
W = torch.zeros((3, 1), requires_grad=True)
b = torch.zeros(1, requires_grad=True)
# optimizer
optimizer = optim.SGD([W, b], lr=1e-5)

nb_epochs = 20
for epoch in range(nb_epochs + 1):

    # H(x) 계산
    hypothesis = x_train.matmul(W) + b # or .mm or @
    # cost 계산
    cost = torch.mean((hypothesis - y_train) ** 2)
    # cost로 H(x) 개선
    optimizer.zero_grad()
    cost.backward()
    optimizer.step()
    print('Epoch {:4d}/{} hypothesis: {} Cost: {:.6f}'.format(
    epoch, nb_epochs, hypothesis.squeeze().detach(),
    cost.item()
    ))

```
즉, 데이터 세팅 이외에는 변한 부분이 없는 것을 알 수 있다.  

위 코드의 결과는
```
Epoch    0/20 hypothesis: tensor([0., 0., 0., 0., 0.]) Cost: 29661.800781
Epoch    1/20 hypothesis: tensor([67.2578, 80.8397, 79.6523, 86.7394, 61.6605]) Cost: 9298.520508
Epoch    2/20 hypothesis: tensor([104.9128, 126.0990, 124.2466, 135.3015,  96.1821]) Cost: 2915.712402
Epoch    3/20 hypothesis: tensor([125.9942, 151.4381, 149.2133, 162.4896, 115.5097]) Cost: 915.040649
Epoch    4/20 hypothesis: tensor([137.7967, 165.6247, 163.1911, 177.7112, 126.3307]) Cost: 287.936157
Epoch    5/20 hypothesis: tensor([144.4044, 173.5674, 171.0168, 186.2332, 132.3891]) Cost: 91.371010
Epoch    6/20 hypothesis: tensor([148.1035, 178.0143, 175.3980, 191.0042, 135.7812]) Cost: 29.758249
Epoch    7/20 hypothesis: tensor([150.1744, 180.5042, 177.8509, 193.6753, 137.6805]) Cost: 10.445281
Epoch    8/20 hypothesis: tensor([151.3336, 181.8983, 179.2240, 195.1707, 138.7440]) Cost: 4.391237
Epoch    9/20 hypothesis: tensor([151.9824, 182.6789, 179.9928, 196.0079, 139.3396]) Cost: 2.493121
Epoch   10/20 hypothesis: tensor([152.3454, 183.1161, 180.4231, 196.4765, 139.6732]) Cost: 1.897688
Epoch   11/20 hypothesis: tensor([152.5485, 183.3609, 180.6640, 196.7389, 139.8602]) Cost: 1.710555
Epoch   12/20 hypothesis: tensor([152.6620, 183.4982, 180.7988, 196.8857, 139.9651]) Cost: 1.651412
Epoch   13/20 hypothesis: tensor([152.7253, 183.5752, 180.8742, 196.9678, 140.0240]) Cost: 1.632369
Epoch   14/20 hypothesis: tensor([152.7606, 183.6184, 180.9164, 197.0138, 140.0571]) Cost: 1.625924
Epoch   15/20 hypothesis: tensor([152.7802, 183.6427, 180.9399, 197.0395, 140.0759]) Cost: 1.623420
Epoch   16/20 hypothesis: tensor([152.7909, 183.6565, 180.9530, 197.0538, 140.0865]) Cost: 1.622141
Epoch   17/20 hypothesis: tensor([152.7968, 183.6643, 180.9603, 197.0618, 140.0927]) Cost: 1.621262
Epoch   18/20 hypothesis: tensor([152.7999, 183.6688, 180.9644, 197.0661, 140.0963]) Cost: 1.620501
Epoch   19/20 hypothesis: tensor([152.8014, 183.6715, 180.9665, 197.0686, 140.0985]) Cost: 1.619764
Epoch   20/20 hypothesis: tensor([152.8020, 183.6731, 180.9677, 197.0699, 140.0999]) Cost: 1.619046
```
다음과 같으며, Cost가 적절히 최소화 되는 것을 확인할 수 있다.










