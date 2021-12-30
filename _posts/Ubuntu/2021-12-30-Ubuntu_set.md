---
title : "Ubuntu 18.04 LTS에서 Iptime A3000U 연결하기"
category :
    - Ubuntu
tag :
    - Ubuntu
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---

## Iptime A3000U
본인의 Ubuntu 18.04와 20.04에서 모두 와이파이를 잡지 못해 커널버전도 건드리고,  
다양한 시도를 해봤지만 당장 와이파이를 써야해 Iptime의 무선 랜카드 A3000U를 사용중이다.<br>
(RTX시리즈에서 Ubuntu와의 이슈가 있는듯한데 정확히 어디가 문제인지를 아직 못찾았다.)

(참고로 사용중인 랩탑의 사양은 Asus사 노트북이며, i7-11800H, RTX3060이다.)<br><br>

A3000U를 Ubuntu에서 사용하기 위해, 필요한 드라이버를 깔아줘야한다.

In Terminal,
```
sudo apt-get update

sudo apt-get install build-essential dkms git

sudo git clone https://github.com/cilynx/rtl88x2BU_WiFi_linux_v5.3.1_27678.20180430_COEX20180427-5959.git

cd rtl88x2BU_WiFi_linux_v5.3.1_27678.20180430_COEX20180427-5959

VER=$(sed -n 's/\PACKAGE_VERSION="\(.*\)"/\1/p' dkms.conf)

sudo rsync -rvhP ./ /usr/src/rtl88x2bu-${VER}

sudo dkms add -m rtl88x2bu -v ${VER}

sudo dkms build -m rtl88x2bu -v ${VER}

sudo dkms install -m rtl88x2bu -v ${VER}

sudo modprobe 88x2bu

#확인용
sudo apt install net-tools

ifconfig
```
`$ ifconfig`까지 진행하면 여러 정보가 나타나는데,<br>
아래와 같은 정보가 확인 되어야 한다.
```
wlx705dccff1daf: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether 70:5d:cc:ff:1d:af  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
<br><br>


📣<br>
본 포스팅의 언어 및 개발 환경 : `C++`, `Ubuntu 18.04.6 LTS`, `Ubuntu 20.04.3 LTS`  
포스팅에 대한 오류나 궁금한 점은 Comments를 작성해주시면, 많은 도움이 됩니다.💡
{: .notice--info}