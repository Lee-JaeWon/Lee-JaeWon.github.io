---
title : "Timer&Counter"
category :
    - ATmega128
tag :
    - ATmega128
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---
ATmega128의 Timer/Counter에 대해 알아보고 실제 코드를 살펴보는 포스팅입니다.<br><br>


# Timer/Counter
먼저 ATmega128에는 두 개의 8bit 타이머/카운터와 두 개의 16bit 타이머/카운터가 내장되어있다.<br><br>
이들은 시간 및 펄스 폭의 계측, 외부 사건을 계수, PWM펄스 발생, 주기적인 인터럽트 발생 등 다양한 용도로 사용된다.<br><br>
위의 다양한 용도가 있지만 가장 큰 의의는 정확한 시간을 측정하거나 펄스를 정확하게 계수하는 것의 응용이라 볼 수 있다.<br><br>
타이머/카운터는 내부 clock이나 외부의 크리스털을 소스로 사용하고, 사용자가 필요한 주기를 설정하여 일정한 주기를 발생시키는 임의의 작업을 할 수 있다.<br>
타이머/카운터는 **일반적으로 클럭이 입력되면 그 클럭을 계수(셈)하는 기능을 의미**하는데, 입력으로 들어오는 **펄스가 어디로부터 오는지에 따라** 타이머와 카운터로 나뉜다.<br>

## Timer mode
**타이머는 MCU 내부에서 발생하는 클럭을 계수하는 장치이다.**

## Counter mode
**카운터는 MCU의 외부에서 입력되는 클럭을 세는 장치이다.**

## Timer/Counter
이 둘을 통칭해서 부르기에 같은 것이라 생각하지만 실상은 엄연히 서로 다른 기능을 기술하고 있다.<br><br>
또한, 단순히 클럭만 세는것이 아니라 위에서 언급한 것처럼 시간 혹은 펄스와 관련된 기능 혹은 지연(delay)의 역할을 한다는 것을 상기하자.<br><br>
***그럼 이제 정확히 어떻게해야 시간도 잴 수 있고, 원하는 주기도 만들어 낼 수 있는지 살펴보자.***<br><br><br>

# 기본 용어
## 주기와 펄스, 주파수
**주기** : 파동에서는 파동이 1회 반복하는데 걸리는 시간, 펄스는 0과 1이 번갈아 한 번 나타나는 시간.<br><br>
**주파수** : 1초 동안 진동하는 횟수, 즉 주기의 역수. $f = {1}/{t}$

## Prescaler(분주기)
-> ***클럭 신호의 주기를 변경 혹은 입력 클릭의 속도를 조절.***<br><br>
Prescaler가 필요한 이유는 16MHz(16000000Hz)를 통해 만약 1초를 세고 싶다면 1초동안 16000000번의 클럭을 카운팅해야한다.<br><br>
이는 불필요한 낭비가 되며, 심지어 8bit나 16bit의 자료형에 들어가지도 않는다.<br><br>
그래서 prescaler라는 것을 이용한다. prescaler를 통해 1:8이라는 분주비를 설정하면 8번의 주기를 한 번이라고 셈 하는 것이다.<br>
(즉, 시스템 클럭이 8번 들어오면 프리스케일러를 통과한 클럭은 1클럭이 발생됨.)<br><br>
그러면 16MHz는 2MHz가 된다.<br>
<p align="center"><img src="/MyPDF/prescaler.png" width = "600" ></p>

## Duty Ratio
-> ***신호의 한 주기 내에 High(or ON) 신호가 차지하는 비율***
<p align="center"><img src="/MyPDF/Duty_Ratio.png" width = "500" ></p>

## Timer/Counter 용어
**bottom** : 8비트 혹은 16비트 타이머/카운터가 가질 수 있는 최솟값(0x00 or 0x0000)<br><br>
**max** : 8비트 혹은 16비트 타이머/카운터가 가질 수 있는 최대값(0xFF or 0xFFFF)<br><br>
**top** : 동작 모드에 따라 타이머/카운터가 도달할 수 있는 최대값으로 max 혹은 OCR 레지스터의 설정값<br><br>
**TCNT** : 타이머/카운터 레지스터 값. 클럭에서 입력 신호가 들어올 때마다 이 레지스터 값을 1 증가 혹은 감소 시킨다.<br><br>
**OCR** : 출력 비교 레지스터. 계속해서 TCNT와 비교되며 비교의 결과가 일치하게 되면 외부 핀인 OC단자에 신호가 출력되도록 설정할 수 있다.<br>
('OCR을 설정할 수 있다'라는 말이지 무조건 OCR을 설정하고 그 값을 지정해야하는 것은 아니다. 이 부분은 뒤에 더 설명하겠다.)<br><br>
**->** 그렇다면, OC에서 신호가 출력되니 타이머/카운터의 동작 모드를 적절히 설정하고 TCNT와 OCR을 적절히 활용하면 PWM을 발생시키거나 가변 주파수 펄스를 만들어 출력할 수 있게 된다.
<p align="center"><img src="/MyPDF/tcnt_period.png" width = "400" ></p>
<p align="center"><img src="/MyPDF/tcnt_and_ocr.png" width = "600" ></p>

위의 Waveform Generator, 파형 발생기는 OCn 단자로 출력되는 파형을 결정하는데,<br>
이는 Compare match mode 비트와 파형 발생 모드 비트(WGM21, WGM20)에 의해 결정된다.<br><br>

# 8비트 '타이머/카운터2' 의 동작
ATmega128의 8bit 타이머/카운터에는 타이머/카운터0과 타이머/카운터2가 있다.<br><br>
타이머/카운터2에는 PWM 출력을 가지고 있는 8비트 타이머/카운터로<br>**prescaler(분주)를 통해 내부 클럭을 입력으로 사용하여 동작하는 타이머 기능**과<br>
**외부 클럭을 입력으로 사용하는 카운터 기능을 수행한다.**<br><br>
타이머/카운터0과 타이머/카운터2의 차이는<br>타이머/카운터0은 타이머/카운터2에 비해 외부 클럭 소스를 사용하여 계수할 수 있는 RTC(Real Time Clock or Counter)기능과 비동기 레지스터를 가지고 있어 비동기 동작이 가능하다는 차이가 있지만,<br>전체적인 기능은 동일하다.