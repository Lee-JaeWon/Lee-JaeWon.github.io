---
title : "[논문 리뷰] Swarm-SLAM: Sparse Decentralized Collaborative Simultaneous Localization and Mapping Framework for Multi-Robot Systems"
category :
    - Multi_Robot
tag :
    - Multi_Robot
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---
[2023,arxiv] Swarm-SLAM: Sparse Decentralized Collaborative Simultaneous Localization and Mapping Framework for Multi-Robot Systems 논문 리뷰

# Introduction
Collaborative perception은 robotics에서 중요한 문제이다.

환경에 대한 **shared understanding**은 다양한 작업에서 이점이 되는 정보를 지닌다.

One of the most powerful robot perception은 SLAM이다.

단일 로봇은 개별 로봇에서 개별 reference frame을 가지고 SLAM을 수행하면 되지만,<br>

GPS-denied 환경에서의 다중 로봇의 작동은 local map을 연결하거나 병합하지 않는한 상황 인식을 공유하지 못한다.

이 문제를 해결하기 위해 Collaborative SLAM(C-SLAM)은 **inter-robot** map link를 찾아서 이를 사용하여 local map을 공유된 환경의 global understanding을고 통합한다.

또한, resource management를 이야기하는데, Centralized C-SLAM은 single server computation에 의존하고, 로봇들과 server간 통신 병목현상을 겪을 수 있다고 한다.<br>
(which limits their scalability _ 확장성의 제한)

또한, large scale 및 challenging한 env에서 stable connection을 유지하기 어려울 수 있다는 것도 이야기하며, 결국 decentralized가 좋다를 소개한다.

**그래서 본 논문에서는,**

resource-efficient한 C-SLAM framework의 새로운 기술을 제안한다.

**특징**은 다음과 같다.

- **Fully decentralized**
- supports different types of sensors(stereo, RGB-D, LiDARs)
- **sporadic connectivity**
- **Inter-robot** loop closures
- Fewer communication resources

<br>

해당 논문에서 주장하는 **Contribution**은 다음과 같다.<br>
간헐적 통신 쪽 내용이 주를 이루는 것 같다.

- algebraic connectivity maximization을 기반으로 한 통신 제약 조건에서의 sparse budgedted **inter-robot loop closure**
- sporadic Inter-robot communication에 적합한 decentralized approach
- ROS2
- 데이터 셋 및 실제 실험을 통한 평가

Open-Source C-SLAM frameworks는 다음과 같다.

<p align="center"><img src="/MyPDF/swarm(1).png" width = "700" ></p>

Kimera-Multi는 읽어볼 예정이고, LAMP 2.0은 포스팅 작업 중이다.

<br>

# System Overview
<p align="center"><img src="/MyPDF/swarm(2).png" width = "700" ></p>

3종류의 모듈로 이루어져 있다.

---

첫번째 모듈은 **neighbor management module**이다.

이 모듈은 로봇들이 communication range에 있는지 **지속적으로 체크**하는 모듈이다.

즉 로봇들이 ROS에서 communication만을 위한 message가 통신 되는지 정도 확인하는 모듈이다.<br>
(heartbeat message at a fixed rate)

다른 모듈들은 neighbor management module을 query해서 가용 로봇을 판단한다.

이는 시스템을 **scalable(Property 1.)**, 즉 확장성있게 만든다.

> **Property 1. Scalable**<br>
로봇의 개수가 predetermining되지 않는다. 전체 작업에서 통신들이 꼭 연결되어 있을 필요가 없다.<br>
random rendezvous가 가능하게 된다.

--- 

front-end는 odometry estimate을 입력으로 받는다.(odometry를 얻는 임의의 기술을 통해)

front-end는 learning method등을 사용해 global descriptor를 추출하고, 3D Keypoint등을 통해 local descriptor를 추출한다.

global descriptor는 로봇 간의 place recognition을 식별하는데 사용, local은 3D registration에 사용.

> **Property 2. Flexible**<br>
framework는 여러 센서들을 support.

여기서의 Loop closure detection process는 **indirect한 Inter-robot을 활용**.

직접적인 관측에 제한을 둔다.

이는 맵 어디에서든 loop closure를 찾을 수 있으며 로봇 간 통신을 통해 수행한다.

다른 하나 이상의 로봇이 통신 범위 내에 있을 때 활성화된다.

> **Assumption 1.**<br>
The robots can transmit data to their neighbors.


🦾**중간 정리**<br>
로봇끼리 통신이 가능하다는 가정.<br>
Indirect Inter-robot에서는 로봇간 통신 범위에 들어왔을 때 loop closure
<br>
{: .notice--info}

--- 
back-end에서는 **Intra-robot(로봇 내)**과 **Inter-robot(로봇 간)**의 loop closure measurement 결과는 odometry 측정치와 결합되어 pose graph로 만들어진다.

Local pose graph는 최적화를 수행하기 위해 **선택된 로봇**에게 전송되고,

resulting estimate은 다시 **각각의 로봇에게 다시 전송**된다.

최적화를 수행하는 로봇은 간단한 negociation을 통해 선택된다고 한다. 이는 Property 3.이다.

> **Property 3. Decentralized**<br>
모든 연산은 로봇에서 수행, without any central authority.<br>
rely only peer-to-peer communication.

<br><br>

# Front-End
Swarn-SLAM의 front-end의 main task는 **indirect inter-robot loop closure detection**이다.

이는 C-SLAM에서 가장 challenging한 problem이다.

여러 관련 기술들과 유사하게, global matching이 후반 단계에서의 local feature를 사용하여 verified place recognition candidate를 생성하는 **two-stage 접근법**을 채택했다고 한다.

global matching에는 inexpensive한 **compact descriptor**를 사용했다고 하고,

**expensive한 local matching**은 가장 좋은 후보 일치에 대해서만 수행된다고 한다.

Front-end의 resource-efficiency를 더 높이기 위해 **pose graph의 algebraic connectivity(??)를 최대화하는 global matching 후보를 선택**하기 위한 novel sparsification한 알고리즘을 제안한다.

이는 estimation error를 감소한다고 한다.

## Global Matching
각각의 Keyframe마다 similarity score로 비교되는 **compact descriptor**는 sensor data로 부터 추출되고, 이웃한 로봇들에게 전송된다.

두 로봇이 만나면 simple bookkeeping이라는 것을 수행하여, 이미 global descriptor를 알고 있는지 아니면 보내줘야하는지를 결정한다.

[ScanContext](https://ieeexplore.ieee.org/abstract/document/8593953)를 LiDAR scan의 global descriptor로 활용하고, 이미지 기반이라면 recent CNN-based CosPlace를 활용했다고 한다.

descriptor matching을 위해 cosine similarity를 기반으로 한 nearest neighbor을 사용한다고 한다.

NetVLAD를 지원하고 본 논문의 시스템은 다른 compact descriptor extraction algorithm을 쉽게 사용할 수 있도록 하였다.(확장성)

> **Assumption 2.**<br>
Place recognition은 compact descriptor를 이용하여 수행.<br>
Geometric verification은 대규모 local feature collection을 사용하여 계산된다.

**multi robot pose graph** 정의는 다음과 같다.
<p align="center"><img src="/MyPDF/swarm(3).png" width = "700" ></p>

$V$는 모든 로봇들의 pose graph의 정점이다. 모든 vertex는 each **keyframe**과 상응한다.

$\mathcal{E}^{local}$ 은 intra-robot loop closure와 odometry를 이용한 local pose graph의 edge이다.

$\mathcal{E}^{global}$ 은 inter-robot loop closure에 상응하는 global pose graph의 edge이다.

$\mathcal{E}^{global}$ 은 $\mathcal{E}^{global}_{fixed}$ 와 

$\mathcal{E}^{global}_{candidate}$ 로 나뉜다.

이미 계산된 후보라면 fixed, **계산할 후보면 candidate**이다.

제안된 candidate prioritization은 Pose estimate과 같은 상세한 측정치가 필요하지 않다고 한다.

local과 global에서 고정된 measurement는 가중치 없는 방향없는 edge이며,

후보(candidate) edge만 각각의 similarity score에 해당하는 추가 가중치 값을 포함한다고 한다.

이러한 reduced multi robot pose graph는 global matching information에서 직접 구축되며, 추가적인 inter-robot communication이 필요없다고 한다.

---

각 time step마다 선택하는 edge의 수 B개는 사용자에 의해 설정된다.

이 budget(예산)은 로봇들의 통신 및 계산 능력을 고려해야한다.

이전 연구에서 널리 사용되는 candidate prioritization 지정 방법은 가장 높은 similarity score를 가진 상위 B개의 후보를 선택하는 **greedy prioritization**이다.

**본 논문의 제안**인 **spectral prioritization** process는 

pose graph sparsfication(포즈 그래프 희소화)을 candidate prioritization problem으로 구성하고

최근 연구인 **Spectral sparsification**을 활용한다.

최근 연구인 Spectral sparsification에서는 추정 정확도를 유지하면서 기존의 단일 로봇 pose graph에서 기존의 **중복 측정을 제거**하였다.

본 논문에서는 이와 달리, 상응하는 3D 측정을 계산하기 전에 inter-robot 후보(candidate) 매치에 대한 희소화를 수행하여 **비용이 많이 드는** 중복된 후보들의 robot geometric 검증을 위한 자원 사용을 줄이고 더 나은 정확도를 달성한다고 한다.

> **Property 4. Sparse**<br>
framework는 communication의 우선 순위 지정하여 더 적은 데이터 교환으로 더 나은 정확도를 달성하도록 한다.<br>
유사한 기술들에 비해 더 적은 통신을 요구하게 된다.

---

algebraic connectivity는 Maximum Likelihood Estimation problem의 solution에서 발생할 수 있는 worst-case를 조절한다.

Pose graph algebraic connectivity는 [rotational weighted Laplacian]에서 2번째로 작은 eigenvalue와 상응한다.

vertex쌍들의 rotational weighted Laplacian는 [다음](../../MyPDF/swarm(4).png)과 같이 정의된다.

<p align="center"><img src="/MyPDF/swarm(4).png" width = "700" ></p>

$\kappa_{ij}$ 는 edge weight

$\delta (i)$ 는 vertex $i$에 입사하는 모든 edge 집합

본 논문의 aprroach에서는

$\kappa_{e}=1 \ \forall \ e \in (\mathcal{E}^{local} , \mathcal{E}^{global}_{fixed})$

$\kappa_{e}=s \ \forall \ e \in (\mathcal{E}^{global}_{candidate})$

where, $s \in [0,1] $

이러한 방식이 기존 edge의 신뢰도에 대한 추가 정보를 전달할 필요가 없다고 한다.

Reference를 잘 확인해보면 $L$이 n by n matrix인 것도 확인할 수 있다.

<p align="center"><img src="/MyPDF/swarm(5).png" width = "300" ></p>

[위 그림](../../MyPDF/swarm(5).png)을 보면 Pose graph의 모습을 알 수 있다.

정리하면,

local edge는 위에서 언급한 것처럼 intra-robot, 즉 단일 로봇에서 우리가 아는 pose graph의 모습이고

global edge는 inter-robot, 즉 다중 로봇끼리의 pose graph의 연결이다.

이미 우선 순위 지정(prioritization)이 끝나면 fixed, 후보면 candidate이다.

[정의한 Laplacian](../../MyPDF/swarm(4).png)을 토대로 edge간의 weight를 지정해주며, 혹은 같은 정점이면 위에서 말한 $\kappa_{e}$를 종류에 따라 값을 선택해준다.

<p align="center"><img src="/MyPDF/swarm(6).png" width = "700" ></p>

이를 [위 식](../../MyPDF/swarm(6).png)처럼 sum of subgraph로 구성한다.

Edge의 종류에 따라 Laplacian도 달라지니 다음과 같이 구성하였고,

여기서 이제 $w_e \in 0,1 $이 candidate를 fix하냐 마냐에 따른 binary 값이다.

위에서 언급한 budget에 따라 선택하는 후보의 개수인 $B$개를 선택하는 문제가 되고,

$B$개를 선택했을 때, 연결성의 어떤 대수적인 특성(algebraic connectivity)이 최대화되는 candidate edge를 선택하게 된다.

이 내용은 다음과 같이 정의된다.

<p align="center"><img src="/MyPDF/swarm(7).png" width = "700" ></p>

이 정도로만 이해하고 넘어갔다.

> 결국 후보 edge를 넣어주거나 안 넣어줄 때의 합(local + fixed + candidate)의 second smallest eigen value $\lambda_{2}$가 최대화 되는 candidate를 선정하는 과정을 통해 Prioritization을 수행한다.

(이 부분에 대한 정확한 내용은 본 논문에서 reference로 제시한 Kevin J. Doherty의 2022년도 IROS, ICRA 논문 2편을 참고해야할 것 같다. [참고자료](#참고자료) [2], [3])

[Problem [1]](../../MyPDF/swarm(7).png)이 NP-Hard problem이어서 [참고자료](#참고자료) [2]에서의 방법을 사용하였다는 것까지 부연설명을 한다.

<p align="center"><img src="/MyPDF/swarm(8).png" width = "500" ></p>

spectral technique을 사용하여 중복되지 않고 더 많은 정보를 제공한다고 한다.

## Local Matching
Global matching, 즉 로봇 간 loop closure 후보가 선택되면, 다음 단계는 local matching을 수행하는 것이다.

이 단계에서는 ORB나 point cloud와 같은 larger collection local feature를 사용하여,

두 후보 정점 간의 3D relative pose를 계산한다.

visual feature 및 geometric vertification을 위해서는 RTAB-Map 라이브러리를 사용하고,

robust point cloud registration을 위해서는 TEASER++을 사용한다.

동일한 loop closure를 두 번 계산하지 않기 위해 다른 논문의 방식(trivial monolog)을 사용했으며, 후보당 두 정점 중 하나만 전송되며 수신 로봇에서 상대 위치를 계산한다.

여기서 더 설명하는 통신 정책 관련 부분은 넘어가도록 하겠다.

### Inter-Robot Communication
spectral matching과 정점 교환 정책(vertex exchange policy) 모두 동적으로 brocker를 선정해줘야할 필요가 있다고 한다.

하지만 본 논문의 구현에서는 가장 낮은 ID를 가진 로봇을 brocker(중개자 정도)로 선정한다.

보다 더 informed decentralized negotiation을 통해 선정될 수 있다.

# Back-End
back-end의 역할을 front-end에서 획득한 odometry, loop closure measurement를 pose graph에 모으고,

noisy한 측정 값을 기반으로 가장 가능성 있는 map과 pose를 추정하는 것이다.

본 논문에서 푸는 Maximum Likelihood Estimation (MLE)는 다음과 같다.

<p align="center"><img src="/MyPDF/swarm(9).png" width = "700" ></p>

$R_{ij}$는 i와 j의 relative pose


식을 살펴보면 결국 $R$과 $t$ 모두 relative pose를 i로 매핑후 그 둘의 차이를 minimize하는 것이 목표이다.

$\kappa$와 $\tau$는 rotation and translation information parameter들이며,

측정 confidence와 관련있다.

$\mathcal{e}$ 는 모든 local과 global edge이다.(선정된)

이 또한, 간단한 분산형 접근 방식을 적용하기 위해

계산 수행을 위한 로봇이 동적으로 선정된다.

마찬가지로 계산이 완료되면 다른 로봇들에게 전송한다.

이외 내용은 GTSAM 사용해서 pose graph를 최적화한다 이런 내용이다.

Central authority가 없어도 하나의 전역 위치 추정치로 수렴하기 위해

anchor 선택 process를 도입했다고 한다.

요약하면, pose graph 최적화를 위해 참조 프레임이 가장 낮은 로봇의 첫 번째 pose를 anchor로 선정한다.

0,4,5 가 만나면 4,5는 0의 첫번째 pose를 참조 프레임으로 굥유

다시 2,3,4 가 만나면 4가 0의 pose를 참조하기 때문에, 2, 3 또한 0의 첫번째 pose를 참조

이러한 반복적 추정을 토해 대규모 로봇 그룹으로 확장 가능하다고 한다.

# Experiment
간단하게 넘어간다.

commuication, ATE, Time에서 모두 좋은 모습을 보인다.

<br>

# 결론 및 고찰
다중 로봇 pose graph의 최적화 내용 또한 처음보는 내용이라 신기했지만,

다중 로봇 시스템에서 centralized system을 적극적으로 탈피하려는 논문이라고 평가할 수 있다.

특히 inter-robot, 로봇과 로봇 간 **통신 범위에 들어왔을 때** 최적화나 loop closure measurement 관련 작업을 하는 것이 주요 contribution 인 것 같다.

Decentralized를 위한 여러 노력에서 더 효율적으로 계산할 로봇을 선정하거나 다수의 pose graph optimization 관련 분야에서 연구가 더 나오면 좋을 것 같다.

다른 multi robot 논문들에서 prioritization 관련 방법을 찾아보고, Swarm-SLAM도 open source로 공개했으니 docker로 구성해서 한 번 돌려볼 계획이다.

<br>

# 참고자료

[1] [Lajoie, Pierre-Yves, and Giovanni Beltrame. "Swarm-slam: Sparse decentralized collaborative simultaneous localization and mapping framework for multi-robot systems." arXiv preprint arXiv:2301.06230 (2023).](https://arxiv.org/abs/2301.06230)

[2] [Doherty, Kevin J., David M. Rosen, and John J. Leonard. "Spectral measurement sparsification for pose-graph SLAM." 2022 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS). IEEE, 2022.](https://ieeexplore.ieee.org/abstract/document/9981584)

[3] [Doherty, Kevin J., David M. Rosen, and John J. Leonard. "Performance guarantees for spectral initialization in rotation averaging and pose-graph SLAM." 2022 International Conference on Robotics and Automation (ICRA). IEEE, 2022.](https://ieeexplore.ieee.org/abstract/document/9811788)

📣<br>
포스팅에서 오류나 궁금한 점은 Comments를 작성해 주시면, 많은 도움이 됩니다.💡
{: .notice--info}
