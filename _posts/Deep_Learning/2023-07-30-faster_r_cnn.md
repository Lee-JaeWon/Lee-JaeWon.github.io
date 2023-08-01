---
title : "[논문 리뷰] Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks"
category :
    - Deep_Learning_Study
tag :
    - Deep_Learning
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---
[NIPS, 2015] Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks 논문 리뷰

# Abstract
SOTA object dection 네트워크는 물체 위치를 가정하기 위해 region proposal 알고리즘에 의존한다.

SPPnet과 Fast R-CNN과 같은 개발로 인해 이러한 탐지 네트워크의 실행 시간이 줄어들었지만, 영역 제안 계산에서 병목 현상이 드러났다.

본 연구에서는 detection 네트워크와 full-image convolutional feature를 공유하여 **cost가 거의 없는 region proposal을 가능**하게 하는 **Region Proposal Network(RPN)**을 소개한다.

RPN은 각 위치에서 물체 경계와 objectness score(객체가 있는지 없는지 여부에 대한 점수화)를 동시에 예측하는 Fully-connected network이다.

RPN은 높은 품질의 region proposal을 생성하도록 end-to-end로 훈련되며, 이는 Fast R-CNN 검출에 사용된다.

Simple alternating optimization을 사용하여 RPN과 Fast R-CNN은 convolutional feature들을 공유하도록 학습될 수 있다.

매우 깊은 VGG-16 모델의 경우, GPU에서 모든 단계를 포함하여 초당 5프레임의 프레임 속도를 달성하며, PASCAL VOC 2007 (73.2% mAP) 및 2012 (70.4% mAP)에서 이미지당 300개의 proposal을 사용하여 SOTA 물체 탐지 정확도를 달성한다.

# Introduction
최근의 물체 탐지 기술의 발전은 영역 제안 방법들과 영역 기반 컨볼루션 신경망(R-CNNs)의 성공으로 진행되고 있었다.

초기에 개발된 영역 기반 CNN은 계산 cost가 높지만, proposal 간의 컨볼루션 공유로 인해 비용이 크게 감소되었다.

최신 버전인 Fast R-CNN은 매우 깊은 네트워크(VGG Net)를 사용하여 거의 실시간 속도를 달성한다.

<p align="center"><img src="/MyPDF/frcnn(1).png" width = "500" ></p>

그러나 region proposal 단계가 최첨단 탐지 시스템에서 계산적 병목 현상을 겪고 있다.

Region proposal 방법들은 일반적으로 비용이 적게 드는 feature와 economical inference schemes(경제적 추론 기법?)에 의존한다.

가장 인기 있는 방법 중 하나인 selective search는 공학적으로 설계된 low-level feature를 기반으로 superpixel을 greedy하게 병합한다.

하지만 efficient detection network(Fast R-CNN)와 비교하면 selective search는 CPU 구현에서 이미지 당 2초로 느리다.

이후에 나온 EdgeBoxes를 활용하면 영역 추정 단계가 이미지당 0.2초가 걸린다.

그럼에도 불구하고 region proposal 단계는 여전히 detection network만큼 많은 실행 시간을 소비한다.

fast region-based CNN은 GPU를 활용하는 반면, 연구에서 사용되는 영역 제안 방법들은 CPU에서 구현되어서 이러한 실행 시간 비교는 불공정하다.(그래서 병목 발생?)

region proposal을 가속화하는 한 가지 명확한 방법은 GPU를 위해 다시 구현하는 것이다.

이것은 효율적인 공학적 solution이 될 수도 있지만 down-stream detection network를 무시하므로 계산을 공유할 수 있는 중요한 기회를 놓치게 된다.

본 논문에서는 알고리즘적인 변경으로 deep 네트워크를 사용하여 proposal을 계산하는 것이 elegant하고 effective한 해결책임을 보인다.

이를 위해 본 논문은 최첨단 물체 탐지 네트워크(K. He, X. Zhang, S. Ren, and J. Sun. **Spatial pyramid pooling in deep convolutional networks for visual recognition**. In ECCV. 2014. 와 R. Girshick. **Fast R-CNN.**)<br>
와 컨볼루션 레이어를 공유하는 새로운 Region Proposal Network (RPN)을 소개한다.

테스트 시간에 컨볼루션을 공유함으로써, Proposal을 계산하는데 드는 추가 비용은 매우 작다(예: 이미지 당 10ms).

본 논문의 관찰은 Fast R-CNN과 같은 영역 기반 탐지기에서 사용되는 컨볼루션(conv) feature 맵은 영역 제안을 생성하는 데에도 사용될 수 있다는 것이다.

이러한 conv feature 위에 본 논문은 **두 개의 추가적인 컨볼루션 레이어를 추가하여** RPN을 구성한다.

하나는 각 conv 맵 위치를 짧은 (예: 256차원) feature vector로 인코딩하고, 두 번째는 각 conv 맵 위치에서 해당 위치에서의 다양한 스케일과 가로 세로비에 대한 k개의 영역 제안에 대한 오브젝트 여부 점수와 회귀된 경계를 출력한다.<br>
(k = 9는 일반적인 값이다)

따라서 본 논문의 RPN은 FCN의 한 종류이며, detection proposal 생성을 위해 특별히 end-to-end로 훈련수 있다.

우리는 RPNs와 Fast R-CNN 물체 탐지 네트워크를 통합하기 위해 간단한 훈련 방식을 제안한다.

방식은 proposal 작업에 대해 fine-tuning을 하고, 그 다음 물체 탐지를 위해 fine-tuning하는 것을 번갈아 진행하면서 제안을 고정 상태로 유지한다.

이러한 방식은 빠르게 수렴하며, 두 작업 간에 컨볼루션 피처가 공유되는 통합된 네트워크를 생성한다.

(이후는 Selective Search의 거의 모든 계산적 부담을 덜었고 우수하며 좋은 성능을 냈다는 이야기, PASCAL VOC detection benchmark에서 평가했다.)

# Related Work
최근 몇 논문에서는 딥 네트워크를 사용하여 클래스별 또는 클래스에 상관없는 경계 상자를 찾는 방법들이 제안되고 있다.

OverFeat 방법 및 Multi-Box 방법등이 있다.

공유 컨볼루션 연산은 효율적이면서도 정확한 시각적 인식을 위해 점점 더 많은 관심을 받고 있다.

# Region Proposal Networks
Region Proposal Network (RPN)은 **임의의 크기의 이미지를 입력으로 받아** 각각의 물체에 대한 사각형 proposal과 그에 따른 물체 여부 점수를 출력한다.

이 과정은 fully-convolutional network로 모델링되며, 이번 섹션에서 설명된다.

궁극적인 목표는 Fast R-CNN 물체 탐지 네트워크와 계산을 공유하는 것이므로, 두 네트워크가 공통으로 사용하는 컨볼루션 레이어 집합을 가정한다.

본 논문의 실험에서는 Zeiler와 Fergus 모델(ZF)과 VGG를 조사한다.

region proposal을 생성하기 위해 마지막으로 공유된 컨볼루션 레이어로 출력된 컨볼루션 피처 맵 상에서 작은 네트워크를 슬라이드한다.(슬라이딩 윈도우 방식 적용)

이 네트워크는 입력 컨볼루션 피처 맵의 n x n 공간 윈도우와 완전히 연결된다.

각 슬라이딩 윈도우는 낮은 차원의 벡터로 매핑된다. (ZF는 256차원, VGG는 512차원)

이 벡터는 box-regression layer(reg)와 box-classification(cls)의 two sibling fully-connected lyaer로 공급된다.

이 논문에서는 n = 3을 사용하며, 입력 이미지에 대한 효과적인 수용 영역은 크다. (각각 ZF와 VGG에 대해 171픽셀과 228픽셀).

<p align="center"><img src="/MyPDF/frcnn(2).png" width = "500" ></p>

mini-network는 슬라이딩 윈도우 방식으로 작동하기 때문에 FC layer는 모든 공간 위치에 걸쳐 공유된다.

이 아키텍처는 자연스럽게 n x n 컨볼루션 레이어 뒤에 두 개의 동료 1 x 1 컨볼루션 레이어 (각각 reg와 cls용)로 구현된다. neural network의 출력에는 ReLU가 적용된다.

## Translation-Invariant Anchors
**각 슬라이딩 윈도우 위치마다, 동시에 k개의 region proposal을 예측한다.**

따라서 reg레이어는 k개의 상자 좌표를 인코딩하는 4k개의 출력을 가지게 된다.<br>
(박스의 4개 좌표)

cls레이어는 각 proposal에 대해 객체/비객체의 확률을 추정하는 2k의 점수를 출력한다.<br>
(물체인지 아닌지 판단하므로)

k개의 proposal은 **anchor**라고 불리는 k개의 reference box에 상대적으로 매개변수화 된다.

Anchor는 슬라이등 윈도우의 중앙에 위치한다.

본 논문은 3개의 scale과 3개의 ratio를 고려하여 총 9개의 anchor를 사용하였다.

특징 맵(conv feature map)의 크기가 W x H(일반적으로 약 2,400)인 경우, 총 WHk개의 앵커가 있다.

본 논문의 접근 방식의 중요한 특성 중 하나는 앵커와<br>
앵커를 기반으로 proposal을 계산하는 함수 모두에 대해 **이동 불변성(translation invariant)**을 갖는다는 것이다.

비교적, MultiBox 방법은 k-means를 사용하여 800개의 앵커를 생성하는데, 이 앵커들은 이동 불변성을 갖지 않는다.

이미지에서 객체를 이동시키면 proposal도 이동해야 하며, 동일한 함수가 어느 위치에서든지 proposal을 예측할 수 있어야한다.

MultiBox 앵커들은 이동 불변성을 갖지 않기 때문에 (4+1) x 800 차원의 출력 레이어가 필요하다. 반면 본 논문의 방법은 (4+2) x 9 차원의 출력 레이어가 필요하다.

본 논문의 proposal 레이어는 매개변수의 수가 더 적기 때문에 작은 데이터셋인 PASCAL VOC와 같은 데이터셋에서 과적합 위험이 적다.

<p align="center"><img src="/MyPDF/frcnn(3).png" width = "700" ></p>

이 수많은 박스들이 Region Proposal들이고, 이제 학습을 해나아가는 것이다.

## A Loss Function for Learning Region Proposals
RPN을 훈련하기 위해, 각 Anchor에 binary class lable을 할당한다.
(객체 혹은 비객체 여부)<br>

두 가지 유형의 anchor에 positive label을 할당한다.
- ground-truth box와 가장 높은 Intersection-over-Union(IoU) 중첩을 갖는 앵커/앵커들
- 어떤 ground-truth 상자와의 IoU 중첩이 0.7보다 높은 앵커

단일 ground-truth 상자가 여러 앵커에 긍정적인 레이블을 할당할 수 있다.

모든 ground-truth 상자에 대해 IoU 비율이 0.3보다 작은 비긍정적인 앵커에 부정적인 레이블을 할당한다.

긍정적이지도 부정적이지도 않은 앵커들은 훈련 목적에 기여하지 않는다.

위의 정의에 따라, Fast R-CNN의 multi-task loss를 따르는 목적 함수를 최소화한다.

이미지에 대한 손실함수는 다음과 같이 정의된다.

<p align="center"><img src="/MyPDF/frcnn(4).png" width = "600" ></p>

먼저 $i$는 anchor 박스 인덱스를 뜻한다.

<p align="center"><img src="/MyPDF/frcnn(5).jpg" width = "600" ></p>

<p align="center"><img src="/MyPDF/frcnn(6).jpg" width = "600" ></p>

(다른 분의 포스팅을 참고해서 적어봤다. [출처:논문 리뷰 - Faster R-CNN 톺아보기](https://bkshin.tistory.com/entry/%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0-Faster-R-CNN-%ED%86%BA%EC%95%84%EB%B3%B4%EA%B8%B0))

<p align="center"><img src="/MyPDF/frcnn(7).png" width = "600" ></p>

bounding-box 회귀에 사용하는 파라미터는 위와 같다.

x,y,z,w는 각각 박스의 좌표와 너비, 높이를 뜻한다.

아무것도 안붙어있으면 예측 경계 box, a가 붙어있는 것은 anchor box, *가 붙어있는 것은 ground-truth이다.

이는 앵커 박스(anchor box)로부터 가까운 실제 상자(ground-truth box)로의 bounding-box 회귀로 생각할 수 있다.

본 논문의 방법은 이전의 피쳐 맵 기반 방법과는 다른 방식으로 bounding-box 회귀를 수행한다.

이전 방법들에서는 임의 크기의 영역에서 pooling한 피쳐에 대해 bounding-box 회귀가 수행되며, 회귀 가중치는 **모든 영역 크기에 대해 공유**된다.

반면에 본 논문의 정의에서는 회귀에 사용되는 feature가 feature map상에서 동일한 공간 크기(n x n)를 갖는다.<br>크기의 변화를 고려하기 위해 **k개의 bounding-box regressor가 학습**된다.

각 regressor는 하나의 스케일과 하나의 종횡비를 담당하며, **k개의 회귀기는 가중치를 공유하지 않는다.**

이렇게 하여, feature가 고정된 크기/스케일이더라도 다양한 크기의 박스를 예측하는 것이 여전히 가능하다.

## Optimization
RPN은 fully-convolutional network로 구현되며, 역전파와 SGD를 사용하여 end-to-end로 훈련된다.

이미지 중심 샘플링 전략을 따라 하나의 이미지에서 256개의 앵커를 무작위로 샘플링하여 미니 배치를 형성한다.<br>
(모든 앵커 박스마다 손실 함수를 적용해 최적화하면 negative label에 치우친 결과를 내기 때문이다.<br>
이미지 안에서 객체가 차지하는 공간 비율보다 배경이 차지하는 비율이 더 크다.<br>
[출처:논문 리뷰 - Faster R-CNN 톺아보기](https://bkshin.tistory.com/entry/%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0-Faster-R-CNN-%ED%86%BA%EC%95%84%EB%B3%B4%EA%B8%B0))

positive 및 negative 앵커의 비율은 최대 1:1이 된다. 만약 이미지에 128개보다 적은 positive 샘플이 있다면, 미니 배치를 negative 샘플로 채운다.

새로운 레이어는 가우시안 분포에서 평균이 0이고 표준 편차가 0.01인 가중치로 무작위로 초기화된다.

나머지 다른 레이어는 ImageNet 분류를 위해 미리 훈련된 모델로 초기화된다.

ZF net의 모든 레이어와 VGG net의 conv3_1 이상의 레이어는 메모리를 보존하기 위해 튜닝한다.

## Sharing Convolutional Features for Region Proposal and Object Detection
이제 RPN에서 생성된 region proposal을 활용할 객체 검출 네트워크로 Fast R-CNN을 채택한다.

RPN과 Fast R-CNN은 독립적으로 학습되지만, 두 네트워크 사이에서 conv 레이어를 공유할 수 있는 기술을 개발하는 것이 목표였다.

두 개의 별도 네트워크를 학습하는 대신에, 두 네트워크 사이에서 conv 레이어를 공유하는 기술을 개발해야 한다.

두 네트워크를 모두 포함하는 하나의 네트워크를 정의하고 backpropagation을 사용하여 학습하는 것만으로는 간단하지 않다.

Fast R-CNN의 학습은 고정된 객체 제안에 의존하며, 동시에 proposal 메커니즘을 변경하면서 Fast R-CNN을 학습시키는 것이 명확하지 않다.

공동 최적화는 향후 연구에 대한 흥미로운 주제이지만, 본 논문은 실용적인 4단계 학습 알고리즘을 개발하였다.(Alternating training 채택 : 번갈아가며 학습)

**첫 번째 단계**에서는 앞서 설명한 대로 RPN을 학습한다.

이 네트워크는 ImageNet으로 사전 훈련된 모델로 초기화하고, region proposal 작업에 대해 end-to-end로 fine-tune한다.

**두 번째 단계**에서는 RPN 단계 1에서 생성된 proposal을 사용하여 Fast R-CNN에 의한 별도의 검출 네트워크를 학습한다.

이 검출 네트워크도 ImageNet으로 사전 훈련된 모델로 초기화된다.

이 시점에서 두 네트워크는 conv 레이어를 공유하지 않는다.

**세 번째 단계**에서는 다시 detection 네트워크를 사용하여 RPN 학습을 초기화한다.

그러나 공유된 conv 레이어는 고정하고 RPN에만 고유한 레이어를 fine-tune한다.

이제 두 네트워크는 conv 레이어를 공유한다. **마지막으로** 공유된 conv 레이어를 고정한 채로 Fast R-CNN의 fc 레이어를 fine-tune한다.

이렇게 함으로써 두 네트워크는 동일한 conv 레이어를 공유하고 통합된 네트워크를 형성한다.

- 결론 : RPN을 먼저 학습하고, 이것으로 Fast R-CNN을 학습하고, 이걸로 다시 RPN을 초기화하고, 마지막으로 다시 이를 활용해 Fast R-CNN의 FC layer를 fine-tune한다.<br>
이 과정을 통해 feature를 공유하면서도 독립적으로 번갈아가며 학습할 수 있다.

# Experiments
<p align="center"><img src="/MyPDF/frcnn(8).png" width = "600" ></p>

Ablation study 진행

<p align="center"><img src="/MyPDF/frcnn(9).png" width = "600" ></p>
<p align="center"><img src="/MyPDF/frcnn(10).png" width = "600" ></p>
<p align="center"><img src="/MyPDF/frcnn(11).png" width = "600" ></p>
<p align="center"><img src="/MyPDF/frcnn(12).png" width = "600" ></p>

# Conclusion
효율적이고 정확한 영역 제안 생성을 위해 지역 제안 네트워크(Region Proposal Networks, RPNs)를 제안

검출 네트워크와 컨볼루션 특징을 공유함으로써 region proposal 단계는 거의 비용이 들지 않았다.

본 논문의 방법은 통합된 딥러닝 기반 객체 감지 시스템이 초당 5-17프레임으로 동작할 수 있게 했다.

학습된 RPN은 영역 제안의 품질을 향상시키고 이로 인해 전체 객체 감지 정확도도 향상되었다.