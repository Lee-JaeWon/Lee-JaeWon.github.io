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
기본적으로 SLAM에 대한 이해가 필요합니다.

## 1. Introduction
### Autonmous Vehicle 개요

Autonomous Vehicle(차량, 모바일 로봇)의 중요한 요구사항은 실내/실외, 구조물이 있거나 없거나 동적이거나 정적이거나 하는 상황(환경)에서의 인식이다.

vehicle의 상태나 환경의 상태에 대한 잘못된 신뢰(belief)는 vehicle로 부터 잘못된 동작을 유발한다.

Endsley(사람)이라는 사람의 situation awareness에 대한 묘사는 다음과 같다.

그녀는 상황 인식을 공간과 시간을 포함한 환경에서의 요소의 인식(perception of elements in the environment), 그들이 가지고 있는 의미에 대한 이해(Comprehension of their meaning), 그리고 가까운 미래에서 그들의 상태에 대한 예상(추정). 이렇게 3가지로 정의했다.

Perception(인식)은 다양한 요소에 대한 인식과 그들의 현재 상태를 제공한다.

Comprehension(이해)은 인식된 요소에 대한 전체적인 의미(이들이 전체적으로 어떻게 fit되는지, 지금 상황이 무슨 상황인지, 목표내에서 어떤 의미를 가지는지)의 이해를 produce한다.

Projection(예상,추정, 혹은 투영)은  가능하고 개연성있는 미래의 상태와 사건에 대해 / 예상되는 진전에 대한 인식을 produce한다.

(Projection produces an awareness of the likely evolution of the situation, its possible/probable future states and events.)

Simultaneous Localization and Mapping(SLAM)은 차량의 자율성을 위해 필요한 첫 번째 수준(first level)의 상황 인식 기술로서 인식을 가능하게 한다.

### i-level, g-level

일반적은 로봇의 행동은 개인(i-level) 수준의 행동과 그룹(g-level) 수준의 행동으로 분류할 수 있다.

로봇공학에서 주어진 문제는 어떻게 주어진 원하는 g-level의 행동에 대해 i-level 동작을 설계하냐는것이다.

i-level 동작으로서의 SLAM은 점점 해결된 문제로 간주되고 있지만,

g-level 동작으로서의 Multi-Robot SLAM(MR-SLAM)은 society level이 등장함에 따라 집단의 행동이 각자의 행동의 단순한 합이 아니라는 사실을 고려할 때, 여전히 광범위한 연구의 여지가 있다.

본 논문에서는 멀티 로봇 SLAM 문제를 해결하기 위한 알고리즘에 대한 **개요**를 제시한다.

다중 로봇 SLAM 문제의 범위가 넓고 문제에 접근하는 방식이 매우 다양하기 때문에 각각의 알고리즘에 대한 종합적이고 자세한 분석이 가능하지 않다.

그러나 알고리즘의 개요와 제안된 분류법이 제시된다.

## 2. Simultaneous Localization and Mapping

unknown environment를 탐색할 수 있도록 해준다.

SLAM은 motion planning 및 task allocation(작업 할당)과 같은 다른 기능들이 종속되는 첫 번째 단계(First Step)이다.

이러한 모든 작업은 로봇이 자신의 위치를 파악하고 환경에 대한 모델을 가지고 있어야하기 때문이다.

### Difficulty & Uncertainty

SLAM은 ‘chicken or the egg’의 다른 버전으로 인과적 딜레마를 겪기에 어렵다.

그 이유는 로봇은 localization을 위해 map이 필요하고 map을 만들기 위해서는 localization이 필요하기 때문이다.

또한, 치명적인 요소로 불확실성(uncertainty)이 있다. 이는 환경과 로봇에 대한 잘못된 정보를 유발할 수 있다.

실제 world는 본질적으로 예측할 수 없다.

센서는 완벽하지 않는 device로 필연적으로 error를 가지고 있다.

그렇기에 본질적으로 센서가 인식할 수 있는 한계가 있다.

모터와 같은 Robot Actuation 또한 control noise나 마모와 같은 이유때문에 예측 불가능성이 존재한다.

모델은 현실 세계의 추상화이다. 따라서 로봇과 그 환경은 기본적인 물리 프로세스만 부분적으로 모델링한다.

모델 오류는 /최첨단 시스템이라고 하더라도/ 대체로 무시되어오는 불확실성의 원인 중 하나이다.

Autonomous System은 real-time system인데, 수행할 수 있는 연산량을 제한한다.

많은 알고리즘은 정확성을 희생하여 시간에 대응한다.

## 3. Multi-Robot SLAM (MRS)

MR-SLAM은 single-robot SLAM의 확장된 기능이며 unknown environment에서 몇몇의 로봇들이 협력적으로 탐색하는 작업이다.

하지만, 이 확장은 직설적이지 않다(혹은 간단하지 않다.)(is not straight-forward).

(무슨 말이냐 하면 위에서 언급한 것처럼 single robot 여러대로 그저 탐색하는 것이 Multi-Robot System과 직결되지 않는다는 것이다.)

각각의 로봇이 자체의 로컬 reference frame을 사용하여 스스로 매핑하기 때문에,<br>
지도를 로봇간 글로벌 맵으로 병합하려면 로봇 간 reference frame이 관련되어야하고, 이들 사이의 변환을 찾아야 한다.

MR-SLAM을 사용하면 환경을 훨씬 빠르게 mapping가능하며, 데이터의 중복성(redundancy)으로 인해 system state belief에 대한 불확실성을 줄일 수 있다.

게다가, 만약 heterogeneous(not homogeneous) sensor를 각각의 로봇이 사용하면,<br>
지도에서 단일 로봇으로 만드는 단일 지도보다 더 많은 feature를 담고 있기에 하나 이상의 로봇의 고장에 대해 시스템을 견고하게 만든다는 것을 말할 필요도 없다.

(처리가 분산(decentralized)되어있는 경우)

### MR-SLAM 주요 문제

- Centralized vs Decentralized
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
        - 나이가 각각의 로봇은 (must have) 반드시 스스로 충분한 계산 능력을 갖춰야 수신된 데티러를 처리하고 유용한 정보에 포함할 수 있다.