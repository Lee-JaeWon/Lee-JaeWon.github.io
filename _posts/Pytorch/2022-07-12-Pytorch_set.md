---
title : "Pytorch 환경 구축 및 Test (Windows, GPU)"
category :
    - Pytorch
tag :
    - Pytorch
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---

Pytorch를 위한 Anaconda 환경 설정 및 환경 구축 후 테스트하는 포스팅입니다.
<br><br>

# Pytorch Anaconda 환경 설정 for windows
## Anaconda 가상 환경 생성(Python 3.7)
먼저 Anaconda prompt에서 Python 3.7 버전의 가상환경을 생성해주겠다.<br>(Anaconda는 이미 설치되어있다고 가정한다.)
```
conda create -n Pytorch python=3.7
```
위 명령어는 `conda create -n 가상환경 이름 python=생성할 가상 환경의 파이썬 버전 명시`이다.<br><br>
파이썬 버전은 명시해 주지 않아도 되지만, 어차피 파이썬이 필요한 경우라면 가상 환경 생성 후에 설치해 주어야 하기 때문에 명시해 준다.<br><br>
파이썬 버전은 사용하고자 하는 환경에 따라 적절히 선택해 주어야 할 필요성이 있다.<br><br>
생성한 가상환경을 활성화하는 방법은 다음과 같다.<br>
활성화해주면 생성한 가상환경 내에서 수행할 수 있게 된다.
```
activate Pytorch
```

## CUDA toolkit 및 cuDNN 설치
본인은 Pytorch에서 GPU를 사용하고자 한다.<br><br>
[Pytorch Install](https://pytorch.org/get-started/locally/)에서 확인해보면 windows에서 GPU 플랫폼을 설치하려면 CUDA 라는 것의 버전을 선택하게 한다.<br>
<p align="center"><img src="/MyPDF/pytorch(1).png" width = "700" ></p>
여기서 적절한 CUDA 버전과 cuDNN을 **생성한 Anaconda 가상 환경**에다가 설치해줄 것이다.<br><br>
이전에 CUDA가 필요한 환경을 세팅할적에, 다른 블로그와 자료들을 참고하여 CUDA와 cuDNN을 가상 환경이 아닌 PC(base)에 직접 설치하려 했는데<br><br>
이때 가상환경에서 CUDA가 적용이 안되고, GPU 인식을 못하는 문제가 발생했었다.<br><br>
그래서 가상환경에 전부 세팅하고자 한다.<br><br>
본인은 생성한 가상 환경에 **CUDA 11.3**과 **cuDNN 8.2.0**을 설치하도록 하겠다.<br>
(본인의 GPU나 컴퓨터 환경에 따라 버전을 달리 해야할 수도 있다.)<br><br>

- [Anaconda CUDA Toolkit](https://anaconda.org/nvidia/cuda-toolkit)에서 11.3.1을 설치한다. (activate 한 상태에서)<br>
(다운로드 때문에 시간이 다소 소요된다.)<br><br>
```
conda install -c nvidia/label/cuda-11.3.1 cuda-toolkit
```
<p align="center"><img src="/MyPDF/pytorch(2).png" width = "500" ></p>
설치가 완료되면 Anaconda Navigator에서 cuda관련 installed를 확인할 수도 있다.

- **cuDNN**을 설치해준다. (activate 한 상태에서)<br><br>
```
conda install -c anaconda cudnn
```
위 명령어는 CUDA를 먼저 설치했을 때, 어느 정도 상호호환이 되는 cuDNN을 설치해준다.<br><br>
위 명령어를 통해 cuDNN을 설치하니 8.2.1 버전이 설치되었다.<br><br>

## Pytorch GPU 설치
[Pytorch Install](https://pytorch.org/get-started/locally/)에서<br>
<p align="center"><img src="/MyPDF/pytorch(1).png" width = "700" ></p>
위 사진처럼 CUDA버전을 설정해주고 본인의 환경을 잘 설정해주면 설치할 수 있는 명령어를 제공한다.
```
conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch
```
마찬가지로 생성한 가상환경을 활성화해주고, 설치를 진행한다.<br><br>

## Pytorch Test
Pytorch가 가상 환경에 잘 설치되었는지, GPU 환경이 잘 구축되었는지 확인해보겠다.<br><br>
취향 상 본인은 Jupyter Notebook을 이용하겠다.<br><br>
<p align="center"><img src="/MyPDF/pytorch(4).png" width = "500" ></p>
위 사진과 같은 명령어를 통해 GPU 사용 가능 상태와 정보를 얻을 수 있다.<br>
```python
import torch

print(torch.cuda.is_available())
print(torch.cuda.get_device_name(0))
print(torch.cuda.device_count())
```


📣<br>
본 포스팅의 언어 및 개발 환경 : `Windows10`, `Python 3.7`, `Pytorch`, `GPU`, `Anaconda`<br>
포스팅에 대한 오류나 궁금한 점은 Comments를 작성해주시면, 많은 도움이 됩니다.💡
{: .notice--info}