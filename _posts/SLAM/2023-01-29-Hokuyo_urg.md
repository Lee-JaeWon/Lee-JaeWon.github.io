---
title : "HOKUYO URG-04LX-UG01 : ROS Noetic에서 실행하기"
category :
    - SLAM
tag :
    - SLAM
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---
ROS noetic에서 HOKUYO 사의 URG-04LX-UG01 LiDAR 센서를 작동시키는 포스팅입니다.

# Intro
SLAM 공부를 하며, LiDAR도 살짝 다뤄보면서 구현해 보기 위해 주변에 남는 LiDAR sensor를 사용해 보려 했다.<br><br>
HOKUYO 사의 URG-04LX-UG01 LiDAR 센서를 사용해 보려 했는데,
<br><br>
ROS noetic에서 HOKUYO 사의 URG-04LX-UG01 LiDAR 센서를 작동시키는 문서가 잘 없는 것 같아, 여러 자료를 찾아 실행시켜보았다.<br><br>
[ROS wiki:hokuyo_node](http://wiki.ros.org/hokuyo_node)에는 noetic 관련 사항이 보이지 않았다.

# How to do?
라즈베리 파이 같은 mini PC 없이 노트북에 바로 USB를 꽂고, 할 수 있도록 하였다.<br>

1. URG node 설치
    ```
    sudo apt-get install ros-noetic-urg-node
    ```

2. USB 확인
    ```
    ls -l /dev/ttyACM0
    ```
    설치한 urg node를 확인하기 위해서, `/opt/ros/noetic/share/urg_node/launch`에 가보면 launch 파일이 존재한다.<br><br>
    여기서 통신 속도와 포트를 설정해 주는데, USB 포트를 확인해 보니 포트는 그대로였고,<br> 통신 속도는 공식 문서를 확인해서 115200 그대로 두었다.
    <p align="center"><img src="/MyPDF/hokuyo(1).png" width = "500" ></p>

    ```
    ls -l /dev/ttyACM0
    ```
    위 명령어 이후, `ccw-rw----`라 나타나면,

    ```
    sudo chmod a+rw /dev/ttyACM0
    ```
    명령어를 통해 `ccw-rw-rw-`로 바꾸어준다.

3. 실행
<br>
    이후,
    ```
    roslaunch urg_node urg_lidar.launch
    ```
    를 입력하면, `/scan` 토픽이 뿌려지며, 노드가 동작한다.
    <p align="center"><img src="/MyPDF/hokuyo(2).png" width = "800" ></p>

4. rviz
    ```
    rviz
    ```
    rviz를 실행한다.<br><br>
    초기에는 아무것도 없는 아래와 같은 모습이다.<br>
    <p align="center"><img src="/MyPDF/hokuyo(3).png" width = "800" ></p>

    <br>
    왼쪽 하단 `Add`를 클릭하고, `/scan`을 추가해 준다.
    <p align="center"><img src="/MyPDF/hokuyo(4).png" width = "500" ></p>

    <br>
    이후, `Fixed Frame`을 `map`에서 `laser`로 변경한다.(왼쪽 상단)
    <p align="center"><img src="/MyPDF/hokuyo(5).png" width = "800" ></p>

    <br>
    여기까지 수행하면 최종적으로 laser scan을 확인할 수 있다.
    <p align="center"><img src="/MyPDF/hokuyo(6).png" width = "800" ></p>

<br>
본 포스팅의 언어 및 개발 환경 : `Ubuntu 20.04 LTS`, `ROS noetic`<br>
포스팅에 대한 오류나 궁금한 점은 Comments를 작성해 주시면, 많은 도움이 됩니다.
{: .notice--info}