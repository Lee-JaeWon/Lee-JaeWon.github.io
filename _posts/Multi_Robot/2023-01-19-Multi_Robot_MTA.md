---
title : "Multi Robot Multi Task Allocation for Hospital Logistics"
category :
    - Multi_Robot
tag :
    - Multi_Robot
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---
병원에서의 Multi Robot Multi Task Allocation에 관한 논문 및 문제 상황에 관한 포스팅입니다.

# 개요
여러 자율주행 이동로봇이 물류를 옮기는 상황에서 효율성을 높이기 위해 한 번에 하나의 패키지만 배송하는 대신 둘 이상의 작업을 할당할 수 있도록 하는 것이 Multi Task Allocation(MTA)이다.<br><br>
병원이라는 상황을 구체적으로 특정한 [논문[1] - Multi-robot multi-task allocation for hospital logistics](https://lee-jaewon.github.io/multi_robot/Multi_Robot_MTA/#reference)을 리뷰하였고, 이 논문에서는 병원에서 Multi Robot System과 MTA가 왜 필요한지 단일 작업 할당에 비해 얼마나 효율이 증가하는지를 보인다.<br><br>

## 문제 상황
먼저 병원에서 이동 로봇이 필요한 이유를 [논문[2] - Mobility Work: The Spatial Dimension of Collaboration at a Hospital](https://lee-jaewon.github.io/multi_robot/Multi_Robot_MTA/#reference)을 통해 알아본다.<br><br>
병원에는 병실, 회의실, 사무실 등 공간이 분산되어 있으며, 병원에 있는 모든 작업자는 도보로 많은 거리를 이동해야한다.<br>
침대, 테이블, 의료기기, 의료기록, 음식 등 많은 것들이 병원 내에서 끊임없이 이동한다.<br><br>
이를 위해 천장이나 벽 내부에 레일이나 물류를 지정된 곳으로 옮길 수 있도록 구성된 곳 또한 존재한다.<br><br>
[논문[2]](https://lee-jaewon.github.io/multi_robot/Multi_Robot_MTA/#reference)에서 제시하는 자료 중, 군로자들의 평균 도보 거리는 근무 시간 동안 10km에 이르고 운송 작업은 일주일에 최대 195시간 발생한다.<br><br>
하지만 추후 노동력의 감소는 어느 정도 예견할 수 있으며, 이를 대체하기 위해 모바일 로봇이 도입되거나 물류를 위한 시스템이 도입된다.<br><br>

# Task Allocation Algorithm
배송 작업은 픽업 위치와 하차 위치로 구성된다.<br><br>
각 위치를 일반적으로 Point-of-Interest, POI라 지칭한다. POI에서 배달 요청이 발생하면, 대기중인 적합한 로봇이 선택되어야한다.<br><br>
하지만 배달 요청이 지속적으로 발생한다면, 대기중인 로봇이 없는 상황이 발생할 수 있다.<br>
이 경우 사용 가능한 로봇이 있을 때까지 기다리는 것이 **Single Task Allocation(STA)**이고, 현재 작업 중에 있는 로봇을 선택하고 작업을 하나 더 추가하는 것이 **Multi Task Allocation(MTA)**이다.<br><br>

## Multi Task Allocation
MTA에서 이동 거리를 최소화하기 위해 선택한 로봇의 경로를 다시 계획해야한다.<br><br>
선택한 로봇은 현재 작업을 위해 향해야하는 지점과 추가된 새로 방문해야할 지점이 존재한다.<br>
최적의 경로를 찾기 위해 두 지점(POI)를 연결해야한다.<br><br>
최적을 다루기 위한 카테고리는 배터리, Capacity, 거리 등이 있지만, [본 논문 - 논문[1]](https://lee-jaewon.github.io/multi_robot/Multi_Robot_MTA/#reference)에서는 거리만을 고려하였다.<br><br>
MTA는 모든 로봇의 현재 위치에서 **새로 추가된** 픽업 위치까지의 경로를 계산하여 새로운 작업을 받을 로봇을 선정한다.<br><br>
여러 task가 존재하는 상황에서 가장 짧은 거리를 도출하기 위해 두 가지 계산을 수행하고, 이 중 더 짧은 거리를 선정한다.<br><br><br>
먼저, 현재 로봇은 현재 작업을 수행중이며 새로운 작업이 호출되기 전 상황은 다음과 같다.<br>
<p align="center"><img src="/MyPDF/MTA(1).png" width = "400" ></p>
새로운 작업 P2-D2가 호출되었다.(픽업위치2-배달위치2)<br>
<p align="center"><img src="/MyPDF/MTA(2).png" width = "400" ></p>
언급한 두 가지 계산 방법은 결국 [현재 위치로 부터 기존 작업을 지나 새로운 작업으로 가는게 가까운지] **or** [새로운 작업을 지나 기존 작업으로 가는 것이 더 가까운지]를 판단하는 것이다.<br>
<p align="center"><img src="/MyPDF/MTA(3).png" width = "600" ></p>

# Experiment
STA와 MTA의 알고리즘 성능 비교는 할당 시간, 픽업 대기 시간, 배송 시간으로 비교된다.<br><br>
<p align="center"><img src="/MyPDF/MTA(4).png" width = "600" ></p>
파란색 선은 STA, 빨간색 선은 MTA이다.<br><br>
STA는 이미 작업 중인 로봇이 많을 수록 로봇을 할당 받기 위한 대기 시간이 증가한다.(a)<br>
호출 후, 픽업을 위해 실제로 로봇이 도착할 때 까지 걸리는 시간 또한 STA는 증가하는 반면, MTA는 상대적으로 짧게 유지된다.<br><br>
<p align="center"><img src="/MyPDF/MTA(5).png" width = "600" ></p>
작업 수와 시간을 비교할 때, 마찬가지로 MTA가 104.67% 더 많이 작업을 완료했다.(본 논문에서 지정한 시뮬레이션 환경에서의 수치)<br><br>

# 본인의 고찰
먼저 다중 작업 할당이 좋은 것은 확인할 수 있었지만, 고려해야할 상황이 굉장히 많으며 병원이라는 공간 특성상 사람도 많고 오히려 굉장히 복잡해져 로봇의 도입을 꺼려할 수도 있다.<br><br>
실제 도입을 위해서라면, 실제 병원의 환경을 시뮬레이션으로 반영한 후 가장 이상적인 로봇의 대수와 시나리오를 책정해야 도입이 가능할 것이라 판단된다.<br><br>

# Reference
- [1] S. Jeon and J. Lee, "Multi-robot multi-task allocation for hospital logistics," 2016 18th International Conference on Advanced Communication Technology (ICACT), PyeongChang, Korea (South), 2016, pp. 1-1, doi: 10.1109/ICACT.2016.7423383.
- [2] Bardram, J.E., Bossen, C. Mobility Work: The Spatial Dimension of Collaboration at a Hospital. Comput Supported Coop Work 14, 131–160 (2005). https://doi.org/10.1007/s10606-005-0989-y

📣<br>
포스팅에 대한 오류나 궁금한 점은 Comments를 작성해주시면, 많은 도움이 됩니다.💡<br>
논문에서 발췌한 사진 자료나 내용의 저작권은 논문 저자에게 있습니다.
{: .notice--info}