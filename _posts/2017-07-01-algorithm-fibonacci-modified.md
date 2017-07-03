---
layout: post
title: Fibonacci Modified
subtitle: from HackerRank
category: algorithm
tags: [algorithm, dynamic programming]
---

<h4>문제</h4>
![envrionment_vars](https://github.com/seongwoopark/seongwoopark.github.io/blob/master/img/2017-07-01-algorithm-fibonacci-modified_1.png?raw=true)<br /><br />
![envrionment_vars](https://github.com/seongwoopark/seongwoopark.github.io/blob/master/img/2017-07-01-algorithm-fibonacci-modified_2.png?raw=true)<br /><br />

<h4>풀이 및 답</h4>
```
#!/bin/python3

t1, t2, n = map(int, input().split())
modified_fibonacci_sequence = [t1, t2]

for i in range(2, n):
    modified_fibonacci_sequence.append(modified_fibonacci_sequence[i - 2] + modified_fibonacci_sequence[i - 1] * modified_fibonacci_sequence[i - 1])

print(modified_fibonacci_sequence[n - 1])
```