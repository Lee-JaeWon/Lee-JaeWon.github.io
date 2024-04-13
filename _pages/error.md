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
This page is a simple page to record **various errors or tips** you encounter when building your environment or coding.<br><br>
**please feel free to contribute.**<br>
- You can contribute using the [comment box below](https://lee-jaewon.github.io/error/#:~:text=LinkedIn-,LEAVE%20A%20COMMENT,-FOLLOW%3A), open an [issue in the repository](https://github.com/Lee-JaeWon/Lee-JaeWon.github.io/issues) directly, or whatever works best for you.<br>
- I always want this page to be enriching and helpful, and I'm always happy to be pointed out if I'm wrong.

## Format

**Topic**:<br>
**How to do**:<br>
**Reference**:<br> 

## Errors
### 1. COLMAP Build Error
**Topic**:<br>
COLMAP 빌드 시 CUDA 관련 에러 발생<br>
**How to do**:
```bash
cmake .. -GNinja # 이후,
cmake .. -GNinja -DCMAKE_CUDA_ARCHITECTURES=75 # 75는 조절해야할 수도 있다.
```  
**Reference**:<br>
[https://github.com/colmap/colmap/issues/1805](https://github.com/colmap/colmap/issues/1805)<br>
[https://colmap.github.io/install.html](https://colmap.github.io/install.html)<br>

---

### 2. Macbook & Ubuntu 연결
**Topic**:<br>
외부 컴퓨터에서 Ubuntu 접속<br>
**How to do**:<br>
[ssh 연결](https://jooky.tistory.com/2)<br>
[Vscode 연결](https://bosungtea9416.tistory.com/entry/VScode%EB%A1%9C-%EC%84%9C%EB%B2%84%EC%97%90-SSH-%EC%9B%90%EA%B2%A9-%EC%A0%91%EC%86%8D%ED%95%98%EA%B8%B0)

---

### 3. Python Import Error
**Topic**:<br>
root directory에 존재하는 파이썬 클래스나 파일들이 import되지 않는 경우.<br>
**How to do**:<br>
```bash
# In terminal
export PYTHONPATH=$PYTHONPATH:/path/to/Your_package
```

---

### 4. GPU Select
**Topic**:<br>
python script 실행 시 GPU 선택<br>
**How to do**:<br>
```bash
# In terminal
CUDA_VISIBLE_DEVICES=0,3 python3 your_train.py
```

---

### 5. libX11.so.6: cannot open shared object file: No such file or directory
**Topic**:<br>
Error: `libX11.so.6: cannot open shared object file: No such file or directory`<br>
**How to do**:<br>
```bash
# In terminal
apt-get update
apt-get install -y libsm6 libxext6 libxrender-dev
```

---

### 6. libGL.so.1, libgthread-2.0.so.0 Error
**Topic**:<br>
`OSError: libGL.so.1: cannot open shared object file: No such file or directory` &&<br>
`ImportError: libgthread-2.0.so.0: cannot open shared object file: No such file or directory`<br>
**How to do**:<br>
```bash
# In terminal
apt-get update
apt-get install -y libgl1-mesa-glx
apt-get install -y libglib2.0-0
```
**Reference**:<br>
[Link](https://yuevelyne.tistory.com/entry/OpenCV-ImportError-libGLso1-cannot-open-shared-object-file-No-such-file-or-directory)<br>

---

### 0. Your title
**Topic**:<br>
text<br>
**How to do**:
```
code or your text
```
**Reference**:<br>
text<br>

---
