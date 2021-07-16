---
title : "4 Wheel Steering Kinematics"
category :
    - 4WS
tag :
    - 4WS
    - Kinematics
toc : true
toc_sticky: true
comments: true
---

## Goal of Drive
4개의 Wheel을 모두 조향하여 주행하는 것이 목표이다.

이를 위한 Kinematics Model을 살펴,  
사용자가 속력과 방향을 제시할 때, 각 바퀴에 해당하는 조향각과 모터 속도에 따라 작동해야하는 것이 목표다.  

### Ackerman condition for a front wheel steering system
[살펴보고 있는 자료]()에서는 먼저 앞바퀴가 조향하는 Ackerman condition의 자료를 인용하여 이를 먼저 설명한다.

### Space Required for Turning
회전에 필요한 공간은 두 원 밖으로 나가지 않도록 하는 공간이다.  
이를 측정하는데는, 조향 각과 차량의 스펙, Center of Rotation과 차량의 거리에 의해 결정된다.

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125956165-9084d6ad-fd6f-490e-9b4f-b506746d01c6.png" width = "400" ></p>

위 두 원에 관한 식은 다음과 같다.  
<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125957580-c22720c1-bf6d-4ace-8e2c-ece26422c5bf.png" width = "300" ></p>

이 R에 관한 식에 뒷부분에서 사용된다.  

### Four wheel Steering Types
4륜 스티어링 구성에는 두 가지 유형이 있다.  
프론트 및 후방 휠이 모두 동일한 방향으로 회전(조향)하는 것을 Positive 4륜 스티어링 시스템이라고 하며, 서로 반대로 회전하는 것을 Negative 4륜 스티어링 시스템이라고 한다.  

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125957723-517697e2-8171-4b11-8c41-9fccac9d2c44.png" width = "400" ></p>
다음과 같이 동작하는 것이 Positive Type이며, 우리는 초점을 Negative Type에 둘 것이다.

#### Negative Four Wheel Steering System

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125957929-f841b5c7-059e-427d-91ed-60c790b51107.png" width = "400" ></p>  
다음과 같이 동작하는 것이 Negative Type이다.  

위의 조향 시스템에 대한 방정식은 아래의 방정식에서 도출된다.  
<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125977147-30db0e5f-db5c-4aa8-bedc-a61fc831b02c.png" width = "400" ></p>  

즉, w_sub_f(r)는 상수이므로, C1 혹은 C2에 대한 정보가 필요하다.

그래서 이 정보들을 위해,

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125978423-3291dd0e-4fdc-4caf-89a5-1ff9ab3e5ed3.png" width = "400" ></p>  
다음의 방정식들을 통해 원하는 정보를 얻어낼 것이다.

위 식들은 Ackerman condition에서 차체의 폭과 길이간의 비율은  
cot(조향 각)의 차이와 같다는 식에서 파생되었다.
(The difference of the cotangents of the angles of the front outer to the inner wheels should be equal to the ratio of width and length of the vehicle)  

식(20)에서 W와 l은 알고 있는 상수이다.  

또한, 식(17), (18)을 c에 관한 식으로 나타낼 수 있으며, 이때의 분모는 0이 아니어야 한다.

하지만 이 때의 문제는, 방정식이 있더라도 알고 있는 정보가 적어서 식이 풀리지 않는다.  
궁극적으로 바퀴 네 개의 각도를 모두 구해야 하는데,  

c1, c2, R_sub_l 에 관한 정보(바깥의 점으로 부터 나오는 정보)가 없기 때문에 알 수가 없다.  

그래서 우리가 조향하려는 각도를 이용해 앞 바퀴 중 하나 혹은 둘 의 각도를 알게 된다면  
(혹은 회전 반경에 관한 정보를 알게 된다면)  

이를 통해 식(17)을 이용해 c1을 쉽게 구할 수 있고,  
c1을 통해 c2를 알 수 있으므로, 식(16)을 통해 뒷 바퀴의 조향각 또한 알아낼 수 있다.

#### Symmetric Four Wheel Steering System
위 시스템을 대칭 시스템으로 할 수 있다.  

이 시스템의 장점은 동일한 각도로 회전한다는 것이다.

그러므로, 위에서 결정하지 못했던 회전 반경에 관한 사항들을 결정할 수 있다.  

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125986098-a908a04c-fe70-4053-861c-68bdf0f7c6b8.png" width = "400" ></p>  

c1, c2는 각각 차체 길이의 절반이 되기 때문에

위에서 사용했던 방정식들이 간단하게 해결될 수 있다.

하지만 두 방식에서 여전히 문제가 되는것은, 사용자가 원하는 각도(ex, 핸들을 통한 각도)를 어떻게 반영하느냐 이다.


