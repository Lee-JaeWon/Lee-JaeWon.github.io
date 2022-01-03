---
title : "ROS melodic 설치(Ubuntu 18.04.6 LTS)"
category :
    - ROS
tag :
    - ROS
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---

## ROS Installation
[ROS Official Reference](http://wiki.ros.org/melodic/Installation/Ubuntu)<br>
**순서대로 진행해보자.**

### 설치 준비(패키지, 키 업데이트)
```
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```
```
# if you haven't already installed curl
$ sudo apt install curl
$ curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

### 설치
Debian 패키지 업데이트
```
sudo apt update
```
Install
```
sudo apt install ros-melodic-desktop-full

#설치 확인하고 싶으면
$ apt search ros-melodic
```

### 환경변수 설정
```
$ echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
$ source ~/.bashrc
```
### Dependencies 설치
```
$ sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
```
### Intialize rosdep
```
$ sudo apt install python-rosdep
```
```
$ sudo rosdep init
$ rosdep update
```
### roscore
터미널에
```
$ roscore
```
명령어를 통해 잘 작동하는지 확인하자.



📣<br>
본 포스팅의 언어 및 개발 환경 : `Ubuntu 18.04.6 LTS`, `ROS melodic`<br>
포스팅에 대한 오류나 궁금한 점은 Comments를 작성해주시면, 많은 도움이 됩니다.💡
{: .notice--info}