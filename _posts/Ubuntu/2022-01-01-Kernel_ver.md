---
title : "Ubuntu 18.04.6 LTS에서 Kernel version upgrade하기"
category :
    - Ubuntu
tag :
    - Ubuntu
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---

## 겪은 문제점
먼저 우분투 20.04가 아닌 18.04를 사용하고자함.<br><br>
Nvidia graphics driver나 CUDA등을 설치하려고 했는데 뭐가 문젠지 설치 이후 재부팅을 하기만 하면
검은 창에서 복구를 하거나 무한 부팅에 들어가는 문제 발생.<br><br>
추가로 와이파이 어댑터 인식 못하고(결국 [무선 랜카드](https://lee-jaewon.github.io/ubuntu/Ubuntu_set/) 사용중), HDMI를 통해 연결한 모니터도 인식 못하는 현상 발생.<br><br>
어찌됬든 이걸 해결하기 위해 우분투만 20번정도 깔며, 삽질을 했는데<br>
당장의 해결책은 다음과 같다.<br><br>
- 그래픽카드 드라이버 설치 전, [nouveau 비활성화 하기](https://lee-jaewon.github.io/ubuntu/CUDA/#nouveau-%EB%B9%84%ED%99%9C%EC%84%B1%ED%99%94).
- 18.04.6의 커널 버전을 높이기

## Linux Kernel 버전을 올리는 이유
RTX 3060 랩탑용을 사용중인데, 구글링을 해보니 낮은 커널 버전이나 낮은 그래픽 드라이버 버전이 RTX 그래픽카드 계열과 문제를 일으키는 것으로 어쨋든 생각하고 있다.<br><br>
Ubuntu 20.04.3 LTS에서는 추가 작업없이 우분투 설치 직후 모니터가 연결되었기 때문이다.<br>
(20.04의 Kernel 버전이 5.11대여서 더 높았음.)<br><br>
그래서 일단 **우분투 다시 깔아준 다음 설치는 최소한**으로 하고 커널 버전을 5.4대에서 5.11대로 올려주기로 했다.<br><br>
여기서 설치는 최소한으로 한다는 이유는 커널 버전에 따라 프로그램 설치되는게 다르다고 해야하나(?),<br> 설치에 커널 버전이 종속되는 경우가 있는 듯 해보였다.<br>(예상 이유 : 커널 업데이트 하고 와이파이 드라이버 설치했던게 안됐던 경우가 있어서.)

## Linux Kernel 버전 높이기
```
$ uname -a
```
혹은
```
$ uname -r
```
을 통해 커널 버전을 확인해준다.<br>
(굳이 업데이트할 필요가 있는지 먼저 생각해보자.)<br><br>
그리고, 아래 사이트에서 커널 업그레이드에 필요한 `*.deb`을 다운받아오자.<br>
[Ubuntu Kernel 5.11](https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.11/)<br><br>
<p align="center"><img src="/MyPDF/kernel.png" width = "800" ></p>

위 사진처럼 밑줄 친 파일 **총 4개**를 다운 받아준다.<br><br>
이 파일들은 Ubuntu내에 `/Downloads` 경로내에 잘 다운로드 되었을 것이다.<br>
파일 4개가 잘 있는 것을 확인한 후, 거기서 터미널을 켜준다.<br><br>
```
$ sudo su

$ ls -l
# `ls -l`을 통해 `*.deb`파일 4개가 뜨는 지 확인.
# (혹시 다른 *.deb 파일들이 있다면 다운로드 한 파일들만 딱 있게 파일을 분리하든지 하자.)

$ dpkg -i *.deb

$ reboot
```
마지막 reboot는 그냥 그 터미널 창에서 reboot하자.<br><br>
reboot후 다시 명령어를 통해 커널 버전을 확인해주면 된다.<br><br>

## Iptime A3000U 오류
18.04.6 LTS에서 [Iptime A3000U를 사용할 수 있도록하는 방법을 포스팅](https://lee-jaewon.github.io/ubuntu/Ubuntu_set/)했었는데,<br><br>
커널 버전 올려주니 또 먹통이 되어, [Iptime A3000U 설치 포스팅에서 커널 버전 올린 내용]()으로 다시 참고해주길 바란다.<br><br>

📣<br>
본 포스팅의 언어 및 개발 환경 : `Ubuntu 18.04.6 LTS` or `Ubuntu 20.04.3 LTS`<br>
포스팅에 대한 오류나 궁금한 점은 Comments를 작성해주시면, 많은 도움이 됩니다.💡
{: .notice--info}