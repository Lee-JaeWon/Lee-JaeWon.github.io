---
title : "[Ubuntu 오류 해결] 외부 모니터 연결 안됨 & ubuntu 진입 안됨 & 버벅임 & Asus 이슈 -> Display mode 바꾸기"
category :
    - Ubuntu
tag :
    - Ubuntu
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---

Ubuntu에서 display연결 오류 및 원활히 동작하지 않는 것을 해결한 포스팅입니다.

포스팅 및 문제 해결에 도움을 주신 [Ilju Lee](https://github.com/leeilju)님께 감사드립니다!


[이전-포스팅--nouveau](#이전-포스팅--nouveau)<br>
[새로운-문제](#새로운-문제)<br>
[문제-해결](#문제-해결)<br>

<br>

# 이전 포스팅 : Nouveau
이전 [Nouveau 비활성화에 관련한 포스팅](https://lee-jaewon.github.io/ubuntu/CUDA/)을 올린적이 있습니다.

많은 분들이 참고해주고 계시고, 저 또한 아직도 Ubuntu 설치 후, [해당 방법](https://lee-jaewon.github.io/ubuntu/CUDA/)을 사용해서 충돌을 막습니다.

이때 포스팅 당시 생긴 문제가 nouveau와 Nvidia GPU와의 충돌이었고,

당시 언급했었던 발생한 문제점은 아래와 같았습니다.

> 보통 nouveau로 인해 생기는 문제는 크게 모니터가 켜지지 않거나, 모너티가 깜빡거리거나, 몇 번의 부팅 후 부팅이 되지 않는 현상등이 존재합니다.
듀얼모니터 혹은 여러 대의 모니터 사용 시에도 문제가 발생할 수 있습니다.

이때 nouveau 관련 사항을 blacklist에 추가해주고, 그래픽 드라이버를 설치하면 문제가 해결되었습니다.

<br>

# 새로운 문제
또 **ASUS사의 노트북**과 **Ubuntu**와 이슈가 발생했었습니다.

- ROG Strix, RTX 4070, i7-13650hx

노트북에서 20.04 사용 시 nouveau세팅 및 **그래픽 드라이버를 설치해도**

우분투에 **진입이 안되거나**, 모니터가 **계속 끊기는 현상**이 발생했습니다.

외부 디스플레이는 연결조차 되지 않았습니다.<br>
(22.04에서는 작동)

22.04는 kernel 버전이 6대이고, 20.04는 5.15 정도를 사용합니다.

높은 사양의 GPU가 문제일 것이라 생각했고,

20.04에서 kernel 버전을 높여 GPU와 맞게 사용하고자 했는데, 이 방법을 적용해도 Ubuntu에 진입이 안되는 현상이 발생했습니다.<br>
(Kernel 올리는 방법은 [해당 포스팅](https://lee-jaewon.github.io/ubuntu/Kernel_ver/)에 정리해놨습니다.)

<br>

# 문제 해결
이 문제를 해결한 것은 **ASUS BIOS내에서 Display mode 세팅을 변경**하여 해결했습니다.

설치 후, 그래픽 드라이버까지 설치가 되었을 때 해당 방법을 적용했습니다.(순서는 상관없을 것 같습니다.)

먼저 BIOS 진입 후, `Advanced`에서 `Display Mode`를 찾습니다.
<p align="center"><img src="/MyPDF/ubuntu_error(2).jpg" width = "600" ></p>


`Dynamic`을 `dGPU Only`로 수정해줍니다.
<p align="center"><img src="/MyPDF/ubuntu_error(1).jpg" width = "600" ></p>

한 가지 수정사항으로 문제가 해결되었습니다.

<br>

# 다른 문제
이외에도 또 다른 문제가 있다면 공유해주시거나 같이 해결해보면 좋을 것 같습니다.

<br>

📣<br>
포스팅에서 오류나 궁금한 점은 Comments를 작성해 주시면, 많은 도움이 됩니다.💡
{: .notice--info}