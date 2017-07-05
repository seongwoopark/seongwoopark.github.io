---
layout: post
title: Recursive Digit Sum
subtitle: from HackerRank
category: algorithm
tags: [algorithm, recursion]
---

<h4>문제</h4>
![problem](https://github.com/seongwoopark/seongwoopark.github.io/blob/master/img/2017-07-05-algorithm-recursive_digit_sum_1.png?raw=true)<br /><br />
![problem](https://github.com/seongwoopark/seongwoopark.github.io/blob/master/img/2017-07-05-algorithm-recursive_digit_sum_2.png?raw=true)<br /><br />

<h4>풀이 및 답</h4>
처음 constraints를 제대로 살피지 못하고 무작정 문제 정의와 예시의 함수 호출 설명 그대로 작성한 로직은 아래와 같았다.
```  
#!/bin/python3
def super_digit(p):
    if len(p) == 1:
        return p
    if '+' in p:
        return super_digit(eval(p))
    return super_digit('+'.join(p))

# read input
n, k = map(int, input().split())

# create P
P = ''
for i in range(k):
    P += n

# print output
print(super_digit(P))
```
하지만 아래와 같은 Error가 발생했고,<br/>
RecursionError: maximum recursion depth exceeded during compilation<br/>
단순히 constraints를 잘 못 확인해서 그런 것이라고 무작정 생각해서 k의 최대 limit 값 만큼 recursion함수 호출이 되도록 아래의 코드를 상단에 추가했다.
```
import sys
sys.setrecursionlimit(100000)
```
하지만 아래의 오류가 다시 발생했고,<br/>
세그멘테이션 오류 (core dumped)<br/>
또 다시 깊은 생각없이 무작정 limit을 늘려도 recursion함수 호출 수가 많다고 간단히 생각해서 이를 줄이려 아래와 같이 함수 refactor를 시도했다.
```
def super_digit(p):
    p = str(p)
    if len(p) == 1:
        return p
    return super_digit(eval('+'.join(p)))
```
하지만 계속해서 같은 세그멘테이션 오류가 발생했고,<br/>
이제서야 문제를 다시 보며 깊게 생각해본 결과 문제의 constraints를 이해한 부분에서 논리적 오류를 발견했다.<br/>
n이 k번 concat될 수 있는 것이므로, 생성되는 P의 자릿수는 무려 최대 10^100000 * 10^5 = 10^100005 였다.<br/>
어떤 한 자리 숫자 뒤에 0이 100005개 붙을 수 있다는 의미로, 최대 100006 자릿수라는 어마어마한 숫자가 생성된다.<br/>
constraints를 다시 보면서 깨달은 것은 n만으로 이미 충분히 큰 숫자라는 사실이다. 때문에, 세그멘테이션 오류의 원인이 recursion함수 호출 숫자와 별개라고 자연스럽게 알게됐다.<br/><br/>

이 시점에서 해결해야할 것은 크게 두 가지라고 생각했다.<br/>
1. 큰 숫자에 대한 효율적인 처리<br/>
2. recursion함수 호출 숫자 감소<br/>

이를 처리한 최종 제출 성공한 답은 아래와 같다.
```
#!/bin/python3
import sys
sys.setrecursionlimit(200000)


def super_digit(p):
    p = str(p)
    if len(p) == 1:
        return p
    return super_digit(eval('+'.join(p)))

# read input
n, k = map(int, input().split())

super_digit_of_n = int(super_digit(n))

# print output
print(super_digit(super_digit_of_n * k))
```
n을 concat하여 거대한 P를 만들어서 처리하기보다는 concat하기 전의 n에 대해서 먼저 처리를 해보자는 나름의 divide & conquer를 컨셉으로 잡았다.<br/>
손으로 예시 케이스를 몇 번 확인해 보니 n의 super_digit을 먼저 구한 후 이를 k번 concat하여 처리하는 것도 같은 결과를 갖는다는 것을 알게됐다.<br/>
concat한 것들은 recusion함수의 join으로 인해 문자 사이사이에 '+'가 추가되고 eval될 것이므로,<br/>
결국 n을 k번 더하는 것과 같은 결과가 되고, 이는 즉, n * k이다. 이 결과를 다시 recusion함수의 인자로 호출하여 최종 결과를 출력했다.<br/>
상단의 코드를 보면 알겠지만, 해결해야할 것의 2번은 처리가 전혀 안됐다는 점을 확인 할 수 있다.

물론, 언제나 그렇듯이 내가 생각해서 만들어낸 코드와 HackerRank의 discussion 탭의 다른 유저들이 수학적인 방법으로 해결한 것을 비교해보면 눈물만 나오지만..ㅠㅠ<br/>
오늘도 그냥 제출한 것이 constraints를 만족하며 pass됐다는 것에 의의를 둔다.