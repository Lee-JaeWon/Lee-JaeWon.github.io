---
title : "Turtlebot3를 이용한 Velocity Obstacle 구현"
category :
    - Multi_Robot
tag :
    - Multi_Robot
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---
2022-2 프로젝트로 진행한 Implementation of VO에 대한 상세 설명 및 고찰에 관한 포스팅입니다.

# 개요
한 대의 로봇을 통해 SLAM을 하거나 탐사를 진행할 때 Multi Robot을 사용하면 더 강인한 시스템을 만들 수 있을 것이라는 기대를 가지고 있다.<br><br>
이를 위해서는 기본적으로 Localization과 충돌 회피(Collision Avoidance)가 필수적이라 판단하였고,<br>
이에 2022-2 개인 프로젝트로 VO, Velocity Obstacle이라는 개념을 구현하여 충돌 회피를 진행하고 하였다.<br>
(Velocity Obstacle에 관한 내용은 [이 포스팅](https://lee-jaewon.github.io/multi_robot/Motion_Planning_VO/)에 정리되어있습니다.)<br><br>

# 구현 계획
## 보통 구현되는 내용
보통 Multi Robot을 이용한 Collision Avoidance와 Localization, Path Planning 등에는 다음과 같은 여러 가지 기술이 고려된다.<br>
- Localization
- Mapping
- Map Merging
- Exploration
- Collision Avoidance
- Dynamic Object detection

특히, 여러 대가 한 환경에 공존하기 때문에 LiDAR 센서를 통해 서로가 인식될 수 있으며, 이를 고려하지 않으면 서로를 Staic Object로 인식하고 Map에 표시하게 된다.<br>
이때, 잘못된 Localization 및 Path Planning을 유발한다.<br><br>

## 구현한 내용
본 프로젝트에서 구현한 내용은 다음과 같다.<br>
- Localization with AMCL. - 적용
- Mapping with one robot(Cartographer). - 적용
- Collision Avoidance with VO - 구현
- Path Planning with A Star. - 구현

# 구현 방식
참고한 VO 알고리즘 및 코드는 Holonomic 모델을 기준으로 제작되었다.<br><br>
각각 로봇의 위치와 속도를 통해 로봇이 부딪히지 않을 2차원 속도 벡터(x, y)를 제공한다.<br>
하지만 사용하는 로봇인 Turtlebot3는 Non-Holonomic 모델이며 단순한 속도 벡터로 움직이지 않고, 선속도와 각속도를 입력 명령을 받아 동작한다.<br><br>
이를 위해 생성된 Path를 일정 길이를 기준으로 Way point로 나누고, 그 Way point를 향한 벡터와 현재 로봇의 포즈를 일치시키고 향하는 방식으로 구현하였다.<br><br>
<p align="center"><img src="/MyPDF/IMVO(1).png" width = "500" ></p>
이때 향해야하는 벡터와 로봇의 포즈를 일치시키는 과정은 가중치 개념을 통해 구현하였다.<br><br>

# 구현 환경
- ROS1 Noetic(Ubuntu 20.04) - PC, Three Turtlebot
- Centralized System - 중앙 관리
- Turtlebot3 Burger model
- Python

# 구현 결과
[![Video Label](https://img.youtube.com/vi/IEfeJPWc0WE/0.jpg)](https://youtu.be/IEfeJPWc0WE)<br>
구현 영상은 다음 Youtube에서 확인할 수 있다.<br><br>

# 한계 및 고찰
이 알고리즘에는 충분한 공간이 필요할 수 있다. 좁은 공간에서는 로봇이 벽에 부딪힐 수 있다.<br>
VO(velocity vector)의 결과가 Path Planning을 따르지 않을 수 있기 때문입니다.<br><br>
Path Planning 보다 충돌 회피가 우선이 되는 경우, 벽 근처에 다가갈 수 있고, 이때 로봇의 포즈를 다시 해결하려 할 때 벽에 충돌하였다.<br>
이에 대한 예상 해결 방법은 벽에 가까워지거나 너무 밀집한 경우는 선속도를 제거하고 제자리에서 회전하는 방식을 채택했어야 했다고 생각한다.<br><br>

동적 물체 인식도 LiDAR를 통해 이루어진다면 인간과의 상호 작용이 가능하지만 이것은 현재 알고리즘과는 다른 문제이다.<br><br>

VO 알고리즘은 고전적인 방법일 수도 있으며, ROS1에서는 Local Path Planning을 다른 방법으로 수행하는 것이 사실이다.

# GitHub
[GitHub](https://github.com/Lee-JaeWon/Multi-Robot-Collision-Avoidance-with-VO)에 코드가 업로드 되어있습니다.