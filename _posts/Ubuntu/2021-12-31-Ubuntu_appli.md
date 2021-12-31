---
title : "Ubuntu에서 Chrome 및 Slack 설치"
category :
    - Ubuntu
tag :
    - Ubuntu
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---

이 포스팅은 알리는 용도보다는 메모 정도로 적어놓고 필요할 때 보기 위한 포스팅이다.<br><br>

## Chrome
### Ubuntu 18.04.6에서는
Ubuntu 18.04에서는 Chrome **사이트에 들어가서** 단순히 Linux버전으로 설치해도 Windows에서 설치하는 것처럼 바로 설치가 된다.<br><br>
### Ubuntu 20.04.3에서는
하지만 20.04에서는 Chrome 사이트에서 설치하면 다운로드 후 support하지 않는다는 문구가 뜨며 설치가 되지 않아 다음과 같은 명령어를 통해 설치하였다.<br><br>
In Terminal,
```
$ wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

$ sudo apt install ./google-chrome-stable_current_amd64.deb
```

## Slack
Ubuntu에서는 카카오톡 같은 메신저가 되지 않아 Slack이라는 협업 도구를 사용중이다.<br><br>
### Ubuntu 18.04.6에서는
Slack 또한 우분투 18.04에서는 slack 사이트에 들어가서 snap store를 통해 아래와 같은 명령어 한 줄이면 설치가 바로 된다.
```
$ sudo snap install slack --classic
```
<p align="center"><img src="/MyPDF/slack.png" width = "800" ></p>
### Ubuntu 20.04.3에서는
우분투 20.04에서도 위와 같은 방법으로 설치가 되지만,<br><br>
무슨 이유에선지 한글 변환이 되지 않아 찾아봤더니 20.04에서 snap store를 통해 다운했을때의 문제인 듯 하여<br>
`.deb`파일을 다운로드 하고 아래와 같은 패키지 설치 명령어를 통해 Slack을 설치해주었다.<br>
다운로드 받은 경로에서 실행하는 것을 유의하자.<br>
```
$ sudo dpkg -i slack-desktop-4.23.0-amd64.deb
``` 
`dpkg`가 미리 설치되어 있어야한다.<br><br>
물론 이 방법또한 18.04에서 사용가능하다.<br><br>

📣<br>
본 포스팅의 언어 및 개발 환경 : `Ubuntu 18.04.6 LTS` or `Ubuntu 20.04.3 LTS`<br>
포스팅에 대한 오류나 궁금한 점은 Comments를 작성해주시면, 많은 도움이 됩니다.💡
{: .notice--info}