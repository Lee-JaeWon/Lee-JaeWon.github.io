---
title : "Path Planning Algorithms"
category :
    - SLAM
tag :
    - SLAM
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---

Mobile-Robot SLAM에서의 Path Planning 기법들에 관한 포스팅입니다.<br>
앞으로 알아가게 될 기법들을 계속해서 추가해 나갈 예정입니다.

## Rapidly exploring Random Tree (RRT)
### 개요
RRT는 nonconvex의 고차원 공간에서 경로를 효율적으로 검색하도록 설계된 자료 구조 및 경로 계획 알고리즘이다.<br><br>
트리는 검색 공간(Search Space)에서 무작위로 추출한 샘플을 통해 점진적으로 구성되며, 검색되지 않은 공간으로 성장하도록 편향되어있다.<br><br>
Autonomous Robotic Motion Planning에 널리 사용되어왔다.<br><br>
**샘플링 기반 경로 계획법**으로 랜덤하게 샘플점을 생성하여 샘플점을 잇는 선이 장애물과 충돌하는지 여부를 파악하여 자유공간의 구조를 파악.<br><br>

### 대략적인 순서
- q_ini는 전체 트리의 root
- q_rand(무작위 샘플점)을 발생시킨 후, 가장 가까운 노드 q_near를 찾는다.
- q_near에서 q_rand 방향으로 연결한 직선상에 일정한 거리 만큼 떨어진 점을 q_new로 선정
- q_near에서 q_new까지의 직선으로 연결한 선상에 장애물이 없으면 q_new를 트리로 편입
<p align="center"><img src="/MyPDF/rrt.png" width = "500" ></p>

### 장점 및 단점
- 장애물이 있는 환경에서도 효율적으로 경로를 생성 및 계산량이 적다는 장점.<br>
(비선형 동적 프로그래밍이나 혼합-정수 선형 프로그래밍, 확률 로드맵에 비해)
- 샘플링 개수가 충분하지 않다면 존재하는 경로를 찾지 못할 수도 있는 단점 존재.
- 경로 탐색은 가능하지만 최적(Optimal)이지 않다는 단점.<br>(Cost를 고려하지 않았기 때문이다.)
- 도착점까지의 수렴에 많은 시간이 소요된다는 단점.
- 로봇의 운동학적 특성 고려x, 경로가 부드럽지 못함.

### ROS Package
ROS에서 테스트를 해 볼 예정이다.<br>
[ROS Package : rrt_exploration](http://wiki.ros.org/rrt_exploration)

📣<br>
포스팅에 대한 오류나 궁금한 점은 Comments를 작성해주시면, 많은 도움이 됩니다.💡
{: .notice--info}