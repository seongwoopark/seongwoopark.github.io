---
layout: post
title: Primality Algorithms
subtitle: from HackerRank
category: algorithm
tags: [algorithm, primality, prime number]
---
<h4>문제</h4>
![problem](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-10-27-algorithm-primality_check_1.png)<br/><br/>
![problem](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-10-27-algorithm-primality_check_2.png)<br/><br/>

<h4>풀이 및 답</h4>
Note를 제대로 읽지 않고 그냥 아무생각없이 O(n)이 되도록 문제를 풀었을 때는 아래와 같았다.
```python
for t in range(int(input())):
    n = int(input())
    for i in range(2, n):
        if n % i == 0:
            print('Not prime')
            break
    else:
        if n == 1:
            print('Not prime')
        else:
            print('Prime')
```
제출 후에 timeout case가 몇개 걸리고 나서야 Note를 다시 보고 제곱근이라는 힌트를 얻고 관련된 숫자를 생각해보았다.<br/>
2, 3, 4, 10을 divisor(약수)로 중복으로 갖는 숫자 4, 9, 16, 100들을 예시로 생각해보니,<br/>
4의 약수 1, 2, 2, 4(이하 중복 약수 1번표시)<br/>
9의 약수 1, 3, 9<br/>
16의 약수 1, 2, 4, 8, 16<br/>
100의 약수 1, 2, 4, 5, 10, 20, 25, 50, 100<br/>
의 값을 가졌다. 즉, 어느 수이든 중간약수를 기준으로 pair를 이룬다(너무나도 당연히 약수라는 성질때문에)<br/>
따라서, 중간약수까지만 체크하면 되는 로직이 되므로, 아래와 같이 코드를 작성함으로써 반복문을 줄일 수 있었다. 
```python
from math import sqrt

for t in range(int(input())):
    n = int(input())
    sqrt_n = int(sqrt(n)) + 1
    for i in range(2, sqrt_n):
        if n % i == 0:
            print('Not prime')
            break
    else:
        if n == 1:
            print('Not prime')
        else:
            print('Prime')
```