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

4 Wheel Steering Kinematics

## Goal of Drive
4개의 Wheel을 모두 조향하여 주행하는 것이 목표이다.

이를 위한 Kinematics Model을 살펴,  
사용자가 속력과 방향을 제시할 때, 각 바퀴에 해당하는 조향각과 모터 속도에 따라 작동해야하는 것이 목표다.  

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/126025383-67d4572a-6669-4dae-adbe-4183fee92262.png" width = "400" ></p>

### Ackerman condition for a front wheel steering system(FWS)
먼저 앞바퀴가 조향하는 [Ackerman condition의 자료](https://link.springer.com/chapter/10.1007%2F978-0-387-74244-1_7)를 인용하여 이를 먼저 설명한다.

### Turning Radius
일반적인 vehicle에서 회전하는 경우를 살펴보면 각각의 바퀴각의 평균을 이용해 R값을 구하는 것을 확인할 수 있다.  

이 개념은 4WS에서도 사용할 것이다.

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/126030353-e893c916-408f-45a4-9449-1e5e8547296f.png" width = "400" ></p>

### Space Required for Turning
회전에 필요한 공간은 두 원 밖으로 나가지 않도록 하는 공간이다.  
이를 측정하는데는, 조향 각과 차량의 스펙, Center of Rotation과 차량의 거리에 의해 결정된다.

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125956165-9084d6ad-fd6f-490e-9b4f-b506746d01c6.png" width = "400" ></p>

위 두 원에 관한 식은 다음과 같다.  
<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125957580-c22720c1-bf6d-4ace-8e2c-ece26422c5bf.png" width = "300" ></p>
  

### Four wheel Steering Types
4륜 스티어링 구성에는 두 가지 유형이 있다.  
프론트 및 후방 휠이 모두 동일한 방향으로 회전(조향)하는 것을 Positive 4륜 스티어링 시스템이라고 하며, 서로 반대로 회전하는 것을 Negative 4륜 스티어링 시스템이라고 한다.  

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125957723-517697e2-8171-4b11-8c41-9fccac9d2c44.png" width = "400" ></p>
다음과 같이 동작하는 것이 Positive Type이며, 우리는 초점을 Negative Type에 둘 것이다.

#### Negative Four Wheel Steering System

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125989954-8e77f9f0-b733-41da-89ca-4955664b6c1d.png" width = "400" ></p>  
다음과 같이 동작하는 것이 Negative Type이다.  

위의 조향 시스템에 대한 방정식은 아래의 방정식에서 도출된다.  
<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125977147-30db0e5f-db5c-4aa8-bedc-a61fc831b02c.png" width = "200" ></p>  

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
위의 문제를 해결하기 위해, 위 시스템을 대칭 시스템으로 할 수 있다.  

이 시스템의 장점은 안쪽은 안쪽끼리 바깥쪽은 바깥쪽끼리 동일한 각도로 회전한다는 것이다.  
(다뤄야하는 성분이 4개에서 2개로 줄었다.)

또한 대칭을 이루기 때문에 회전 반경에 대한 사항이 결정된다.

그러므로, 위에서 풀지 못한 식들을 해결할 수 있다.  

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125986098-a908a04c-fe70-4053-861c-68bdf0f7c6b8.png" width = "400" ></p>  

c1, c2는 각각 a1, a2와 동일해지고 R(O부터 CG까지의 길이)은 R_sub_L과 같아진다.

이로 인해,  
위에서 사용했던 방정식들이 간단하게 해결될 수 있다.

하지만 두 방식에서 여전히 문제가 되는것은, 사용자가 원하는 각도(ex, 핸들을 통한 각도)를 어떻게 반영하느냐 이다.  
=> 이 문제를 위의 앞바퀴만 조향할 때 각도를 다루는 방법을 통해 해결한다.

정리
> - R = a1 * cot(δ_sub_f)  
> - δ_sub_f = (cotδ𝑜+cotδ𝑖)/2
> - R = R_sub_L  

이 때, δ_sub_f는 사용자가 주는 각도라 할 수 있다.  

> - c1 = a1  
> - c2 = a2  

위의 사항들을 통해, 아래의 식을 풀 수 있다.  
대칭 시스템이기 때문에 결과적으로는 두개의 각도가 나올 것이다.
<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125977147-30db0e5f-db5c-4aa8-bedc-a61fc831b02c.png" width = "200" ></p>
 
### About Velocity
결론적으로 위의 조향 방식과 적절한 속도를 이용하여, 회전하는데 있어 안정적이어야 한다.

우리가 운전할때도 알 수 있듯이 보통 커브를 해야하는 상황에서는 진입 시 속도를 낮춰야한다.

마찬가지로 4WS System이 회전하는 상황에서 탈조하지 않을 적절한 Speed를 찾아야한다.  

간단한 전방-후방 움직임 동안 모든 휠은 미끄러짐을 방지하기 위해 정확히 동일한 속도로 움직여야 하며, 휠 속도 간의 순간적인 불일치로 인해 휠이 미끄러지거나 예상치 못하게 미끄러질 수 있기에 제어에 있어 중요한 사항이다.  

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/125990055-98863373-a8bf-4512-ad1f-222d8a87ec4f.png" width = "400" ></p> 

안 쪽 wheel이 바깥 쪽 wheel 보다 느려야한다.(회전반경 기준)

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/126026004-87710b87-33ea-41e0-a3a3-0209df5cf11e.png" width = "400" ></p>

다음과 같은 식으로 표현된다.
<p align="center"><img src="https://user-images.githubusercontent.com/72693388/126026335-0dfcc386-3fe4-427d-a0be-c1ba48d30a89.png" width = "200" ></p>

속도는 다음과 같이 벡터로 합쳐 표현할 수 있으며, w는 angular velocity이다.  
I = 1,2,3,4 는 휠의 중앙으로 부터의 기하학적 Point들이다.

이를 y축에 정사영하면 다음을 얻는다.  
<p align="center"><img src="https://user-images.githubusercontent.com/72693388/126026514-83e7bdf1-b248-48a6-bcef-bd8acc47ffd7.png" width = "200" ></p>

대칭 조향 상황에서 전방 휠과 후방 휠의 속도의 크기는 같아야한다.

<p align="center"><img src="https://user-images.githubusercontent.com/72693388/126027111-4d253a6b-dfca-4f25-ad67-9af89ea15c93.png" width = "200" ></p>

그러므로, 다음과 같은 과정을 통해 각각의 V값을 알아낼 수 있다.

결론적으로, 각속도가 필요하다.  
각속도는 순간 각속도의 개념을 이용해 구해야할 듯 하다.
(대칭 조향 시스템에서 각도는 알 수 있기 때문?)