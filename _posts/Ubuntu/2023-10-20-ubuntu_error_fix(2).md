---
title : "[Ubuntu 20.04 오류 해결] Windows 저장소가 RAID로 묶인 경우, 해결 방안"
category :
    - Ubuntu
tag :
    - Ubuntu
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---

Windows 저장소가 RAID로 묶인 경우, Ubuntu가 설치되지 않는 현상을 다루는 포스팅입니다. 

# 이전 Ubuntu 문제들
이전에 Ubuntu 문제 해결 관련 포스팅을 했었습니다.

[Ubuntu 문제 1 : Nouveau 비활성화](https://lee-jaewon.github.io/ubuntu/CUDA/)<br>
[Ubuntu 문제 1-1 : Kernel 버전 올리기](https://lee-jaewon.github.io/ubuntu/Kernel_ver/)<br>
[Ubuntu 문제 2 : ASUS GPU와의 이슈 및 display mode 변경](https://lee-jaewon.github.io/ubuntu/ubuntu_error_fix(1)/)

# 이번 문제
이번에 ROG Strix 시리즈에 2TB SSD 2개가 독립적으로 장착된 PC에 Ubuntu를 설치하고자 하였습니다.<br>

windows11과 듀얼 부팅하였고, 이전 방법처럼 늘 하던대로 윈도우에서 일정부분을 Unallocated하고

이 부분을 Ubuntu에서 ext4와 linux-swap으로 잡고 설치하려했습니다.

하지만, Ubuntu에서 `gparted`로 확인해보니

에러가 발생했습니다.

```
invalid argument during seek for read on /dev/nvme1n1
```

그리고 보이는 파티션들의 모습이 **Windows 운영체제도 보이지 않고**, 할당 & 미할당된 **파티션이 구분되어있지 않았습니다.**

무언가 파티션을 읽는 과정이 잘못되었다고 판단했고, 처음에는 용량이 너무 커서 발생한 문제라 생각했습니다.

# RAID Array 시스템
처음 알게 된 것인데, laptop 판매처에서 독립적인 2TB SSD 2개를 **RAID**라는 것을 이용해

**하나의 디스크처럼 작동**되도록 설정해놓았습니다.

이는 windows에서 하나의 디스크처럼 잘 작동하지만, Ubuntu에서 받아들이지 못해 **파티션들 이름이 Intel RAID Member**? 이런 식으로 되어있습니다.<br>
(당황해서 사진은 못찍었습니다.)

적당한 시간동안 찾아봤는데, 큰 해결책을 찾지 못했고

아직 windows를 건들지 않은 상태라 모두 **초기화**하기로 했습니다.

# 해결 방법 : RAID Array 해제
결국 해결 방법은 그냥 다 처음부터 제가 원하는 환경을 만들어내고,

설치하는 것이었습니다.

Windows가 이미 많이 세팅되었고, heavy하다면 **사용할 수 없는 방법**이라 생각합니다.

순서는 다음과 같습니다.

- BIOS에서 RAID 해제 (해제 순간 모든 데이터 삭제됨)
- 하나의 SSD에 Windows설치
- 다른 하나의 SSD에 Ubuntu 설치

를 수행했습니다.

RAID Array해제는 [ASUS 홈페이지](https://www.asus.com/kr/support/FAQ/1046579/#a2)를 참고했으며,

과정 자체는 간단합니다.

[ASUS 홈페이지](https://www.asus.com/kr/support/FAQ/1046579/#a2)에서 동일하게 수행해주시면 됩니다.

**필요한 자료를 백업 후 -> BIOS에서 설정 -> 윈도우 부팅 USB를 통해 새로 설치**

# 결론
이렇게 다시 2개의 SSD로 복구한다음 초기화 상태로 모두 설치해주었습니다.

# 다른 문제
이외에도 또 다른 문제가 있다면 공유해 주시거나 같이 해결해 보면 좋을 것 같습니다.

<br>

📣<br>
포스팅에서 오류나 궁금한 점은 Comments를 작성해 주시면, 많은 도움이 됩니다.💡
{: .notice--info}