---
title : "PointNet++: Deep Hierarchical Feature Learning on Point Sets in a Metric Space 리뷰"
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
PointNet은 point sets를 직접 처리하는 선구적인 노력이다.<br>PointNet의 기본 아이디어는 각 point의 공간 인코딩을 학습한 다음 모든 개별 point features을 global point cloud signature에 통합하는 것이다.<br>설계상 PointNet은 metric에 의해 유도된 local structure를 캡처하지 않는다.<br><br>
그러나 local structure를 활용하는 것이 convolutional architectures의 성공에 중요한 것으로 입증되었다.<br><br>
metric space에서 샘플링된 points 집합을 계층적 방식(hierarchical fashion)으로 처리하기 위해 PointNet++라는 이름의 계층적 신경망을 소개한다.<br><br>
PointNet++의 일반적인 아이디어는 간단하다.<br>먼저, 점 세트를 거리 메트릭에 따라 겹치는 로컬 영역으로 분할한다.<br>CNN과 유사하게 작은 이웃 환경에서 세밀한 기하 구조를 포착하는 로컬 특징을 추출한다. 이러한 로컬 특징은 더 큰 단위로 그룹화되고 처리되어 더 높은 수준의 특징을 생성한다.<br><br>
이 과정을 전체 포인트 세트의 특징을 얻을 때까지 반복한다.<br><br>

PointNet++의 설계는 두 가지 문제를 해결해야 한다.
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




