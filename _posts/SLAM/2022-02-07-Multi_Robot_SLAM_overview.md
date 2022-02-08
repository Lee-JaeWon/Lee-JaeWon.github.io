---
title : "'Multi-Robot SLAM: An Overview' Paper Review"
category :
    - SLAM
tag :
    - SLAM
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---

Multi-Robot SLAM system의 대략적인 이해를 위한 논문 리뷰 포스팅입니다.<br><br>
기본적으로 SLAM에 대한 이해가 필요합니다.<br><br>

**Reference** : Mahmoud A. Abdulgalil, (2018), Multi-robot SLAM: An Overview and Quantitative Evaluation of MRGS ROS Framework for MR-SLAM, RiTA 2017: Robot Intelligence Technology and Applications 5 pp 165-183

## 1. Introduction
### Autonmous Vehicle 개요

Autonomous Vehicle(차량, 모바일 로봇)의 중요한 요구사항은 실내/실외, 구조물이 있거나 없거나 동적이거나 정적이거나 하는 상황(환경)에서의 인식이다.

vehicle의 상태나 환경의 상태에 대한 잘못된 신뢰(belief)는 vehicle로 부터 잘못된 동작을 유발한다.

Endsley(Reference)이라는 사람의 situation awareness에 대한 묘사는 다음과 같다.

그녀는 상황 인식을 공간과 시간을 포함한 환경에서의 요소의 인식(perception of elements in the environment), 그들이 가지고 있는 의미에 대한 이해(Comprehension of their meaning), 그리고 가까운 미래에서 그들의 상태에 대한 예상(추정). 이렇게 3가지로 정의했다.

- **Perception(인식)**은 다양한 요소에 대한 인식과 그들의 현재 상태를 제공한다.

- **Comprehension(이해)**은 인식된 요소에 대한 전체적인 의미(이들이 전체적으로 어떻게 fit되는지, 지금 상황이 무슨 상황인지, 목표내에서 어떤 의미를 가지는지)의 이해를 produce한다.

- **Projection(예상,추정, 혹은 투영)**은  가능하고 개연성있는 미래의 상태와 사건에 대해 / 예상되는 진전에 대한 인식을 produce한다.<br>
(Projection produces an awareness of the likely evolution of the situation, its possible/probable future states and events.)

**Simultaneous Localization and Mapping(SLAM)**은 차량의 자율성을 위해 필요한 **첫 번째 수준(first level)의 상황 인식 기술**로서 인식을 가능하게 한다.

### i-level, g-level

일반적은 로봇의 행동은 **개인(i-level) 수준**의 행동과 **그룹(g-level) 수준**의 행동으로 분류할 수 있다.

로봇공학에서 주어진 문제는 어떻게 주어진 원하는 g-level의 행동에 대해 i-level 동작을 설계하냐는것이다.

i-level 동작으로서의 SLAM은 점점 해결된 문제로 간주되고 있지만,

g-level 동작으로서의 Multi-Robot SLAM(MR-SLAM)은 society level이 등장함에 따라 집단의 행동이 각자의 행동의 단순한 합이 아니라는 사실을 고려할 때, 여전히 광범위한 연구의 여지가 있다.

본 논문에서는 멀티 로봇 SLAM 문제를 해결하기 위한 알고리즘에 대한 **개요**를 제시한다.

다중 로봇 SLAM 문제의 범위가 넓고 문제에 접근하는 방식이 매우 다양하기 때문에 각각의 알고리즘에 대한 종합적이고 자세한 분석이 가능하지 않다.

그러나 알고리즘의 개요와 제안된 분류법이 제시된다.

## 2. Simultaneous Localization and Mapping

**unknown environment를 탐색**할 수 있도록 해준다.

SLAM은 motion planning 및 task allocation(작업 할당)과 같은 다른 기능들이 종속되는 첫 번째 단계(First Step)이다.

이러한 모든 작업은 ***로봇이 자신의 위치를 파악하고 환경에 대한 모델을 가지고 있어야하기 때문***이다.

### Difficulty & Uncertainty

SLAM은 ‘chicken or the egg’의 다른 버전으로 인과적 딜레마를 겪기에 어렵다.

그 이유는 로봇은 localization을 위해 map이 필요하고 map을 만들기 위해서는 localization이 필요하기 때문이다.

또한, 치명적인 요소로 **불확실성(uncertainty)**이 있다. 이는 환경과 로봇에 대한 잘못된 정보를 유발할 수 있다.

실제 world는 본질적으로 모든 것을 정확히 예측할 수 없으며,

센서는 완벽하지 않는 device로 필연적으로 error를 가지고 있다.

그렇기에 본질적으로 센서가 인식할 수 있는 한계가 있다.

모터와 같은 Robot Actuation 또한 control noise나 마모와 같은 이유때문에 예측 불가능성이 존재한다.

모델은 현실 세계의 추상화이다. 따라서 로봇과 그 환경은 기본적인 물리 프로세스만 부분적으로 모델링한다.

모델 오류는 (최첨단 시스템이라고 하더라도) 대체로 무시되어오는 불확실성의 원인 중 하나이다.

Autonomous System은 real-time system인데, 수행할 수 있는 연산량을 제한한다.

많은 알고리즘은 정확성을 희생하여 시간에 대응한다.

### A. Mathematical Formulation

SLAM은 이름과 같이 Localizaation과 Mapping의 조합일 뿐이다.

수학 용어로 기술된 localization problem은 $x_{1:t}$로 정의된 시간 1부터 시간 t까지 시스템의 신뢰할 수 있는 궤도의 추정이다.

환경($z_{1:t}$)의 모든 관측과 로봇 $u_{1:t-1}$에 대한 제어 입력이 주어진 경우 $x_{1:t}$는 확률 밀도 함수로 표현된다.

$$ p(x_{1:t} | z_{1:t}, u_{1:t-1}) $$

<br>
이를 SLAM으로 확장하면, $m$으로 표시된 map과 함께 로봇의 상태 궤적의 estimation이 된다.

$$ p(x_{1:t}, m| z_{1:t}, u_{1:t-1}) $$


실제 상황에서는 지정된 시간이나 목표내에서 single robot이 이를 달성할 수 없다.<br>
이는 이러한 문제나 작업을 수행하기 위해 협력이 가능한 로봇 팀을 사용할 필요가 있다는 것을 요구한다.<br>
그럼에도 불구하고, Single-Robot SLAM에서 Multi-Robot SLAM으로의 확장은 간단하지 않다.<br>

### B. Online vs Offline
- Offline SLAM
지도와 시간이 시작한 이후의 로봇의 궤적을 모두 추정하는 것.<br>
수학적 용어로 이것은 확률 밀도 함수(Probability Dnesity function)를 추정하는 것과 동일하다.<br>

- Online SLAM
Offline SLAM의 단순화된 버전은 Online SLAM으로<br>
현재 시간에만 state belief를 추정하는 것과, 가장 최근의 측정 및 이전의 state belief이 주어진 가장 최근의 맵으로 주어진다.<br>
수학적 용어로 다음과 같은 확률 밀도 함수와 같다.

$$p(x_{t}, m| x_{t-1}, z_{t}, u_{t-1})$$

### C. Map Representation

## 3. Multi-Robot SLAM (MRS)

**MR-SLAM**은 single-robot SLAM의 확장된 기능이며 unknown environment에서 몇몇의 로봇들이 협력적으로 탐색하는 작업이다.

하지만, 이 확장은 직설적이지 않다(혹은 간단하지 않다.)(is not straight-forward).

(무슨 말이냐 하면 위에서 언급한 것처럼 single robot 여러대로 그저 탐색하는 것이 Multi-Robot System과 직결되지 않는다는 것이다.)

각각의 로봇이 자체의 로컬 reference frame을 사용하여 스스로 매핑하기 때문에,<br>
지도를 로봇간 글로벌 맵으로 병합하려면 로봇 간 reference frame이 관련되어야하고, 이들 사이의 변환을 찾아야 한다.

MR-SLAM을 사용하면 환경을 훨씬 빠르게 mapping가능하며, 데이터의 중복성(redundancy)으로 인해 system state belief에 대한 불확실성을 줄일 수 있다.

게다가, 만약 heterogeneous(not homogeneous) sensor를 각각의 로봇이 사용하면,<br>
지도에서 단일 로봇으로 만드는 단일 지도보다 더 많은 feature를 담고 있기에<br>
하나 이상의 로봇의 고장에 대해 시스템을 견고하게 만든다는 것을 말할 필요도 없다.<br>
(처리가 분산(decentralized)되어있는 경우)

### MR-SLAM 문제의 주요 측면

#### A. Centralized vs Decentralized
- Centralized Data-Processing
    - 이 접근 방법은, 로봇의 협력 그룹 중 a central agent나 외부 agent에서 데이터 처리가 일어난다.<br>
    Central Unit은 모든 로봇의 information fusion을 책임지고, 다시 개별 로봇에게 피드백을 제공하거나 융합된 정보를 공유하여 그들의 self-localization을 강화시킨다.<br><br>
    하지만 이 접근 방식은 직관적이고 간단하지만 당연하게도 Central agent의 장애나 문제가 전체 시스템에 장애를 일으키기 때문에, 시스템이 문제에 노출된다.<br>
- Decentralized Data-Processing
    - 분산형 시스템은 일반적으로 합동 구조에 의해 관리되는 여러 개의 상호 연결된 에이전트로 구성된다.<br>
    이 시스템은 대처하기 쉬우며, 에이전트의 수를 증가하는 것에 효율적이다.<br>
    - 이 시스템은 partial-failure property(부분-고장 속성)를 지니고 있다.<br><br>
    왜냐하면, single point of failure(단일 장애점)가 없으며, 심지어 일부 분산된 에이전트가 고장나더라도 다른 에이전트는 여전히 임무를 수행할 수 있기 때문<br>
    (단일 장애점이란 구성 요소 중 어느 것이라도 동작하지 않으면 전체 시스템이 중단되는 요소를 말한다.)<br>
        
    <p align="center"><img src="/MyPDF/single_point_of_failure.png" width = "200" ></p>
        
    - n로봇에 대하여 총 n(n-1)의 연결을 가져야한다
    - Centralized 방식의 단점을 해결하고 localization의 정확도에 도움이 되는 중복적인 데이터를 제공한다.
    - 견고하지만 잠재적인 단점을 지니고 있다.<br>
    이러한 단점은 대규모 대역폭 요구와 네트워크 트래픽의 형태로 주어진다.<br>
    - 나이가 각각의 로봇은 (must have) 반드시 스스로 충분한 계산 능력을 갖춰야 수신된 데이터를 처리하고 유용한 정보에 포함할 수 있다.

#### B. Communication and Information Sharing
MR-SLAM에서 할 수 있는 질문 중 또 다른 하나는 어떻게 로봇 간 통신 문제를 다룰 것인가이다.<br><br>
모든 무선(wireless) 통신 시스템은 제한된 대역폭, 지연율, 가능 영역을 가지고 있다.<br>

#### C. Map Fusion
Information process는 MR-SLAM이 직면한 가장 큰 과제 중 하나이다.<br><br>
로봇들이 주변을 대표하는 글로벌 통합 map을 가지기 위해서는 각 로봇들이 독자적으로 만든 local map을 병합해야한다.<br><br>

#### D. Mutli-Robot SLAM Implementations
