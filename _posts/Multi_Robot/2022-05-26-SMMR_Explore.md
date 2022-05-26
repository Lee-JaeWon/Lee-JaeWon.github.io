---
title : "'SMMR-Explore: SubMap-based Multi-Robot Exploration System with Multi-robot Multi-target Potential Field Exploration Method' Paper Review"
category :
    - Multi_Robot
tag :
    - Multi_Robot
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---
'SMMR-Explore: SubMap-based Multi-Robot Exploration System with Multi-robot Multi-target Potential Field Exploration Method' 논문 리뷰 포스팅입니다.(작업 중)

## 0. Reference
J. Yu et al., "SMMR-Explore: SubMap-based Multi-Robot Exploration System with Multi-robot Multi-target Potential Field Exploration Method," 2021 IEEE International Conference on Robotics and Automation (ICRA), 2021, pp. 8779-8785, doi: 10.1109/ICRA48506.2021.9561328.

## 1. Introduction
미지 영역을 탐사하는 것은 autonomous robot system에서 근본적인(가장 중요한) 일이다.<br>
전형적인 탐사는 두가지 파트로 이루어져 있는데 이는 인식&위치(perception & location)와 결정(decision)이다.<br><br>
**Perception & Location**의 경우, DSLAM(Distributed SLAM)이 서로 다른 로봇이 동일한 장소를 경험할 때 로봇 간의 상대적 자세를 제공한다.<br><br>
**Decision**의 경우, 로봇은 같은 장소를 경험하기 전 개별적으로 환경을 탐색하고, 상대적인 자세가 제공되면 협력적으로 탐색을 진행한다.<br>