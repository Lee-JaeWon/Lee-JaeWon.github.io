---
title : "ATmega128 Timer&Counter"
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
타이머/카운터는 내부 clock이나 외부의 clock을 소스로 사용하고, 사용자가 필요한 주기를 설정하여 일정한 주기를 발생시키는 임의의 작업을 할 수 있다.<br>
타이머/카운터는 **일반적으로 클럭이 입력되면 그 클럭을 계수(셈)하는 기능을 의미**하는데, 입력으로 들어오는 **펄스가 어디로부터 오는지에 따라** 타이머와 카운터로 나뉜다.<br>

## Timer mode
**타이머는 MCU 내부에서 발생하는 클럭을 계수하는 장치이다.**<br><br>
MCU의 **내부 클럭**과 분주기를 이용하여 일정 간격의 펄스를 만들어 원하는 간격 경과 후에 **인터럽트를 발생**시키는 기능을 의미.<br><br>
내부 클럭을 이용하기에 빠르며, 분주가 가능하다. -> 동기 모드

## Counter mode
**카운터는 MCU의 외부에서 입력되는 클럭을 세는 장치이다.**<br><br>
MCU의 **외부 핀**을 통해 펄스를 입력받고, 이 펄스(event)의 발생 횟수(사건의 발생 횟수)를 셈한다.<br><br>
외부 클럭을 이용하기에 느리며(이것은 정확히 어떤 기준을 가지고 느리다고 하는지에 대한 부분은 모르겠다.) 외부클럭을 그대로 사용하기에 분주가 불가능하다. -> 비동기 모드

## Timer/Counter
이 둘은 결국 목적과 결과는 같다는 이유로 통칭해서 부르기에 같은 것이라 생각하지만 실상은 엄연히 서로 다른 기능을 기술하고 있다.<br><br>
단순히 클럭만 세는것이 아니라 위에서 언급한 것처럼 시간 혹은 펄스와 관련된 기능 혹은 지연(delay)의 역할을 한다는 것을 상기하자.<br><br>
- 또한, **이 포스팅에서도 다른 자료에서나 Datasheet처럼**<br>
**Timer/Counter는 내/외부로 클럭을 입력받고 다루는 하나의 개념으로 보고 설명하겠다.**<br><br>

***그럼 이제 정확히 어떻게해야 시간도 잴 수 있고, 원하는 주기도 만들어서 다양하게 응용할 수 있는지 살펴보자.***<br><br><br>

# 기본 용어
## 주기와 펄스, 주파수
**주기** : 파동에서는 파동이 1회 반복하는데 걸리는 시간, 펄스는 0과 1이 번갈아 한 번 나타나는 시간.<br><br>
**주파수** : 1초 동안 진동하는 횟수, 즉 주기의 역수. $f = {1}/{t}$

## Synchronous와 Asynchronous(동기와 비동기)
**Synchronous** : 동기는 말 그대로 동시에 일어난다는 뜻이다.<br>
그렇다면, MCU에서는 MCU가 작동하고는 clock과 같은 시간에서 일한다는 것이다.<br>(MCU clock과 동기가 맞는다.)<br><br>
무슨말인지 한 번 더 정리하면, 동기는 MCU와 제때제때 데이터를 주고 받고 있다는 것이다.<br><br>
그렇다면,<br>
**Asynchronous** : 비동기는 동시에 일어나지 않는다는 것이다.<br>
즉, 데이터가 계속 왔다 갔다 하는 것이 아닌, 약속한 상황이나 신호가 입력된다면 그제서야 그 응답을 돌려주는 것이라 이해하면 편할 것 같다.<br><br>
그러면 동기가 더 좋은 것 아니냐 라고 할 수 있지만, 다른 것들과 마찬가지로 응용과 목적에 따라 필요한 것을 사용하면 된다.

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

# Timer/Counter Interrupt
위에서 설명한 TCNT와 OCR의 개념에 덧붙여<br>
Timer/Counter에서 사용되는 두 가지 Interrupt 종류를 살펴보자.
- **Overflow Interrupt** : TCNT가 TOP값을 넘으면 Overflow Interrupt 발생.

여기서 8bit와 16bit의 차이도 같이 짚고 넘어가자면, 8bit는 0~255까지 셀 수 있고, 16bit는 0~65535까지 셀 수 있다.<br>
이 때, Top값은 각각 255와 65535가 된다.<br><br>
16bit에서는 TCNT 레지스터가 계수할 수 있는 범위가 8bit에 비해 확장된다.<br>
이 때, 주기를 더 길게 할 수 있으므로 저주파 신호를 출력할 수도 있다.
- **Compare Match Interrupt** : TCNT가 설정한 OCR값과 비교시 같아졌을때, Compare Match Interrupt가 발생한다.

# Timer/Counter Mode
비트 수와 타이머/카운터 종류에 따라 모드가 세분화되거나 다르지만,<br>
대략적인 타이머/카운터의 Mode는 다음과 같다.<br>
(데이터시트 97페이지 참고)
- **Normal Mode** : Overflow Interrupt 사용
- **CTC(Clear Time on Compare Match) Mode** : Compare Match Interrupt 사용 
- **Fast PWM Mode** : 두 Interrupt 모두 사용.<br>
높은 주파수의 PWM 파형이 필요할 때 주로 사용<br><br>
TCNT와 OCR이 같으면 Compare Match Interrupt 발생과 동시에 OC에서 Low 출력,<br>
이 후, TCNT는 계속 진행하며 MAX에 도달 시, OC에서 high 출력.<br><br>
Fast PWM Mode가 더 빠른 이유는 일반적으로 Compare match나 Overflow가 일어날 때만 파형을 바꾸는 것이 아닌 한 주기내에서 OCR과 MAX 지점에서 두 번 Toggle을 하기 때문이다.<br>
(fast PWM mode can be twice as high as the phase correct PWM)<br>
<p align="center"><img src="/MyPDF/Fastpwm.png" width = "300" ></p>
부가적으로, phase correct PWM은 OCR과 Compare Match가 일어날 때만 Toggle한다.<br>
phase correct PWM은 Fast PWM보다는 최대 동작 주파수가 작지만, 삼각파를 사용하는 대칭적인 특징 때문에 모터 제어 응용에서 많이 사용된다.<br><br>

# 8비트 '타이머/카운터' 의 동작
## 개요
ATmega128의 8bit 타이머/카운터에는 타이머/카운터0과 타이머/카운터2가 있다.<br><br>
타이머/카운터2에는 PWM 출력을 가지고 있는 8비트 타이머/카운터로<br>**prescaler(분주)를 통해 내부 클럭을 입력으로 사용하여 동작하는 타이머 기능**과<br>
**외부 클럭을 입력으로 사용하는 카운터 기능을 수행한다.**<br><br>
타이머/카운터0과 타이머/카운터2의 차이는<br>타이머/카운터0은 타이머/카운터2에 비해 외부 클럭 소스를 사용하여 계수할 수 있는 RTC(Real Time Clock or Counter)기능과 비동기 레지스터를 가지고 있어 비동기 동작이 가능하다는 차이가 있지만,<br>전체적인 기능은 동일하다.<br><br>

## 특징
- PWM 및 비동기 동작 Mode를 가지는 8bit Up/Down counter
- 8bit -> 0~255까지 Count 가능
- 10bit prescaler
- Interrupt 사용 가능(위에서 언급한 두 가지)

## Block Diagram
<p align="center"><img src="/MyPDF/8bittc.png" width = "600" ></p>

## Registor Control (Timer/Counter0 기준)
### TCCR0
**동작모드와 분주비를 결정한다.**<br><br>
<p align="center"><img src="/MyPDF/tccr0.png" width = "800" ></p>

* Bit 6, 3 : WGM00 & WGM01
**Waveform Generation Mode**

<p align="center"><img src="/MyPDF/wgm.png" width = "600" ></p>
위에서 언급한 Timer/Counter Mode에 관한 설정이다.

* Bit 5, 4 : COM01 & COM00
**Compare Match Output Mode**<br>
Compare Match가 일어났을때(OCR이든 TOP이든), 수행할 동작을 결정한다.<br>
COM레지스터 설정은 non-PWM, Fast PWM, phase Correct PWM Mode에 따라 설정 양식이 다르다.

    * COM, non-PWM
    <p align="center"><img src="/MyPDF/comnonpwm.png" width = "600" ></p>
    non-PWM에서는 OC로 파형을 물리적인 핀으로 출력해낼 필요도 없고,<br>
    그렇기에 굳이 파형을 Toggle하거나 할 필요없이, 인터럽트 걸릴 때마다 코드 내부적으로 flag 변수나 cnt변수 등을 두어서 활용할 수 있다.<br><br>
    그래서 일반적인 경우 non-PWM에서는 `COM01, COM00 : 0, 0`으로 설정해준다.<br>
    (물론 다른 용도나 파형을 다루어야 하는 경우는 재량 것 COM 레지스터를 수정하면 되는데 그 정도 수준이 되면 데이터시트에서 잘 찾아서 할 정도라 생각한다.)

    * COM, Fast-PWM
    <p align="center"><img src="/MyPDF/comfastpwm.png" width = "600" ></p>
    PWM에서는 물리적으로 OC핀에서 파형을 출력해야하는 경우일텐데,<br>
    위의 Fast-PWM에서 설명한 것처럼 Compare Match시 Low, overflow interrupt 후 Bottom 복귀 시 High라고 설명했었다.<br>
    그러면 이에 맞는 것은 `COM01, COM00 : 1, 0`이 될 것이다.(해당하는 Description을 읽어보자 별로 길지도 않다.)<br><br>
    혹여라도 이의 반대 방향의 파형을 원한다면 어떻게 설정해야할까?(문제로 남겨놓겠음. Description을 읽으면 바로 알 수 있을 것이다.)

    * COM, Phase Correct PWM Mode
    <p align="center"><img src="/MyPDF/phasepwm.png" width = "600" ></p>
<br><br>
* Bit 2, 1, 0 : CS02 & CS01 & CS00
<p align="center"><img src="/MyPDF/cs210.png" width = "600" ></p>
Prescaler의 분주비를 설정해준다.<br>
10bit prescaler답게 1024까지 분주가 가능하다.<br>
<p align="center"><img src="/MyPDF/timer0prescaler.png" width = "600" ></p>
$clk_{T0S}$는 위의 다이어그램에서 확인할 수 있으며, 내부 혹은 외부 클럭 소스가 선택된 후 나온 클럭이다.<br>
이 클럭이 Prescaler를 통과하여 원하는 클럭이 출력된다.

### TCNT0
<p align="center"><img src="/MyPDF/tcnt0.png" width = "600" ></p>
위에서 말한 Timer/Counter 8bit 값을 저장하고 있는 TCNT 레지스터이다.<br>

### TIMSK
<p align="center"><img src="/MyPDF/timsk0.png" width = "600" ></p>

* Bit 1 : OCIE0
OCIE0가 1로 set되면 Output Compare Match Interrupt Enable로 설정된다.<br>

* Bit 0 : TOIE0
0번 비트가 1로 set되면 Overflow Interrupt Enable.<br>
(여기까지 다 읽고 왔고, 데이터시트도 한 번쯤 봤다면 지금 이게 무슨 소리지? 라고 하면 안된다.)

## 전역 인터럽트 활성화
지금까지 레지스터를 설정한 것들은 실제로는 Initialization과정이다.<br><br>
실제로 어떠한 코드가 작동하기 전에 전역적으로 인터럽트를 활성화 해줘야한다.<br><br>
원래는 SREG 레지스터를 설정해줬었는데, AVR 버전이 높아지며 `sei();`만으로도 전역 인터럽트 활성화를 시켜줘야한다.<br>
(이유를 물어본다면 본인은 코드 시작전에 내가 인터럽트를 사용할 것이니 전체적으로 인터럽트를 사용하겠다 라고 sei를 통해 알려준다고 받아들였다.)

## 전역 인터럽트 비활성화
는 `cli();`를 통해 비활성화해준다.<br>
예제에서는 많이 안쓰였지만, 실제로 자신이 무엇을 만들다보면 전역적으로 인터럽트를 비활성화 시켜버려야할 순간도 올 수 있다.<br><br>
본인은 SCARA를 제작할 때, 어떤 스위치가 눌리면 전역적으로 모두 인터럽트를 해제하고 다른 작업을 해야할 때 사용했다.<br>

## 8bit 예제 : 원하는 주기마다 LED켜기
### 원하는 주기 만들기
먼저 OCR은 이용하지 않고, Overflow Interrupt와 TCNT의 초기값 설정만으로 주기를 만들어보자.<br><br>
$$ \frac{1}{T_{desired}}=f_{desired}=\frac{f_{osc}/psc}{(TOP+1)-TCNT_{init}} $$<br><br>
여기서 $f_{osc}$는 16MHz이고, $psc$는 prescaler이다.<br>
우리는 이 식을 통해 원하는 주기에 맞는 초기 TCNT값을 찾을 것이다.<br>
(Overflow Interrupt를 사용할 것이므로 TCNT의 시작값을 높여서 주기를 맞추겠다는 의미이다.)<br>
<p align="center"><img src="/MyPDF/tcnt_ex.png" width = "400" ></p>
<br><br>
예시 : 나는 2ms를 만들어내고 싶다. 그럼 일단 분주비는 몇으로 해야하지?<br>
일단 분주비 8로 한 번 해보자.<br><br>
$$ \frac{1}{0.002}=500=\frac{16000000/8}{(255+1)-TCNT_{init}} $$<br>
로 계산했더니, TCNT = -3744가 나와버렸고 이는 우리가 아는 TCNT 0~255의 범위에 맞지 않는다.<br><br>
256으로 해보자.<br><br>
$$ \frac{1}{0.002}=500=\frac{16000000/256}{(255+1)-TCNT_{init}} $$<br>
이면, TCNT = 131이 나오게 된다.<br><br>
즉, TCNT0가 0이 아닌 131부터 시작한다면, 2ms 마다 Overflow Interrupt가 발생한다는 뜻이 된다.<br><br>
이제 내가 1초를 원한다면, 2ms를 500번 반복하면 된다.<br>

### 실제 코드 (8bit Timer/Counter0)
2ms주기를 만든 후, cnt == 500일 때마다 (1초) LED 점멸을 뒤집는다.<br>
```c
#include <avr/io.h>
#include <avr/interrupt.h>

int cnt;

ISR(TIMER0_OVF_vect){
	cnt++;
	TCNT0 = 131;
	
	if(cnt == 500){
		PORTA = ~PORTA;
		cnt = 0;
	}
}

int main(void)
{
	
	DDRA = 0xff;
	PORTA = 0xaa;
	
	TCCR0 = (0<<WGM01)|(0<<WGM00)|(0<<COM01)|(0<<COM00)|(1<<CS02)|(1<<CS01)|(0<<CS00);
	TIMSK = (1<<TOIE0);
	TCNT0 = 131;
	
	sei();
	/* Replace with your application code */
	while (1)
	{
	}
}
```

# 16비트 '타이머/카운터' 의 동작
## 개요
16비트 타이머/카운터도 8bit와 큰 차이가 나는 것은 아니다.<br><br>
기능의 차이와 더 많은 비트를 한 번에 Bus에 보낼 수 있거나,<br>
앞 서 언급한 것처럼 counting할 수 있는 범위가 더 많아진다.<br>
(Most register and bit references in this section are written in general form)

## 특징
- 0~65535까지 counting가능
- 10bit prescaler
- 3개의 PWM출력 가능
- Input Caputure 기능
- 10개의 Independent Interrupt Sources

## Registor Control (Timer/Counter1 기준)
### TCCR1A & TCCR1B
**TCCR은 동작모드와 분주비를 결정한다.**
MCU자체는 8bit짜리 MCU이기 때문에 위 8bit예제에서 본 것처럼 TCCR을 설정하지만 더 많은 bit를 다루어야하기 때문에, A, B, C로 나뉜다.<br>
<p align="center"><img src="/MyPDF/tccr1a.png" width = "600" ></p>

- Bit 7:6 – COMnA1:0: Compare Output Mode for Channel A
- Bit 5:4 – COMnB1:0: Compare Output Mode for Channel B
- Bit 3:2 – COMnC1:0: Compare Output Mode for Channel C

원하는 출력 포트(OCn)에 따라 Bit를 설정해주면 된다.<br><br>
COM 레지스터는 위에서 설명했지만, Compare Match가 일어났을때(OCR이든 TOP이든), 수행할 동작을 결정한다.<br>
COM레지스터 설정은 non-PWM, Fast PWM, phase Correct PWM Mode에 따라 설정 양식이 다르다.
<p align="center"><img src="/MyPDF/tccrn_nonpwm.png" width = "600" ></p>
<p align="center"><img src="/MyPDF/tccrn_fastpwm.png" width = "600" ></p>

그리고 WGM과 분주비(cs레지스터)는 TCCR1A와 TCCR1B로 나눠져있다.<br>
어떻게 설정해줘야할지 보자.<br>
<p align="center"><img src="/MyPDF/tccr1a.png" width = "600" ></p>
- Bit 1:0 – WGMn1:0: Waveform Generation Mode
<p align="center"><img src="/MyPDF/tccr1b.png" width = "600" ></p>
- Bit 4:3 – WGMn3:2: Waveform Generation Mode
WGM을 설정해주고 싶은데 타이머/카운터1 에서 위와 같이 WGM이 2개씩 나눠져 있다.<br>
<p align="center"><img src="/MyPDF/16bitwgm.png" width = "600" ></p>
하지만 어렵지 않다, 원하는 모드를 찾아서 그저 Table을 보고 설정해주면 된다.<br>
(지금까지 계속해왔던 과정)<br><br>
TCCR1B를 보니 CS도 보인다. 분주비 설정도 해줄 수 있다.<br>
- Bit 2:0 – CSn2:0: Clock Select
<p align="center"><img src="/MyPDF/16bitcs.png" width = "600" ></p>

### TCNT1
<p align="center"><img src="/MyPDF/tcnt1.png" width = "600" ></p>
16bit 범위로 값을 증가하거나 감소시키기에 다음과 같이 H/L로 구성되어있다.<br>

### OCR1
OCR1에는 A,B,C로 나눠져있는데 이는 원하는 OCn에 따라 설정해주면 된다.<br>
(16bit에서 OC핀을 통해 3개의 파형을 출력할 수 있다고 위에서 설명했다.)<br><br>
만약, OC1A에서 출력하고 싶다면 OCR1A를 설정해주면 된다.<br>
<p align="center"><img src="/MyPDF/ocr1a.png" width = "600" ></p>
Compare Match Interrupt를 위해 사용된다.

### ICR1
<p align="center"><img src="/MyPDF/icr1.png" width = "600" ></p>
**Input Capture Register**이다.<br><br>
Input Capture는 ICPn핀에 event가 발생하면, ICR레지스터 값을 TCNT값으로 갱신한다.<br><br>
또한, Input Capture는 **counter의 TOP value를 지정**하는데도 사용할 수 있다.<br>
(실질적으로 TOP값 지정하는데 이 기능을 사용할 것이다.)<br>

### TIMSK
**Timer/Counter Interrupt Mask Register**<br>
**Timer/Counter1 의 Interrupt를 Enable하는 Register**
<p align="center"><img src="/MyPDF/timsk.png" width = "600" ></p>
- Bit 4 – OCIE1A: Timer/Counter1, Output Compare A Match Interrupt Enable
- Bit 3 – OCIE1B: Timer/Counter1, Output Compare B Match Interrupt Enable
- Bit 2 – TOIE1: Timer/Counter1, Overflow Interrupt Enable

### ETIMSK
ETIMSK는 TIMSK의 Extended 레지스터이다.
OCIE1C와 Timer/Counter3 Interrupt Enable에 관한 설정을 하게 된다.

## 16bit 예제 : Duty Ratio 80%인 PWM만들기
$$ \frac{1}{T_{desired}}=f_{desired}=\frac{f_{osc}/psc}{(TOP+1)-TCNT_{init}} $$<br><br>
이 식에서 $TCNT_{init}$는 0이고 $TOP$는 $ICR$로 설정하고 분주는 1로 할 것이다.<br><br>
Mode는 Fast-PWM을 할 것이니까, OCR에서 Compare Match Interrupt, TOP(ICR)에서 Overflow Interrupt가 발생한다.<br><br>
모터 출력을 할 것인데, Motor 주파수는 20KHz로 설정할 것이다.<br><br>
$$ 20000=\frac{16000000/1}{(ICR + 1)-0} $$<br><br>
$ICR = 799$가 나옴을 확인하였고, Duty Ratio 80%가 되려면 OCR이 ICR의 80%가 되야한다.<br> 
이제 실제 코드를 살펴보자.<br>
```c
#include <avr/io.h>

int main(void)
{
	DDRB = 0xFF;
	DDRE = 0x0F;
	
	PORTE = 0b00001010;
	
	TCCR1A = (1<<COM1A1)|(0<<COM1A0)|(1<<COM1B1)|(0<<COM1B0)|(1<<WGM11)|(0<<WGM10);

	TCCR1B = (1<<WGM13)|(1<<WGM12)|(0<<CS02)|(0<<CS01)|(1<<CS00);
	
	ICR1 = 799;
	OCR1A = 639;
	OCR1B = 639;
	
    /* Replace with your application code */
    while (1) 
    {
    }
}
```
<br><br>

📣<br>
본 포스팅의 언어 및 개발 환경 : `Windows`, `C`. `MicroChip Studio(AVR7)`<br>
포스팅에 대한 오류나 궁금한 점은 Comments를 작성해주시면, 많은 도움이 됩니다.💡
{: .notice--info}