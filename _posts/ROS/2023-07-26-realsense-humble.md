---
title : "ROS2 Humble - Realsense D345 실행"
category :
    - ROS
tag :
    - ROS
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---
ROS2 Humble - Realsense D345 실행


# Realsense
ros2 humble을 설치해보고 Realsense 실물을 돌려보려고 포스팅을 작성했다.<br><br>

처음에 다른 블로그를 참고하다가 Realsense SDK를 설치하려하는데 오류가 나서<br>
[공식 GitHub:realsense-ros/tree/ros2-development](https://github.com/IntelRealSense/realsense-ros/tree/ros2-development)에서 보고 다시 설치했다.<br><br>

굉장히 간단하다. 깃허브에서 패키지 설치에 관해 안내하고 있었고,<br><br>

이걸 모두 수행해야하는줄 알았는데 option중에서 본인에게 편한 option을 따라하면 됬었다.<br><br>

<p align="center"><img src="/MyPDF/realsense(1).png" width = "500" ></p>

위 사진에서 Step2와 Step3을 수행하면 된다.<br><br>

# Realsense SDK 설치

<p align="center"><img src="/MyPDF/realsense(2).png" width = "500" ></p>

Step2에서 제일 쉬운 option은 두번째 옵션이다.<br><br>

SDK를 설치하기 위해 아래 명령만 수행하면 된다.<br><br>

이 명령을 수행하기 전에 패키지를 실행하면 SDK가 없다고 오류가나며 실행되지 않는다.<br><br>

```
sudo apt install ros-humble-librealsense2*
```

<br>

# Intel® RealSense™ ROS2 wrapper 설치
여기서도 option을 여러 개 제공하는데, 잘 모르고 그냥 ws에다가 clone받아서 `colcon build`를 수행했다.<br><br>

<p align="center"><img src="/MyPDF/realsense(3).png" width = "500" ></p>

다음과 같이 설치하는 명령어도 제공한다.<br><br>

# 실행
카메라를 PC와 연결 후,<br>
노드를 실행시켰다.<br><br>

깃허브에서 제공하는 명령어 중 rviz2에서 바로 확인되었던 명령어로 설명하겠다.<br>

```
ros2 launch realsense2_camera rs_launch.py depth_module.profile:=1280x720x30 pointcloud.enable:=true
```
`pointcloud.enable:=true` 옵션이 있어야 rviz2에서 보이는 것 같다.(확실하진 않음)<br><br>

```
rviz2
```
실행 후,<br>
rviz를 다음과 같이 세팅해주었다.<br><br>

<p align="center"><img src="/MyPDF/realsense(4).png" width = "700" ></p>

그러면 최종 결과로 아래와 같은 모습을 확인할 수 있다.<br><br>

<p align="center"><img src="/MyPDF/realsense(5).png" width = "700" ></p>

<br><br>

📣<br>
본 포스팅의 언어 및 개발 환경 : `Ubuntu 22.04`, `ROS2 humble`<br>
포스팅에 대한 오류나 궁금한 점은 Comments나 [메일](https://lee-jaewon.github.io/Aboutme/email)을 보내주시면, 많은 도움이 됩니다.💡
{: .notice--info}