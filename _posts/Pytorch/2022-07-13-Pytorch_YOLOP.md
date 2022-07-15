---
title : "[Pytorch Hub] YOLOP Test"
category :
    - Pytorch
tag :
    - Pytorch
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---

Pytorch **YOLOP** (YOU ONLY LOOK ONCE FOR PANOPTIC DRIVING PERCEPTION)를 테스트해보는 포스팅입니다.(작업 중)
<br><br>

# YOLOP
[Pytorch Hub : YOLOP](https://pytorch.org/hub/hustvl_yolop/)<br>
[GitHub : YOLOP](https://github.com/hustvl/YOLOP)<br>

> [**You Only Look at Once for Panoptic driving Perception**](https://arxiv.org/abs/2108.11250)
>
> by Dong Wu, Manwen Liao, Weitian Zhang, [Xinggang Wang](https://xinggangw.info/), [Xiang Bai](https://scholar.google.com/citations?user=UeltiQ4AAAAJ&hl=zh-CN), [Wenqing Cheng](http://eic.hust.edu.cn/professor/chengwenqing/), [Wenyu Liu](http://eic.hust.edu.cn/professor/liuwenyu/)      [*School of EIC, HUST*](http://eic.hust.edu.cn/English/Home.htm)
>
> corresponding author.
>
> *arXiv technical report ([arXiv 2108.11250](https://arxiv.org/abs/2108.11250))*

from [GitHub : YOLOP](https://github.com/hustvl/YOLOP)<br>

## 요구 사항
YOLOP를 실행하기 위한 요구 사항이 있다.<br><br>
먼저 python 3.7에서 개발되었고, Pytorch 1.7 이상, torchvision 0.8 이상이 요구된다.<br><br>
처음에 Python 버전 요구 사항을 맞추지 않았더니 다른 요구 패키지를 설치하지 못했었다.<br>주의해야하는 것으로 보인다.<br>

### Requirements
from [GitHub : YOLOP](https://github.com/hustvl/YOLOP)<br><br>
This codebase has been developed with python version 3.7, PyTorch 1.7+ and torchvision 0.8+:

```
conda install pytorch==1.7.0 torchvision==0.8.0 cudatoolkit=10.2 -c pytorch
```

See `requirements.txt` for additional dependencies and version requirements.

```
pip install -r requirements.txt
```
`requirements.txt` 설치는 `git clone`해온 **YOLOP 경로 안에서** 진행하였다.<br><br>

결론적으로 총 순서는 다음과 같았다.
- 아나콘다 가상환경 생성 -> Pytorch&CUDA 설치 -> git clone YOLOP -> Requirements 설치

## YOLOP Test
### Load From Pytorch Hub
[Pytorch Hub : YOLOP](https://pytorch.org/hub/hustvl_yolop/)에 나와있는 **Pytorch Hub로부터 모델을 불러오는 방법**이다.<br><br>
YOLOP를 위한 환경설정이 모두 된 후,  [Pytorch Hub document](https://pytorch.org/hub/hustvl_yolop/)에 나와있는대로 코드를 작성하여 실행해보면 모델을 불러올 수 있다.<br>

```python
model = torch.hub.load('hustvl/yolop', 'yolop', pretrained=True)

img = torch.randn(1,3,640,640)

det_out, da_seg_out, ll_seg_out = model(img)

print(model)
```
위 코드를 실행해보면 model 구조를 확인해 볼 수 있다.<br>
<p align="center"><img src="/MyPDF/yolop(1).png" width = "700" ></p>

### Demo test
실제 이미지를 가지고 YOLOP를 테스트해 볼 수 있는 2가지 방법을 제공한다.<br><br>
명령 프롬프트에서 다음과 같이 `.py`를 실행해보면 perception 및 segmentation이 진행된 결과 이미지를 확인할 수 있다.<br><br>

YOLOP 경로 안에서
```
python tools/demo.py --source inference/images
```
그러면 `inference/output`에 테스트 결과가 저장된다.<br><br>
<p align="center"><img src="/MyPDF/yolop(2).jpg" width = "500" ></p>
<p align="center"><img src="/MyPDF/yolop(3).jpg" width = "500" ></p>
위 두 사진은 이미 제공된 사진으로 테스트한 것이고,<br>
실제 다른 사진으로도 테스트해 볼 수 있다.<br><br>
(다만 이미지를 가져올 때 **사이즈가 1280*720을 만족하지 않으면**, demo.py가 작동하지 않는다.)<br>
<p align="center"><img src="/MyPDF/yolop(4).jpg" width = "500" ></p>
위 사진은 우리나라의 주행 사진을 가져와 테스트해 본 것이다.<br>

<br>

# About YOLOP
YOLOP (YOU ONLY LOOK ONCE FOR PANOPTIC DRIVING PERCEPTION)에 대해 간단하게 알아보자.<br><br>
