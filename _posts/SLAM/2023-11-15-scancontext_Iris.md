---
title : "[논문 리뷰] Scan Context & LiDAR Iris"
category :
    - SLAM
tag :
    - SLAM
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---
Scan Context & LiDAR Iris 논문을 간단하게 정리한 포스팅입니다.

[2018,IROS]Scan Context: Egocentric Spatial Descriptor for Place Recognition within 3D Point Cloud Map

[2020,IROS]LiDAR Iris for Loop-Closure Detection

> 논문을 읽으며 간단하게 정리한 포스팅이기에 맥락이 없을 수 있습니다.

> 다른 논문들에서 사용되어 읽어보게 된 Place Recognition, Loop-Closure Detection 논문입니다.

# Scan Context
Place descriptor for outdoor place recognition.

- ours differ from theirs in that we use a maximum height of points in each bin.
    
    <p align="center"><img src="/MyPDF/scancontext(1).png" width = "500" ></p>

    <p align="center"><img src="/MyPDF/scancontext(10).png" width = "600" ></p>
    
- 3d point cloud 상에서 **최대 높이**에 있는 점을 encoding
(이러한 점이 기존 방법처럼 bin안의 point개수를 알필요가 없다함)
    
    phi함수가 max height 함수, N_r(그림의 파랑), N_s(그림의 노랑) 행렬 사이즈만큼의 Intensity image가 나온다.
    
    <p align="center"><img src="/MyPDF/scancontext(2).png" width = "600" ></p>
    

  

- Not rotation-invariant(iris에서 언급)
    
    하지만 논문에서는 translation에 관한 내용이 있다.
    
    root shifting을 활용
    
- similarity score between scan context
    
    columnwise manner, 즉 column별로 비교한다.
    
    column별로 cosine distance를 계산
    
    <p align="center"><img src="/MyPDF/scancontext(3).png" width = "400" ></p>
    
    q,c는 두 scan context를 의미, j는 column index
    
    (column-wise comparison is particularly effective for dynamic objects)
    
    revisit하거나 viewpoint가 다른 경우 scan context가 달라진다.
    
    이를 해결하기 위해, all possible column-shifted scan context의 거리를 계산하고 minimum을 찾는다.
    
    <p align="center"><img src="/MyPDF/scancontext(4).png" width = "400" ></p>
    
    이러한 additional shift가 ICP와 같은 localization refinement의 좋은 initial value를 제공할 수 있다.
    

- Two-Phase Search Algorithm
    
    place recognition에서 Typical하게 사용하는 searching방법은 pairwise similarity scoring, nearest neighbor search, sparse optimizaiton이 있다.
    
    해당 논문은 합리적인 seaching time을 달성하기 위해 pairwise scoring과 nearest search를 hierarchical하게 융합한다.
    
    식(6)처럼 모든 shifted scan context를 통한 계산은 다른 global descriptor들보다 heavy하다.
    
    Ring key(rotation-invariant descriptor)를 도입함을 통해 Two-Phase hierarchical Search Algorithm을 제공
    
    <p align="center"><img src="/MyPDF/scancontext(5).png" width = "400" ></p>
    
    <p align="center"><img src="/MyPDF/scancontext(6).png" width = "400" ></p>
    
    (9)는 Ring key의 encoding function, L0 Norm을 사용하며, L0 Norm은 개수. Ns개의 원에서 몇 개의 지역이 차있는지를 encoding
    
    <p align="center"><img src="/MyPDF/scancontext(7).png" width = "600" ></p>
    
    <p align="center"><img src="/MyPDF/scancontext(8).png" width = "500" ></p>
    
    <p align="center"><img src="/MyPDF/scancontext(9).png" width = "800" ></p>
    
    Ring Key를 통해 몇개의 top similar place만 추린다음, pariwise similarity score로 계산하여 loop detection을 수행.

# LiDAR-Iris
사람의 홍채(human’s iris)에서 아이디어를 얻음. point cloud를 iris image로 생성하여, image간 matching을 다루는 논문.

- **Scan context와 달리** 해당 지역의 3d point cloud를 **8bit code로 encoding(1~128)**
이를 통해 8by360 의 Iris image 생성
    
    <p align="center"><img src="/MyPDF/iris(1).png" width = "500" ></p>
    

- Fourier Transform을 통해 translation-invariant함을 달성.(between two-images)
    
    <p align="center"><img src="/MyPDF/iris(2).png" width = "500" ></p>
    
    point cloud의 회전이 image에서 translation에 해당
    
- For more feature extraction, exploit LoG-Gabor filters
    
    
- Loop-Closure detection은 Hamming distance 활용

정리하면, Fourier transform으로 image간 shift(x,y)를 줄여놓고(rotation-invarint),
hamming distance로 detection하는 것 같다.

# 참고 문헌
[Kim, Giseop, and Ayoung Kim. "Scan context: Egocentric spatial descriptor for place recognition within 3d point cloud map." 2018 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS). IEEE, 2018.](https://ieeexplore.ieee.org/abstract/document/8593953)

[Wang, Ying, et al. "Lidar iris for loop-closure detection." 2020 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS). IEEE, 2020.](https://ieeexplore.ieee.org/abstract/document/9341010)

📣<br>
포스팅에서 오류나 궁금한 점은 Comments를 작성해 주시면, 많은 도움이 됩니다.💡
{: .notice--info}