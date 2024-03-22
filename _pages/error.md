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
This page is a simple place to record various errors you encounter when building or coding in your environment.   
**please feel free to contribute.**

## Format

**Error**:<br>
**How to do**:<br>
**Reference**:<br> 

## Errors
### 1. COLMAP Build Error
**Error**:<br>
COLMAP 빌드 시 CUDA 관련 에러 발생<br>
**How to do**:
```
cmake .. -GNinja # 이후,
cmake .. -GNinja -DCMAKE_CUDA_ARCHITECTURES=75 # 75는 조절해야할 수도 있다.
```  
**Reference**:<br>
[https://github.com/colmap/colmap/issues/1805](https://github.com/colmap/colmap/issues/1805)<br>
[https://colmap.github.io/install.html](https://colmap.github.io/install.html)<br>

### 2. Your title
**Error**:<br>
text<br>
**How to do**:
```
code or your text
```  
**Reference**:<br>
text<br>
