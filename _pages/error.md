---
title: "Trouble Shooting"
permalink: /error/
layout: single
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---

# Trouble Shooting Page
환경 구축 및 코딩 시 발생하는 여러 Error를 **간단히** 기록해놓는 페이지입니다.  
This page is a simple page to record various errors you encounter when building your environment or coding.<br><br>
**please feel free to contribute.**<br>
(You can contribute using the comment box below, open an issue in the repository directly, or whatever works best for you.)

## Format

**Topic**:<br>
**How to do**:<br>
**Reference**:<br> 

## Errors
### 1. COLMAP Build Error
**Topic**:<br>
COLMAP 빌드 시 CUDA 관련 에러 발생<br>
**How to do**:
```
cmake .. -GNinja # 이후,
cmake .. -GNinja -DCMAKE_CUDA_ARCHITECTURES=75 # 75는 조절해야할 수도 있다.
```  
**Reference**:<br>
[https://github.com/colmap/colmap/issues/1805](https://github.com/colmap/colmap/issues/1805)<br>
[https://colmap.github.io/install.html](https://colmap.github.io/install.html)<br>

### 2. Macbook & Ubuntu 연결
**Topic**:<br>
외부 컴퓨터에서 Ubuntu 접속<br>
**How to do**:<br>
[ssh 연결](https://jooky.tistory.com/2)<br>
[Vscode 연결](https://bosungtea9416.tistory.com/entry/VScode%EB%A1%9C-%EC%84%9C%EB%B2%84%EC%97%90-SSH-%EC%9B%90%EA%B2%A9-%EC%A0%91%EC%86%8D%ED%95%98%EA%B8%B0)

### 0. Your title
**Topic**:<br>
text<br>
**How to do**:
```
code or your text
```
**Reference**:<br>
text<br>
