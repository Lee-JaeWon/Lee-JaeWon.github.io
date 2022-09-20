---
title : "Motion Planning : Velocity Obstacle"
category :
    - Multi_Robot
tag :
    - Multi_Robot
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---

Velocity Obstacle에 대해 알아본다.

# Intro
Human Interaction이 가능한 Robot Navigation을 위해서는 단순히 로봇의 경로만을 추구하는 것이 아닌 여러 제약 조건과 사람과 공존하기 위한 사회적 규칙에 기반을 둔 적절한 접근법이 필요하다.<br><br>
이와 함께 모바일 로봇의 task를 수행하기 위해서는 대표적으로 SLAM이나 Path Planning과 같은 여러 주요 기술이 사용된다.<br><br>
하지만 Multi Robot 개념으로 확장되었을 때, 기존 Path Planning 방식에 더해 여러 로봇이 여러 사람과 공존하기 위한 다른 접근법을 수행해야 할 수도 있다.<br><br>
Motion Planning 기법 중 하나인 VO(Velocity Obstacles)라는 속도 공간에 대한 개념을 더해 한 작업 공간에 여러 로봇과 여러 사람이 있을 때 원하는 속도에는 가깝지만, 충돌에는 자유로운 속도를 찾아낸다.<br><br>
Path Planning과 Motion Planning의 차이는 설명에 따라 모호할 수도 있다.<br><br>
하지만 경로 계획은 원하는 상태로 도달하기 위해 경로를 찾는 문제를 설명하는데 사용될 수 있으며,<br>
모션 계획 혹은 모션 제어는 동일한 문제를 설명하지만, time step이 고려되며, 구체적으로 로봇이 계획된 경로를 따르기 위해 실행해야 하는 실제 명령에 관한 동작을 수행할 수 있다.<br><br>
Multi Robot이 단순히 Path Planning을 통해서만 목표를 달성하는 것이 아닌 로봇 간 관계를 인지하고 기하학적으로 장애물을 회피하여 사람들과 공존하기 위한 Motion Planning 알고리즘을 사용해서 task를 수행한다는 것의 차이에 대한 개념을 얻고자 한다.<br><br>

# Basic Concept of VO
[Velocity Obstacle](https://en.wikipedia.org/wiki/Velocity_obstacle)<br><br>
로봇 공학에서 Velocity Obstacle은 다른 로봇이 현재의 속도를 유지한다고 가정할 때,<br>
어떤 순간에 다른 로봇과 충돌하게 되는 한 로봇의 모든 속도의 집합이다.<br><br>
로봇이 velocity Obstacle안에 있는 속도를 선택하면 결국 두 로봇은 충돌하게 되며,<br>
velocity obstacle 외부에 있는 속도를 선택하면 충돌이 발생하지 않도록 보장된다.<br><br>

# Motion Planning in Dynamic Environments Using Velocity Obstacles.(1998)
본 논문은 속도 공간(velocity space)에서 정적인 혹은 동적인 장애물을 회피한기 위한 회피 동작을 선택한다.<br>
이것은 **현재 위치와 속도에 기반**한다. 이것은 시간의 함수로써 위치를 얻어내기 위해 속도를 적분하지 않으므로 first-order 방법이다.<br>

회피 기동(avoidance maneuvers)은 **velocity obstacles의 외부에 있는 로봇의 속도를 선택**함으로써 이루어진다.<br><br>
VO는 주어진 속도로 움직이는 주어진 장애물과 어떤 미래에 충돌을 초래할 로봇 속도의 집합을 나타낸다.<br><br>
회피가 동적으로 실현 가능한지 보장하기 위해, 회피 속도(avoidance velocities)의 집합은 로봇의 가속도 제약 조건(or some constraint)에 의해 정의된 허용되는 속도의 집합과 교집합된다.<br><br>

동적인 환경에서의 Motion Planning은 상태 공간(State-space)에서의 planning이 요구되므로 어려운 문제이다.<br>

## VO
단순성을 위해, circular robot과 obstacle로 제한하여 분석하고, 회전이 없는 planar 문제를 고려한다.<br>
일반적인 다각형은 다수의 원으로 표현할 수 있기 때문에 이것은 심각한 제한이 아니다.<br><br>
장애물은 임의의 경로를 따라 움직인다 가정하며 이것의 순간적인 상태(위치와 속도)는 알고 있거나 측정 가능하다.<br><br>
<p align="center"><img src="/MyPDF/multi_vo(2).png" width = "500" ></p>
위 Figure 1에서 time $t_0$에서의 속도 $v_A$, $v_B$를 가진 두 개의 circular robot objects를 고려해보자.<br><br>
원 A는 로봇이고, 원 B는 장애물이라고 하자.<br>
VO를 계산하기 위해 먼저 $A$를 점 $\hat{A}$까지 줄이고, 원 $B$는 $\hat{B}$에서 $A$의 반지름만큼 추가로 확대하며, $B$를 $A$의 Configuration Space에 매핑한다.<br><br>
각각의 moving object의 상태는 각각의 위치와 각각의 중심에 부착된 속도 벡터로 표현된다.<br><br>
$Collision \ Cone$, $CC_{A,B}$를 정의하자. 이것은 $\hat{A}$와 $\hat{B}$ 사이의 충돌 상대 속도의 집합이다.<br>

$$ CC_{A,B}=\{v_{A,B} \mid \lambda_{A,B} \cap \hat{B} \neq \emptyset \} $$

$v_{A,B}$는 B에 대한 A의 상대 속도이다. $v_{A,B}=v_A-v_B$<br>
$\lambda_{A,B}$는 $v_{A,B}$의 선(line)이다.<br><br>
이 원뿔(cone)은 Figure2와 같이 $\hat{A}$에서 $\hat{B}$까지의 두 접선 $\lambda_{f}$와 $\lambda_{r}$에 의해 bounded되었으며 $\hat{A}$에 정점(apex)이 있는 평면 부채꼴이다.
<p align="center"><img src="/MyPDF/multi_vo(3).png" width = "500" ></p>
$\hat{B}$에 대한 두 접선인 $\lambda_{f}$와 $\lambda_{r}$ 사이에 있는 상대 속도는 A와 B사이 충돌을 유발한다.<br><br>
결국, { obstacle $\hat{B}$ }이 현재 shape과 speed를 유지한다는 것을 제공하면, $CC_{A,B}$ 밖에 있는 상대 속도는 충돌로부터 자유로움(collision-free)을 보장한다.<br><br>
이 collision cone은 특정 쌍의 로봇이나 장애물에 대해서만 특정되어있다. multiple obstacle을 고려하기 위해서는 $A$의 절대 속도에 대해 동등한 조건을 설정하는 것이 유용하다.<br><br>
이것은 collision cone $CC_{A,B}$ 에서의 각각에 속도에 간단하게 $B$의 속도, $v_B$를 추가하거나 Figure3과 같이 collision cone $CC_{A,B}$를 $v_B$로 이동함으로써 간단하게 수행된다.
<p align="center"><img src="/MyPDF/multi_vo(4).png" width = "500" ></p>

$Velocity \ Obstacle \ VO$는 다음과 같이 정의된다.<br>

$$ VO=CC_{A,B} \oplus v_B $$

$\oplus$는 [**Minkowski vector sum**](#minkowski-sum)을 의미한다.<br><br>
VO는 A의 절대 속도를 회피 속도와 충돌 속도로 나눈다.<br><br>
VO 밖에 있는 속도 $v_A$를 선택하면 B와의 충돌을 회피할 수 있다.<br>

$$ A(t) \cap B(t) = \emptyset \ \ if \ \ v_{A}(t) \not\in VO(t) $$

VO의 경계에서의 속도는 A가 B를 방목하는 결과를 초래할 것이다.<br><br>
또한, 정지 장애물의 VO는 $v_B$가 $0$이므로, 아래 식에서<br>

$$ VO=CC_{A,B} \oplus v_B \mid v_B = 0$$

$$ VO=CC_{A,B} $$

결국, $VO$는 상대 속도 원뿔과 동일하다.<br><br>

multiple obstacle을 피하기 위해서는 각자의 VO의 합을 고려해야한다.<br>

$$ VO=\bigcup\limits_{i=1}^m VO_{B_{i}}$$

$m$은 장애물의 개수이다.<br>
따라서 Figure4 처럼 회피 속도는 모든 VO 외부에 있는 속도 $v_A$로 구성된다.
<p align="center"><img src="/MyPDF/multi_vo(5).png" width = "400" ></p>

많은 장애물이 있는 경우, **충돌이 임박한 장애물**이 **충돌할 시간이 더 긴 사람보다 우선**하도록 장애물의 우선순위를 정하는 것에 유용할 수 있다.<br><br>
게다가, $VO$는 장애물 궤적의 선형 근사치에 기반하기 때문에 장애물이 직선을 따라 이동하지 않을 경우 원격 충돌을 예측하기 위해 $VO$를 사용하는 것은 부정확할 수 있다.<br><br>

어떤 시간 $t<T_h$에 로봇과 장애물 사이에 충돌이 발생할 경우 임박한(imminent) 충돌이라 부르며,<br>
여기서 $T_h$는 시스템 dynamics, 장애물 궤적 및 회피 동작의 계산 속도를 기반으로 선택된 적절한 시간 범위이다.<br><br>
imminent collisions를 설명하기 위해 아래 식 처럼 정의 된 $VO_h$ 집합을 빼서 $VO$ 집합을 수정한다.

$$ VO_h = \{ v_A \mid v_A \in VO, \ \parallel v_{A,B} \parallel \le \frac{d_m}{T_h} \} $$

여기서 $d_m$은 로봇과 장애물 사이의 가장 짧은 상대 거리이다.<br><br>
결국 $VO_h$ 집합은 시간 지평선(time horizon) 너머에서 발생하는 충돌을 초래할 수 있는 속도를 나타내게 된다.<br><br>
Figure5는 $VO_h$가 제거된 속도인 modified VO를 나타낸다. 
<p align="center"><img src="/MyPDF/multi_vo(6).png" width = "500" ></p>
<br>

# Reciprocal Velocity Obstacles for real-time multi-agent navigation.(2008)
## RVO(Recirprocal Velocity Obstacle)
RVO(Recirprocal Velocity Obstacle)는 VO에서 발생하는 진동(Oscillation) 현상을 해결하기 위해 고안된 방법이다. 굉장히 간단한 방법으로 부드럽고, 안전한 동작을 가능하게 한다.<br><br>
RVO역시 VO와 마찬가지로 서로 간의 직접적인 통신 없이 충돌 없는 동작이 가능하다.<br><br>
VO에서 진동이 발생하는 이유는 두 물체 A, B가 있을 때, A는 B의 VO 밖에 있는 속도를 선정하며 충돌을 피하게 된다.<br>

$$ A(t) \cap B(t) = \emptyset \ \ if \ \ v_{A}(t) \notin VO_B(t) $$

하지만 충돌을 피하기 위해 선호된 이전의 속도인 는 다시 다음 time step과 다음 새로운 상황에 대해 새로운 VO를 계산하여 새로운 속도 가 계산될 것이다.<br><br>
두 물체 A, B는 결국 목표를 향하기 때문에 다시 이전의 목표를 향한 속도를 선정하게되며, 다음 사이클에서는 다시 이러한 속도가 충돌을 일으킬 속도가 된다.<br><br>
즉, VO 밖의 속도를 선정하고 다시 목표를 향한 속도를 선택하는 과정이 반복되며 진동이 일어난다.<br><br>
RVO의 기본 원리는 굉장히 간단하다. VO 밖의 속도를 단순히 선택하는 것 대신 현재 속도와 다른 물체의 VO를 통해 나온 속도와의 평균을 선택한다.<br>

$$ RVO^{A}_{B}(v_B, \ v_A) = \{ v'_A \mid 2v'_A - v'_A \in VO^{A}_{B}(v_B) \}  $$

<p align="center"><img src="/MyPDF/multi_vo(8).png" width = "500" ></p>

<!-- ## The Avoidance Maneuver
이 섹션에서는 주어진 시간 범위내에서 미래의 충돌을 피하기위한 속도의 한 단계 변화로 구성된 회피 동작을 설명한다.<br><br>
로봇 역학 및 액추에이터 constraint를 설명하는 도달가능한 속도들의 집합에 대해 논의하는 것으로 시작한다.<br><br>

### The Reachable Avoidance Velocities
주어진 시간 간격 $\Delta t$에 걸쳐 주어진 상태에서 로봇 A가 도달 가능한 속도는 actuator 제약 조건을 가속 제약 조건에 매핑하여 계산된다.<br><br>
<p align="center"><img src="/MyPDF/multi_vo(7).png" width = "500" ></p>

time $t$에 실현 가능한 가속도(**f**easible **a**cclerations), $FA(t)$는 다음과 같이 정의된다.

$$ FA(t)=\{ \ddot{x} \mid \ddot{x}=f(x,\dot{x},u), \ u \in U \} $$
 -->


# Minkowski Sum
기하학에서, 유클리드 공간의 위치벡터 A와 B의 두 집합의 **민코프스키 합**은<br><br>
A에 있는 모든 벡터를 B에 있는 각각의 벡터에 더해서 만들어진다.<br><br>

$$ A \oplus B=\{ a+b \mid a \cap A, \ b \cap B \} $$


# Reference
- Kruse, T., Pandey, A.K., Alami, R., & Kirsch, A. (2013). **Human-aware robot navigation: A survey.** Robotics Auton. Syst., 61, 1726-1743.<br>
- Fiorini, P., & Shiller, Z. (1998). **Motion Planning in Dynamic Environments Using Velocity Obstacles.** The International Journal of Robotics Research, 17(7), 760–772.<br>
- Claes, Daniel & Tuyls, Karl. (2018). **Multi robot collision avoidance in a shared workspace.** Autonomous Robots. 42. 10.1007/s10514-018-9726-5.<br>
- Hennes, Daniel & Claes, Daniel & Meeussen, Wim & Tuyls, Karl. (2012). **Multi-robot collision avoidance with localization uncertainty.** 2.<br>
- Ran Zhao, Daniel Sidobre. **Dynamic Obstacle Avoidance using Online Trajectory Time-Scaling and Local Replanning.** Informatics in Control, Automation and Robotics (ICINCO), Jul 2015, Colmar, France. pp.414-421. ⟨hal-01764018⟩<br>