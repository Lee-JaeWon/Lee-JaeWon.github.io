---
title : "Loop Closure Detetion with DBoW2"
category :
    - SLAM
tag :
    - SLAM
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---
Loop Detection에 대한 개념과, Kitti Dataset에서 DBoW2라이브러리를 이용해 Loop Detection을 진행하는 포스팅입니다.

<br>

**++ 처음 공부할 당시 적은 포스팅이라, 내용에 오류가 많습니다. 지적해주신 분들에게 감사드립니다.**<br>
**틀린 부분은 찾아서 보완할 수 있도록 하겠습니다.**

<br><br>
<p align="center"><img src="/MyPDF/week_5_2.jpg" width = "800" ></p>

# Basic Structure of SLAM
<p align="center"><img src="/MyPDF/week_5_3.jpg" width = "800" ></p>

# Loop Closing
<p align="center"><img src="/MyPDF/week_5_4.jpg" width = "800" ></p>

# Bag of Words
<p align="center"><img src="/MyPDF/week_5_5.jpg" width = "800" ></p>
<p align="center"><img src="/MyPDF/week_5_6.jpg" width = "800" ></p>

# Implementation of Loop Detection
[DBoW2](https://github.com/dorian3d/DBoW2) 라이브러리와 [mez's Monocular Visual Odometry with FAST](https://github.com/mez/monocular-visual-odometry) 코드를 수정하고 합하여 Loop Detection을 수행할 수 있도록 하였습니다.<br><br>
결합한 코드와 실행 방법은 [Lee-JaeWon's GitHub/Loop-Closure-Detection-with-DBoW2](https://github.com/Lee-JaeWon/Loop-Closure-Detection-with-DBoW2)에서 확인할 수 있습니다.<br><br>
<p align="center"><img src="/MyPDF/week_5_7.jpg" width = "800" ></p>
<p align="center"><img src="/MyPDF/week_5_8.jpg" width = "800" ></p>
<p align="center"><img src="/MyPDF/week_5_9.jpg" width = "800" ></p>
<p align="center"><img src="/MyPDF/week_5_10.jpg" width = "800" ></p>
<p align="center"><img src="/MyPDF/week_5_11.jpg" width = "800" ></p>
<p align="center"><img src="/MyPDF/week_5_12.jpg" width = "800" ></p>
<p align="center"><img src="/MyPDF/week_5_13.jpg" width = "800" ></p>
<p align="center"><img src="/MyPDF/week_5_14.jpg" width = "800" ></p>
<p align="center"><img src="/MyPDF/week_5_15.jpg" width = "800" ></p>
<p align="center"><img src="/MyPDF/week_5_16.jpg" width = "800" ></p>
<p align="center"><img src="/MyPDF/week_5_17.jpg" width = "800" ></p>
<p align="center"><img src="/MyPDF/week_5_18.jpg" width = "800" ></p>


<br><br>

📣<br>
본 포스팅의 언어 및 개발 환경 : `Ubuntu 18.04.6 LTS`, `OpenCV 3.2.0`<br>
포스팅에 대한 오류나 궁금한 점은 Comments를 작성해주시면, 많은 도움이 됩니다.💡
{: .notice--info}