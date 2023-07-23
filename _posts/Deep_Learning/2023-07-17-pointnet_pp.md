---
title : "[논문 리뷰] PointNet++: Deep Hierarchical Feature Learning on Point Sets in a Metric Space"
category :
    - Deep_Learning_Study
tag :
    - Deep_Learning
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---
PointNet++: Deep Hierarchical Feature Learning on Point Sets in a Metric Space 리뷰

# Abstract
[PointNet](https://lee-jaewon.github.io/deep_learning_study/pointnet/)은 point set에 대한 딥러닝이 없던 이전 연구들에 비해 pioneer인 작업이었다.<br><br>
하지만 PointNet은 local structure를 유추하기 않기 때문에, 세밀하거나 복잡한 장면에 대한 일반화 능력이 제한된다.<br>
(PointNet은 한번의 global max pooling만을 활용하다 보니 local structure를 유추하기가 힘들다.)
<br><br>
본 논문에서는 input point set의 중첩 분할에 재귀적으로 PointNet을 적용하는 계층적 신경망(hierarchical neural network)을 제시한다.<br><br>

# Introduction
PointNet과 마찬가지로 Point cloud는 permutation invariance해야함을 설명한다.<br><br>
Distance metric은 다른 속성을 가질 수 있는 local neighborhhod를 정의한다.<br>
(거리 메트릭(distance metric)은 공간에서 두 점 사이의 거리를 정의하는 방법)
<br><br>
예를 들어, points의 밀도 및 기타 속성은 서로 다른 위치에 걸쳐 균일하지 않을 수 있다.<br>
PointNet은 point sets를 직접 처리하는 선구적인 노력이다.<br><br>PointNet의 기본 아이디어는 각 point의 공간 인코딩을 학습한 다음 모든 개별 point features을 global point cloud signature에 통합하는 것이다.<br><br>설계상 PointNet은 metric에 의해 유도된 local structure를 캡처하지 않는다.<br><br>
그러나 local structure를 활용하는 것이 convolutional architectures의 성공에 중요한 것으로 입증되었다.<br><br>
metric space에서 샘플링된 points 집합을 계층적 방식(hierarchical fashion)으로 처리하기 위해 PointNet++라는 이름의 계층적 신경망을 소개한다.<br><br>
PointNet++의 일반적인 아이디어는 간단하다.<br>먼저, 점 세트를 거리 메트릭에 따라 겹치는 로컬 영역으로 분할한다.<br>CNN과 유사하게 작은 이웃 환경에서 세밀한 기하 구조를 포착하는 로컬 특징을 추출한다.<br>이러한 로컬 특징은 더 큰 단위로 그룹화되고 처리되어 더 높은 수준의 특징을 생성한다.<br><br>
이 과정을 전체 포인트 세트의 특징을 얻을 때까지 반복한다.<br><br>

PointNet++의 설계는 **두 가지 문제를 해결**해야 한다.
- point set의 partitioning 생성 방법
- local feature learner를 통해 points 집합 또는 local features을 추상화하는 방법.

<br><br>
이 두 가지 문제는 관련성이 있다.<br>point set의 partitioning은 파티션 간에 공통 구조를 생성해야 하므로, local feature learner의 가중치를 공유할 수 있다.<br><br>

local feature learner로 PointNet을 선택.<br>
PointNet연구에서 입증되었듯이, PointNet은 의미론적 특징 추출(semantic feature extraction)을 위해 unordered set of points을 처리하는 효과적인 아키텍처이기 때문이다.<br><br>
PointNet++는 입력 세트의 중첩된 분할(nested partitioning)에 반복적으로 PointNet을 적용합니다.<br><br>

**Main Contribution**
- PointNet의 local feature를 추출하기 힘든 문제를 해결
- Unordered point set을 처리하는 아키텍쳐인 PointNet을 계층적으로 활용하여 이를 해결.

<br><br>

# Problem Statement
$X=(M,d)$는 Euclidean 공간 $\mathbb{R}^n$으로부터 metric을 상속받는 이산 metric space이다.<br><br>
여기서 $M \subseteq \mathbb{R}^n$은 점의 집합이고, d는 distance metric이다.<br><br>
또한, 주변 Eulidean space에서 $M$의 밀도는 모든 곳에서 일정하지 않을 수 있다.<br><br>
본 논문은 이러한 $X$를 입력(각 포인트에 대한 추가적인 feautre도 함께)으로 사용하여 의미 있는 정보를 생성하는 집합 함수 $f$를 학습하는 것에 관심있다.<br><br>
실제로, 이러한 $f$는 $X$에 label을 할당하는 classification 함수이거나 $M$의 각 구성원에 대해 개별 point label을 할당하는 segmentation 함수가 될 수 있다.<br>
- (metric space : 수학에서 거리 공간은 두 점 사이의 거리가 정의된 공간이다. 거리의 정의에 따라 표준적인 위상을 갖는다.)
- (ambient Euclidean space : 수학에서, 환경 공간(영어: ambient space)은 공간을 둘러싸는 공간이다.)<br><br>

# Method
hierarchical structure가 추가된 PointNet의 확장이다.<br><br>
3.1에서 PointNet review<br><br>
3.2에서 hierarchical structure를 가진 PointNet의 기본 확장을 소개<br><br>
3.3에서 불균일하게 샘플링된 point sets에서도 기능을 강력하게 학습할 수 있는 PointNet++를 제안<br><br>

## 3.1 Review of PointNet
Given an **unordered** point set $\{ x_1,x_2,...,x_n \}$ with $x_i \in \mathbb{R}^d$.<br><br>
$f:X \rightarrow R$ <br><br>
$f(x_1,x_2,...,x_n)=\gamma(\underset{i=1,...,n}{MAX}(h(x_i)))$ <br>
<br><br>
$where \ \ \gamma \ and \ \ h$ -> MLP
<br><br>
PointNet은 몇 가지 benchmarks에서 인상적인 성능을 달성.<br><br>
그러나 다른 scales로 local context를 캡처할 수 있는 기능이 부족하다.<br><br>
한계를 해결하기 위해 다음 절에 hierarchical feature learning framework를 도입.<br><br>

## 3.2 Hierarchical Point Set Feature Learning
<p align="center"><img src="/MyPDF/PNPP(1).png" width = "700" ></p>
<p align="center">Figure 2</p>

PointNet은 single max pooling operation을 사용하여 전체 point set를 집계하지만,<br>
새로운 아키텍처는 포인트의 hierarchical grouping을 구축하고 hierarchy을 따라 점점 더 큰 local regions을 추상화한다.<br><br>
hierarchical structure는 많은 집합 추상화 수준으로 구성된다(Figure 2).<br><br>
각 level에서 points 집합이 처리되고 추상화되어 더 적은 수의 요소로 새 집합을 생성한다.<br><br>
The **set abstraction** level is made of three key layers:<br>
- **Sampling layer** : input points에서 points 집합을 선택하여 **local regions의 centroid를 정의**
- **Grouping layer** : 이후, 다음 중심 주위에 “neighboring” points을 찾아 **local region sets을 구성**
- **PointNet layer** : local region patterns을 **feature vectors로 encode**하기 위해 **mini-PointNet을 사용**

<br>

- **Sampling layer**<br>
입력 포인트 $\{x_1, x_2,..., x_n\}$이 주어진 경우, interative FPS(farthest point sampling)을 사용하여 $\{x_{i1}, x_{i2},..., x_{im}\}$의 하위 집합을 선택.<br>(이때 FPS는 계속 멀리있는 점을 위주로 sampling 하기 위해 돌아다님.)<br><br>
즉, $x_{ij}$는 집합 $\{x_{i1}, x_{i2}, ..., x_{ij-1}\}$에서 나머지 포인트에 대해 가장 멀리 떨어져 있는 포인트(미터 단위)<br><br>
무작위 표본 추출에 비해 동일한 수의 중심 포인트가 주어졌을 때 전체 포인트 세트에 대한 적용 범위가 더 좋다.
<p align="center"><img src="/MyPDF/PNPP(2).png" width = "500" ></p>

- **Grouping layer**<br>
d-dim coordinates, C-dim point feature, N points<br>
(d=3, N은 pointcloud점의 개수,3의 3차원, C은 학습을 통해서 알아내야 할 point의 feature)
<br><br>
이 레이어에 대한 입력은 크기 $N \times (d + C)$의 포인트 집합과 크기 $N' \times d$의 중심 집합의 좌표<br><br>
출력은 $N' \times K \times (d + C)$ 크기의 포인트 집합 그룹.<br><br>
여기서 각 그룹은 로컬 영역에 해당하며 K는 중심 포인트 주변의 포인트 수.<br><br>
Sampling과 Grouping을 거치면 $N_1 \times K \times (3 + C)$로 K개의 그룹으로 표현할 수 있고 PointNet을 통해 학습을 한 결과는 $N_1 \times (3 + C)$로 K개의 그룹이 각각의 local feature를 학습한 결과로 나타나게 된다.<br><br>
이 과정을 반복하면 마치 CNN이 hierarchy하게 feature를 추출하는 방법과 유사하게 low-level부터 high-level까지 다양한 feature를 뽑는다.<br>
([https://jaehoon-daddy.tistory.com/47](https://jaehoon-daddy.tistory.com/47))
<br><br>
Grouping을 위해서는 **Ball query**가 사용됨. 이는 특정 반경 내에 있는 점들을 찾는 과정을 말한다.
<br><br>
Ball Query는 쿼리 포인트로부터 **특정 반경 내에 있는 모든 포인트들을 찾는 과정을 수행**.구현에서는 K의 상한을 설정하여 반경 내에 속하는 포인트들을 찾는다.<br><br>
다른 대안으로는 K 최근접 이웃(K nearest neighbor, kNN) 검색이 있다.<br>kNN은 고정된 수의 이웃 포인트를 찾는 방식이며, kNN과 비교했을 때, Ball Query는 로컬 이웃을 고정된 영역 규모로 보장하여 로컬 영역 특징을 공간적으로 일반화하는 데 더 유용하다.<br><br>
따라서, Ball Query는 PointNet++의 Grouping layer에서 사용되며, 로컬 영역을 정의하고 해당 영역 내의 포인트들을 선택하는 역할을 수행.<br><br>
이를 통해 PointNet++은 입력 포인트 클라우드를 로컬 영역으로 구분하고, 각 영역에서 세밀한 기하 구조를 포착할 수 있다.<br><br>
<p align="center"><img src="/MyPDF/PNPP(3).png" width = "500" ></p>

- **PointNet layer**<br>
이 레이어에서 입력은 데이터 크기가 $N' \times K \times (d+C)$인 포인트의 $N'$ local region<br><br>
출력에서 각 local region은 중심과 중심 이웃을 인코딩한 로컬 특징에 의해 추상화한다.<br><br>
출력 데이터 크기는 $N' \times (d + C')$이다.<br><br>
(여기서 local frame 변환을 수행한다. 이유는 순서 없는 데이터에서는 인접한 점들 사이의 상대적인 위치와 거리 정보를 쉽게 파악하기 어렵다. 따라서 각 점의 좌표를 중심점을 기준으로 상대적인 좌표로 변환하여 로컬 프레임을 만들어주는 것이다.)<br><br>
지역 영역 내의 각 점 $i$와 각 공간적 차원 $j$ (예: x, y, z)에 대해, $x^{(j)}_i$는 $x^{(j)}_i = x^{(j)}_i - \hat{x}^{(j)}$ 를 통해 local frame으로 변환된다.<br><br>
PointNet 레이어는 개별 점을 처리하기 위해 활용된다.<br><br>
PointNet은 개별 점을 처리하기 위해 설계된 신경망 아키텍처로, **상대적 좌표(변환된 점)**와 점과 관련된 **추가적인 특징**을 **입력**으로 받는다.<br><br>상대적 좌표와 점의 특징을 결합함으로써, PointNet은 지역 영역 내의 **점 간 관계를 포착**하고 **의미 있는 특징을 학습**할 수 있다.<br><br>

## 3.3 Robust Feature Learning under Non-Uniform Sampling Density
앞에서 논의한 바와 같이, 포인트 집합은 다른 영역에서 **균일하지 않은 밀도를 갖는 것**이 일반적이다.<br><br>
이러한 **불균일성(non-uniformity)은 포인트 세트 특징 학습에 중요한 문제를 야기**한다.<br><br>
고밀도 데이터에서 학습된 특징은 희박하게 샘플링된 영역에 대해 일반화되지 않을 수 있다.<br><br>
따라서 희소 포인트 클라우드에 대해 훈련된 모델은 세분화된 로컬 구조를 인식하지 못할 수 있다.<br><br>
<br>

이상적으로, **조밀하게 샘플링된 영역에서 가장 미세한 디테일(finest detail)을 포착**하기 위해 가능한 한 포인트 세트를 면밀히 조사하기를 원한다.<br><br>
그러나 저밀도 지역에서는 sampling 부족으로 인해 local pattern이 손상될 수 있어, 다음과 같은 밀접한 검사(close inspect)는 금지된다.<br><br>
이 경우, 우리는 더 넓은 범위에서 큰 규모의 패턴을 찾아야 한다.<br><br>
이 목표를 달성하기 위해 밀도에 적응하는(density adaptive) PointNet 레이어(Figure 3)를 제안하였다.<br>
<p align="center"><img src="/MyPDF/PNPP(4).png" width = "500" ></p>
<p align="center">Figure 3</p>

이 레이어는 입력 샘플링 밀도가 **변경될 때** 서로 **다른** 규모의 영역에서 **특징을 결합**하는 방법을 학습한다.<br>
이러한 방법을 **density adaptive layer**라 한다.<br><br>
이러한 밀도 적응형 PointNet 레이어를 포함한 계층적 네트워크를 **PointNet++**이라고 부른다.<br><br>
density adaptive layer방법으로 두 가지 방법을 소개한다.<br><br>

- Multi-scale grouping(MSG)<br>
이 아이디어는 grouping layer를 서로 다른 scale로 적용하는 것.<br><br>
각 scale에 해당하는 PointNet을 사용하여 특징을 추출하고, 각 scale에서 추출된 특징들은 Multi-scale feature로 concatenate된다.<br><br>
네트워크를 학습시켜 다중 스케일 특징을 최적으로 결합하는 전략을 학습한다.<br><br>
이는 각 인스턴스마다 무작위 확률로 입력 점들을 제외시키는 방식인 랜덤 입력 드롭아웃(random input dropout)을 사용하여 수행된다.<br><br>

- Multi-resolution grouping(MRG)<br>
MRG는 논문에서 채택한 방식<br><br>
위의 MSG 접근법은 모든 중심 포인트에 대해 대규모 이웃에서 로컬 PointNet을 실행하기 때문에 계산 비용이 많이 든다.<br><br>
특히 보통 lowest level에서 중심 포인트 수가 상당히 많기 때문에 time cost가 상당하다.<br><br>
Pointnet++에서 set abstraction(sampling+grouping)과정을 5번 진행한다고 가정하였을때 1번째 진행하여 얻을 feature vector부터 5번째 진행하여 얻은 feature vector까지 concat하여 모든 level의 정보를 활용(해당 부분은 정확히 이해 안되서 참고자료에서 따왔습니다.)<br>
([https://jaehoon-daddy.tistory.com/47](https://jaehoon-daddy.tistory.com/47))
<br><br>
Figure 3의 (b)를 살펴보면 왼쪽 벡터는 low level $L_{i-1}$의 각 low level의 특징을 요약하는 과정을 통해 얻어진다.<br><br>
오른쪽에 있는 벡터는 단일 PointNet을 사용하여 로컬 지역 내의 모든 원시 점을 직접 처리하여 얻은 특징이다.<br><br>
이를 concat한다.<br><br>
이는 로컬 영역의 밀도가 높을 때와 낮을 때 신뢰도를 달리 갖는다.<br><br>
**로컬 영역의 밀도가 낮을 때**, 첫 번째 벡터는 두 번째 벡터보다 신뢰도가 낮을 수 있는데, 첫 번째 벡터는 첫 번째 벡터(왼쪽)를 계산하는 부분 영역이 **더 희박한 포인트를 포함**하고 표본 추출 결핍으로 더 고통(suffer) 받기 때문이다.<br><br>
이러한 경우, 두 번째 벡터는 더 높은 가중치를 부여해야 한다.<br><br>
<br>
반면에, **로컬 영역의 밀도가 높을 때**, 첫 번째 벡터는 더 높은 해상도에서 낮은 수준에서 재귀적으로 검사할 수 있는 능력을 가지고 있기 때문에 **더 미세한 세부 정보를 제공**한다.<br><br>
MSG와 비교하여, 이 방법은 가장 낮은 수준에서 대규모 이웃에서 특징 추출을 피하기 때문에 계산적으로 더 효율적이다.<br><br>
(이해 : MSG는 가장 low level에서 다양한 scale로 grouping을 시도하기에 low level에서의 cost가 증가)
<br><br>

## 3.4 Point Feature Propagation for Set Segmentation
set abstraction layer에서는 original point set이 subsampled(다운 샘플링)된다.<br><br>
그러나 Semantic point labeling과 같은 set segmentation 작업에서는 모든 원본 점에 대한 점 특징을 얻고자 한다.<br><br>
하나의 해결책은 항상 모든 점을 중심점으로 샘플링하는 것이다. 그러나 이는 계산 비용이 매우 높아진다. <br><br>
또 다른 방법은 다운 샘플링된 점으로부터 원본 점으로까지 특징을 전파(propagate)하는 것이다.<br><br>
distance based interpolation과 across level skip link를 사용한 계층적 전파 전략을 채택하였다.<br><br>
interpolate된 피쳐는 설정된 set abstraction level의 feature와 concat된다.<br><br>
보간을 위한 많은 선택 중에서 k nearest neighbor을 기반으로 하는 역거리 가중 평균을 사용한다.<br><br>
이 다음 concatenated feature는 unit PointNet을 통과한다.<br><br>
요약 : interpolation을 통해 feature propagation을 진행하고 보간된 피쳐는 이전의 set abstraction level의 feature와 concat. 이후 unit PointNet을 통과.<br><br>
<p align="center"><img src="/MyPDF/PNPP(5).png" width = "500" ></p>

# Experiments
## Point Set Classification in Euclidean Metric Space
<p align="center"><img src="/MyPDF/PNPP(6).png" width = "500" ></p>

## Point Set Segmentation for Semantic Scene Labeling
<p align="center"><img src="/MyPDF/PNPP(7).png" width = "500" ></p>
<p align="center"><img src="/MyPDF/PNPP(8).png" width = "500" ></p>

## Point Set Classification in Non-Euclidean Metric Space
<p align="center"><img src="/MyPDF/PNPP(9).png" width = "500" ></p>
