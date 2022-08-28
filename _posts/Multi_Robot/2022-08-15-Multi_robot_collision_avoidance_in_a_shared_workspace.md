---
title : "'Multi robot collision avoidance in a shared workspace' 간략 리뷰"
category :
    - Multi_Robot
tag :
    - Multi_Robot
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---

'Multi robot collision avoidance in a shared workspace' 논문 리뷰 포스팅입니다.(작업 중)<br><br>

> Referece : Claes, Daniel & Tuyls, Karl. (2018). Multi robot collision avoidance in a shared workspace. Autonomous Robots. 42. 10.1007/s10514-018-9726-5.


# Multi robot collision avoidance in a shared workspace 간략 리뷰(작업 중)

## Collision avoidance in shared workspaces
여러대의 로봇이 사람이 있든 없든 같은 작업 공간에서 task를 수행할 때 일어나는 문제점들을 다룬다.<br><br>
기존에 있던 방식 Global, Local Path Planning과 같은 기존의 인간 인식 항법(human aware navigation) 접근 방식의 주요 초점은 인간이 있는 환경에서 로봇의 comfort하고 자연스러움 및 사회성이 있음을 보여준다.<br>
> (Challenges of Human-aware Navigation : **Comfort, Naturalness, Sociability**)

하지만 이것은 보통 group of humans에서 개인을 위한 하나의 로봇만을 수반한다.<br><br>
그러나 Multi Robot을 위한 접근 방식에서는 로봇들의 다른 분포, 많은 사람들과 함께 작업 공간을 공유하며 navigating하는 것을 목표로 하며, 이는 기존 Path Planning 방식과는 다른 주요 목표를 가지고 있다.<br><br> 
이외에도 이미 제안된 Multi-Robot Collision Avoidance를 위한 방법들이 있다.

## Velocity obstacles (VO)

### In Wikipedia,
[Velocity Obstacle](https://en.wikipedia.org/wiki/Velocity_obstacle)<br><br>
로봇 공학에서 Velocity Obstalce은 다른 로봇이 현재의 속도를 유지한다고 가정할 때,<br>
어떤 순간에 다른 로봇과 충돌하게 되는 로봇의 모든 속도의 집합이다.<br><br>
로봇이 velocity Obstacle안에 있는 속도를 선택하면 결국 두 로봇은 충돌하게 되며,<br>
velocity obstacle 외부에 있는 속도를 선택하면 충돌이 발생하지 않도록 보장된다.<br>

### In Paper,

*Velocity Obstacle (VO)*는 동적 물체를 다루기 위해 제안된 접근 방법이다.

VO는 동적 장애물의 관측된 속도를 유지한다는 점에서 결국 충돌을 일으킬 모든 속도들의 기하학적 표현이다.

동적 장애물의 속도의 변화를 해결하기위해, controller는 초당 여러번 실행되어야하며, 이는 문제의 조각 선형(piece-wise linear) 근사치를 산출한다.

속도 공간(veloctiy space)에서 각각의 VO에 대한 면적을 계산한다면, 충돌과 충돌에 자유로운 속도를 이끄는 면적을 찾을 수 있다.

**만약 선호되어지는 속도가 충돌을 유발한다면, 선호된 속도에 가깝지만 충돌에는 자유로운 속도를 찾고자 한다.**

새로운 속도를 계산하기 위한 몇 가지 접근법이 있다.

VO의 사전 정의는 평면 모션(Planar motion)을 가정하지만, 이 concept은 직접적인 방식으로 3D motion으로 확장될 수 있다.

아래 **Fig. 1 (a)**처럼 충돌 코스에서 두 개의 로봇이 있는 작업 공간을 가정해보자.

<p align="center"><img src="/MyPDF/multi_vo(1).png" width = "600" ></p>
<p align='center'>Fig. 1</p>

움직이는 물체(여기서는 로봇 $R_B$)의 위치와 속도가 로봇 $R_A$에 알려진 경우, 현재 속도에서 충돌을 일으켜 안전하지 않은 영역에서의 로봇의 속도 공간을 영역에 표시할 수 있다.<br><br>
