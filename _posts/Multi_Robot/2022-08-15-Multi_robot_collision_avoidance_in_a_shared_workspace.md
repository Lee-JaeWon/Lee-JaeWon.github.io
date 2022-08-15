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

'Multi robot collision avoidance in a shared workspace' 논문 리뷰 포스팅입니다.<br><br>

> Referece : Claes, Daniel & Tuyls, Karl. (2018). Multi robot collision avoidance in a shared workspace. Autonomous Robots. 42. 10.1007/s10514-018-9726-5.

<br>

# Multi robot collision avoidance in a shared workspace 간략 리뷰(작업 중)

## Velocity obstacles (VO)

*Velocity Obstacle (VO)*는 동적 물체를 다루기 위해 제안된 접근 방법이다.

VO는 동적 장애물의 관측된 속도를 유지한다는 점에서 결국 충돌을 일으킬 모든 속도들의 기하학적 표현이다.

동적 장애물의 속도의 변화를 해결하기위해, controller는 초당 여러번 실행되어야하며, 이는 문제의 조각 선형(piece-wise linear) 근사치를 산출한다.

속도 공간(veloctiy space)에서 각각의 VO에 대한 면적을 계산한다면, 충돌과 충돌에 자유로운 속도를 이끄는 면적을 찾을 수 있다.

만약 선호되어지는 속도가 충돌을 유발한다면, 선호된 속도에 가깝지만 충돌에는 자유로운 속도를 찾고자 한다.

새로운 속도를 계산하기 위한 몇 가지 접근법이 있다.

VO의 사전 정의는 평면 모션(Planar motion)을 가정하지만, 이 concept은 직접적인 방식으로 3D motion으로 확장될 수 있다.

아래 **Fig. 1 (a)**처럼 충돌 코스에서 두 개의 로봇의 작업 공간을 가정해보자.

<p align="center"><img src="/MyPDF/multi_vo(1).png" width = "600" ></p>
<p align='center'>Fig. 1</p>

움직이는 물체(여기서는 로봇 $R_B$)의 위치와 속도가 로봇 $R_A$에 알려진 경우,현재 속도에서 충돌을 일으켜 안전하지 않은 영역을 로봇의 속도 공간에 표시할 수 있다.<br><br>
