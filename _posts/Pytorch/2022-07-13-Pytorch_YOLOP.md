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

Pytorch YOLOP (YOU ONLY LOOK ONCE FOR PANOPTIC DRIVING PERCEPTION)를 테스트해보는 포스팅입니다.(작업 중)
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
gi
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
`requirements.txt` 설치는 `git clone`해온 YOLOP 폴더안에서 진행하였다.<br><br>

결론적으로 총 순서는 다음과 같았다.
- 아나콘다 가상환경 생성 -> git clone YOLOP -> Requirements 설치

### 환경 Test


