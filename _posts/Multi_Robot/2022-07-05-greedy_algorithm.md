---
title : "Greedy Algorithm Python 구현"
category :
    - Multi_Robot
tag :
    - Multi_Robot
    - Greedy Algorithm
    - Task Allocation
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---
Multi-Robot SLAM 개인 프로젝트에서 해결하지 못한 Robot Allocation을 위한 Greedy Algorithm을 Python으로 구현하는 포스팅입니다.

<br><br>

# # 참고 자료
[그래프 이론 : Greedy Algorithm](https://www.youtube.com/watch?v=ZeZgP4vsUuw)<br>
[코드 참고 자료 : 로봇 작업 할당 - Hungarian Algorithm](https://ropiens.tistory.com/184)<br>
[그리디(Greedy) 알고리즘과 예제 (파이썬)](https://velog.io/@cha-suyeon/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EA%B7%B8%EB%A6%AC%EB%94%94Greedy-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98%EA%B3%BC-%EC%98%88%EC%A0%9C-%ED%8C%8C%EC%9D%B4%EC%8D%AC)<br>
<br><br>

# About Greedy Algorithm
먼저 **Greedy Algorithm** 알고리즘을 사용하여 robot task allocation 문제에 접근하거나 일반적으로 문제를 해결하기 위한 최소한의 아이디어를 제공할 수 있다.<br><br>
본래 Multi Robot Exploration 프로젝트에서 각 로봇이 본인에게 제일 가까운 frontier를 향하도록 하기 위해 Allocation에 관한 기술을 적용하려 했다.<br><br>
그중 **Greedy Algorithm**를 사용하려 했고, 이를 파이썬으로 구현하고자 한다.<br><br>
아래 사진은 ROS를 이용해 시뮬레이션으로 구현한 모습인데, 로봇의 navigation cost와 information gain을 고려하였음에도 가장 가까운 frontier를 선정하지 못하는 모습을 확인할 수 있다.<br><br>
여러 면에서 아쉬운 프로젝트이다.
<p align="center"><img src="/MyPDF/multi_robot(1).png" width = "700" ></p><br>

## Greedy Algorithm의 특징
개관을 살펴보면 Greedy Algorithm을 사용하게 된다면 매 순간 가장 좋은 선택을 하고자 한다.<br><br> 
하지만 이는 현재의 선택이 나중의 선택에 어떠한 영향을 미치는지 고려하지 않기 때문에, **최적의 해를 보장하지 못한다.**<br><br>
그렇기에 이 방법으로 문제에 접근했을 때, 정확한 답을 찾아내는지에 대한 보장을 할 수 없고, 프로젝트에서도 이러한 내용을 고려하지 않고 선정하였었다.<br><br>
하지만 Greedy Algorithm으로 문제에 접근했을 때, 정확한 답을 찾아내는지에 대한 **보장이 있다면** 매우 효과적이고 직관적이다.<br><br>
결론은 Greedy Algorithm을 사용하기 위해서는 정당성 분석이 중요하며, 최적해 근사치 추정에 사용 된다.<br>
<br><br>

# Python Implementation
```python
import numpy as np
from scipy.spatial import distance_matrix
from scipy.optimize import linear_sum_assignment

from matplotlib import pyplot as plt

class Greedy_Algorithm():
    def __init__(self):
        print("------- Greedy Algorithm Class -------")
        self.seed = 15

        # Number of robots and tasks
        self.n_robot = 10
        self.n_task = 10

        self.greedy_task = []
        self.greedy_cost = 0


    def random(self, seed):
        self.seed = seed
        np.random.seed(self.seed)

        # 균등분포로부터 무작위 표본 추출 # 2D Environment
        self.robot_loc = np.random.uniform(low=-1, high=1, size=(self.n_robot, 2)) # 10 by 2
        self.task_loc = np.random.uniform(low=-1, high=1, size=(self.n_task, 2)) # 10 by 2
        # print(self.robot_loc)
        # print(self.task_loc)
        self.cost = distance_matrix(self.robot_loc, self.task_loc)
        print(self.cost)

    def plot(self):
        # Create figure
        self.fig, self.ax = plt.subplots(1, 2, figsize=(15, 5))
        self.row_ind, self.col_ind = linear_sum_assignment(self.cost)

        # plot initial mission environment
        self.ax[0].plot(self.robot_loc[:, 0], self.robot_loc[:, 1], 'k^', label="robot") # column 0, column 1
        self.ax[0].plot(self.task_loc[:, 0], self.task_loc[:, 1], 'bo', label="task")
        self.ax[0].set_title("Environment")
        self.ax[0].set(xlim=(-1.2, 1.2), ylim=(-1.2, 1.2))
        self.ax[0].set_aspect("equal")
        self.ax[0].legend()

        print(self.row_ind, self.col_ind)
        for i in range(len(self.col_ind)):
            self.ax[1].plot([self.robot_loc[self.row_ind, 0], self.task_loc[self.greedy_task, 0]], [self.robot_loc[self.row_ind, 1], self.task_loc[self.greedy_task, 1]], 'r--')
        self.ax[1].plot(self.robot_loc[:, 0], self.robot_loc[:, 1], 'k^', label="robot") # column 0, column 1
        self.ax[1].plot(self.task_loc[:, 0], self.task_loc[:, 1], 'bo', label="task")
        self.ax[1].set_title("Greedy Algorithm Cost: {:.3}".format(self.greedy_cost))
        self.ax[1].set(xlim=(-1.2, 1.2), ylim=(-1.2, 1.2))
        self.ax[1].set_aspect("equal")
        self.ax[1].legend()

    def greedy(self):
        for robot in range(self.n_robot):
            candidates_task = list(np.argsort(self.cost[robot, :])) # 크기 등수 인덱스 반환 [13, 7, 9] -> [0, 2, 1] 0번째가 1등, 2번째가 2등, 1번째가 3등
            print(candidates_task);print()
            while True:
                task = candidates_task[0]
                if task not in self.greedy_task:
                    self.greedy_task.append(task)
                    self.greedy_cost += self.cost[robot][task]
                    break
                else:
                    candidates_task.pop(0)


    def start(self, num_robots=10, num_task=10):
        self.n_robot = num_robots
        self.n_task = num_task

        print(f"num_robot : {self.n_robot}")
        print(f"num_task : {self.n_task}")

        self.random(np.random.randint(0,100))
        self.greedy()
        self.plot()

        plt.show()

if __name__ == "__main__":

    greedy_ = Greedy_Algorithm()

    greedy_.start(num_robots=8, num_task=8)

```

<br><br>

# 결과 및 고찰

<p align="center"><img src="/MyPDF/greedy(1).png" width = "800" ></p><br>

코드를 많이 참고하게 되었는데, 조금 더 알고리즘의 개념을 이해하고 직접 구현하는 연습을 해야 할 것 같다.<br><br>
또한, Greedy Algorithm Cost를 비교할 다른 allocation에 관한 알고리즘도 공부해서 최대한 직접 구현해 볼 계획이다.<br><br>  


<br>

📣<br>
포스팅에 대한 오류나 궁금한 점은 Comments를 작성해주시면, 많은 도움이 됩니다.💡
{: .notice--info}