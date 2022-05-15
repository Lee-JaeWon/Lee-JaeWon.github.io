---
title : "'Coordinated Multi-Robot Exploration' Paper Review"
category :
    - Multi_Robot
tag :
    - Multi_Robot
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---

'Coordinated Multi-Robot Exploration' 논문 리뷰 포스팅입니다.(작업 중)

공부하며 정리하는 포스팅이기 때문에 다소 직역과 번역에 가까워 질 수 있습니다.

()에 담는 내용은 본인의 부연 설명입니다.
## 1. Introduction

## 2. Coordinating a Team Of Robots During Exploration
### A. Costs
### B. Computing Utilities of Frontier Cells
<br>**Frontier cell들의 효용성(Utility)을 추정하는 것은 굉장히 어렵다.**<br>
(Utility? -> 쓸모있는 것, 효용성, 로봇이 쓰기 좋은 정보?)<br><br>
실제로 특정 위치로 이동해 수집할 수 있는 실제 정보는 해당 지역의 구조에 의존하기에 예측이 불가능하다.<br>
하지만 이미 특정 frontier cell로 이동하는 로봇이 있는 경우, 다른 로봇한테는 그 cell의 효용성이 낮아질거라 예상할 수 있을 것이다.<br>
(이미 로봇이 간 cell로 갈 이유가 없기에 다른 로봇한테는 효용성이 떨어진다.)<br><br>
그러나 지정된 목표 위치로만 효용성이 줄어드는 것은 아니다.<br><br>
로봇의 센서는 일반적으로 로봇이 도착하자마자 특정 영역을 커버하기 때문에 로봇의 목표 지점 근처에 있는 Frontier cell의 기대 효용성(Expected Utility)또한 줄어든다.<br>
(로봇의 센서가 목표 지점만 비추는 것이 아니라 그 영역 부근 전체를 인식하기에 목표 지점 근처 cell들의 활용도가 떨어짐. 목표 지점이 있는데 다른 지점으로 갈 이유가 없기 때문에.)<br><br>
2-B. 에서는 다른 로봇에 할당된 cell들의 거리(distance)와 가시성(visibility)을 기반으로 frontier cell의 Expected Utility를 추정하는 기술을 제시한다.<br><br>
처음에 각 frontier cell $t$가 환경에서 특정 위치의 유용성에 대한 추가 정보가 없을 때, 모든 frontier cell에 대해 동일한 Utility $U_t$를 가진다고 가정하자.<br><br>
어떤 로봇의 target point $t'$이 설정되었을 때, 우리는 $t'$주위의 거리 $d$만큼 인접한 frontier cell들의 utility를 감소시킬 수 있다.<br>이는 로봇의 센서가 거리 $d$이내의 cell들을 덮을 확률 $P(d)$ 에 따라 결정된다.<br><br>
또한, 지정된 $t'$부터 거리 $d$에 있는 어떤 cell $t$은 로봇이 $t'$ 도착했을 때, 확률 $P(d)$와 함께 덮일 것이다.<br><br>
따라서, 우리는 frontier cell $t_n$의 utility $U(t_{n}|t_{1},...,t_{n-1})$를 계산할 수 있다.<br>
이 $t_{1},...,t_{n-1}$은 이미 robot $1,...,n-1$에 할당되어 있다.<br><br>
식은 다음과 같다.<br>
$ U(t_{n}|t_{1},...,t_{n-1} = U_{t_{n}}) - \sum_{i=1}^{n-1} P(\Vert{t_{n}-t_{i}\Vert}) $

📣<br>
포스팅에 대한 오류나 궁금한 점은 Comments를 작성해주시면, 많은 도움이 됩니다.💡
{: .notice--info}