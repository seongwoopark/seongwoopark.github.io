---
layout: post
title: Elevator Stops
subtitle: from Codility
category: algorithm
tags: [algorithm]
---
<h4>문제</h4>
**Note:** 문제 이름은 임의로 작성한 것 입니다.<br/>
![problem](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-12-07-algorithm-elevator_stops_1.png)<br/><br/>
![problem](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-12-07-algorithm-elevator_stops_2.png)<br/><br/>

<h4>풀이 및 답</h4>
문제를 다 읽고나서 우선, 문제 흐름대로 코멘트 부터 작성했다. 왜 영어로 작성했는지는 모르겠지만 문법 파괴는 덤이다.
```python
def solution(A, B, M, X, Y):
    # take passengers based on constraints
    # stop at each target floors of passengers and return ground floor
    # repeat it and if queue is empty and return stop counts
```
그리고 역시 문제 그대로 G층에서 대기하는 passenger들의 waiting queue를 작성했고, 이후 로직들도 죽죽죽 썼더니 아래와 같은 코드가 나왔다.
```python
from collections import deque


def solution(A, B, M, X, Y):
    stop_counts = 0
    waiting_queue = deque(zip(A, B))

    while True:
        # take passengers based on constraints
        num_passengers = 0
        total_weight = 0
        stop_floors = set()

        # first passenger
        weight, target_floor = waiting_queue.popleft()
        while num_passengers + 1 <= X and total_weight + weight <= Y:
            # update elevator status
            num_passengers += 1
            total_weight += weight
            stop_floors |= {target_floor}

            # check empty queue
            if not waiting_queue:
                break
            # next passenger
            weight, target_floor = waiting_queue.popleft()
        else:
            # restore passenger
            waiting_queue.appendleft((weight, target_floor))

        # stop at each target floors of passengers and return ground floor
        stop_counts += len(stop_floors) + 1

        # repeat it and if queue is empty and return stop counts
        if not waiting_queue:
            break

    return stop_counts
```
다행스럽게도 기본 테스트 케이스들에 대해서 문제없이 working 했다. 이 문제가 가장 마지막에 푼 문제였는데, ~~(문제설명이 가장 길어서 마지막으로 넘겼다.)~~ <br/>
다 풀고나서 시간이 남아, 다시 한번 문제를 볼 시간이 있었다. 다시 보니 이 문제만 complexity에 대한 언급이 있어서 봤더니
N과 M에 대한 수식이었다. 내 코드를 보니.. 응? 난 M을 안썼는데.. 라고 생각하면서 아래와 같은 주석을 달기 시작했다.(task 2는 문제번호이다.)
```python
from collections import deque


def solution(A, B, M, X, Y):
    """
    :param A: array of passenger weights
    :param B: array of passenger target floor
    :param M: max floors in hotel
    :param X: elevator constraint, number of passengers
    :param Y: elevator constraint, total weight of passengers

    :return: stop counts
    Note: in this solution, M is not used. so I have to consider only N factor.
    so expected complexities in task 2 can be modified, like,
    time: O(N * log(N))
    space: O(N)
    """
    ...
    ..
    .
```
complexity 파악을 제대로 하지는 않았지만, 아마 시간에 대한 worst-case expected가 얼추 O(N)이 될 것 같다.
메모리는 변수를 몇 개 쓰지 않아서...  아마 가장 많이 잡아 먹을 변수가 파라미터 제외하고 deque 인스턴스일 것이다.
그래서 이것도 worst-case expected가 얼추 O(N)이 될 것이라 추정해서, 제출 했더니 timeout 케이스가 발생하지 않았다.