---
layout: post
title: Bitwise And
subtitle: from HackerRank
category: algorithm
tags: [algorithm, bitwise operator, bitwise and]
---
<h4>문제</h4>
![problem](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-11-01-algorithm-bitwise_and_1.png)<br/><br/>
![problem](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-11-01-algorithm-bitwise_and_2.png)<br/><br/>

<h4>풀이 및 답</h4>
처음 brute force하게 문제를 해결했을때는 O(n!)이었다. 
```python
t = int(input().strip())
for a0 in range(t):
    n, k = input().strip().split(' ')
    n, k = [int(n), int(k)]

    m = 0
    for a in range(1, n+1):
        for b in range(a+1, n+1):
            if m < a & b < k:
                m = a & b
    print(m)
```
timeout case가 발생하면서 개선하기 위해서 골머리를 썩였다. 약간의 변화 후 2차시도. 역시나 worst case는 O(n!)이다.
```python
t = int(input().strip())
for a0 in range(t):
    n, k = input().strip().split(' ')
    n, k = [int(n), int(k)]

    m = 0
    done = False
    for a in range(1, n+1):
        for b in range(a+1, n+1):
            if m < a & b < k:
                m = a & b
                if m == k - 1:
                    done = True
                    break
        if done:
            break
    print(m)
```
역시 timeout, 다시 머리를 꽁꽁싸매고 3차시도. C의 더블 포인터 개념으로 접근, worst case는 아마 O(logn!)?
```python
t = int(input().strip())
for a0 in range(t):
    n,k = input().strip().split(' ')
    n,k = [int(n),int(k)]
    
    m = 0
    done = False
    for a, rev_a in zip(range(1, n+1), reversed(range(1, n+1))):
        for b, rev_b in zip(range(a+1, n+1), reversed(range(a+1, n+1))):
            if m < a & b < k:
                m = a & b
            if m < rev_a & rev_b < k:
                m = rev_a & rev_b
            if m == k - 1:
                done = True
                break
        if done:
            break
    print(m)
```
timeout과 더불어 error case도 등장.. 뭔가 더 똘똘하게 풀어야할 것 같다고 생각하고, 꽤나 머리를 싸맸지만,
한동안 TIL진행을 못하다가 결국 오늘에서야 discussion탭을 cheating했다.
```python
T = int(input().strip())
for _ in range(T):
    n , k = map(int , input().split())
    print(k-1 if ((k-1) | k) <= n else k-2)
# reference: Jekus의 comment from https://www.hackerrank.com/challenges/30-bitwise-and/forum
```
이.. 하.. 얼마나... 짧단 말인가.. comment를 읽어보니 수학적으로 접근했다.
![proof](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-11-01-algorithm-bitwise_and_3.png)<br/><br/>
오늘도 머리 좋은 분을 보고 나의 머리에 참담함을 느낀다.