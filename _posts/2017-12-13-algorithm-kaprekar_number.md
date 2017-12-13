---
layout: post
title: Kaprekar Number
subtitle: from PB
category: algorithm
tags: [algorithm]
---
<h4>문제</h4>

**Note:** 문제 이름은 임의로 작성한 것 입니다.<br/>
Problem 1.<br/>
In 1949 the Indian mathematician D.R. Kaprekar discovered a class of numbers called self-numbers.<br/>
For any positive integer n, define d(n) to be n plus the sum of the digits of n.<br/>
For example, d(75) = 75 + 7 + 5 = 87.<br/>
The number n is called a generator of d(n).<br/>
Some numbers have more than one generator: for example, 101 has two generators, 91 and 100.<br/>
A number with no generators is a self-number.<br/>
There are thirteen self-numbers less than 100: 1, 3, 5, 7, 9, 20, 31, 42, 53, 64, 75, 86, and 97.<br/>
<br/>
**\- Write a program to sum of all self-numbers which are bigger than 0 and smaller than 5000.**<br/>

<h4>풀이 및 답</h4>

문제를 보고, 일단은 아래와 같은 코드를 작성해서 Kaprekar Number에 대해서 파악하려고 시도했다.
```python
def kaprekar_number(n):
    return n + sum([int(d) for d in str(n)])


input_numbers = range(0, 21)

for n in input_numbers:
    print('input: {:<5}, output {:<5}'.format(n, kaprekar_number(n)))
    if n % 10 == 0:
        print('=============================================================')
```
아웃풋은, 이랬다.
```
input: 0    , output 0
=============================================================
input: 1    , output 2
input: 2    , output 4
input: 3    , output 6
input: 4    , output 8
input: 5    , output 10
input: 6    , output 12
input: 7    , output 14
input: 8    , output 16
input: 9    , output 18
input: 10   , output 11
=============================================================
input: 11   , output 13
input: 12   , output 15
input: 13   , output 17
input: 14   , output 19
input: 15   , output 21
input: 16   , output 23
input: 17   , output 25
input: 18   , output 27
input: 19   , output 29
input: 20   , output 22
=============================================================
```
음... if문 조건을 아래와 같이 약간 바꿔보니, 훨씬 output이 훨씬 깔끔해졌다.
```python
for n in input_numbers:
    print('input: {:<5}, output {:<5}'.format(n, kaprekar_number(n)))
    # if n % 10 == 0:
    if str(n)[-1] == '9':
        print('=============================================================')
```
```
input: 0    , output 0
input: 1    , output 2
input: 2    , output 4
input: 3    , output 6
input: 4    , output 8
input: 5    , output 10
input: 6    , output 12
input: 7    , output 14
input: 8    , output 16
input: 9    , output 18
=============================================================
input: 10   , output 11
input: 11   , output 13
input: 12   , output 15
input: 13   , output 17
input: 14   , output 19
input: 15   , output 21
input: 16   , output 23
input: 17   , output 25
input: 18   , output 27
input: 19   , output 29
=============================================================
input: 20   , output 22
```
오케이, 이제 Kaprekar Number에 대해서 파악했으니 머리를 써서 코드를 짜보자!.. 라고 생각하다가,
아~ 일단 brute-force하게 먼저 문제에 맞게 해보자라고 생각해서 그냥 일단 아래와 같이 단순하게 코딩했다.
```python
def kaprekar_number(n):
    return n + sum([int(d) for d in str(n)])


def get_self_numbers(input_range):
    kaprekar_number_set = set()
    for n in input_range:
        kaprekar_number_set |= {kaprekar_number(n)}
    return set(input_range) - kaprekar_number_set


def solution(range_min, range_max):
    # init range to based on constraint of problem 1,
    # to satisfy bigger than min, smaller than max.
    # e.g. if range_min is 0, range_max = 10 then,
    # list(range(range_min + 1, range_max)) is [1, 2, 3, 4, 5, 6, 7, 8, 9]
    input_range = range(range_min + 1, range_max)

    # print(sorted(get_self_numbers(input_range)))
    return sum(get_self_numbers(input_range))


print(solution(0, 5000))
```
코드는 잘 동작해서 제출했다. 물론, 퍼포먼스에 문제가 있거나 동작중에 문제가 있었다면 짚고 넘어갔겠지만,
이 문제는 코딩테스트 중 1번문제였기에 잘 동작하는 것만 확인하고 바로 제출했다.<br/><br/>
다만, 추후 면접의 진행과정에서 면접관이 이 소스의 대해서 time complexity를 말해보라는 질문을 받았다.<br/>
소스를 찬찬히 살펴보니 O(n)이 었다. ~~사실 문제는 n이 5000이라는 상수여서 상수라고 말하고 싶었지만~~<br/>
하지만 set이라는 자료구조가 없을 때, 어떻게 해결할 것이냐에 대한 것과 그 때의 Big-O는 어떻게 될 것 같냐는 추가질문을 받고 다시 생각해봤다.
아마 python을 안쓰시는 분이라서 하시는 질문 같았다.<br/>
우선, 처음 시도했던 것 처럼 Kaprekar Number의 성질을 이용한 방법을 이용해서 기똥찬 로직을 뽑아내는 것이 가장 베스트일 것이다.<br/>
하지만 일단 이 로직에서 set만 대체한다면, 대충 아래와 같은 수도코드로 다시 구현할 수 있을 것이다.
```python
def get_self_numbers(input_range):
    # 1. get kaprekar_numbers from input_range
    kaprekar_numbers = []
    for n in input_range:
        kaprekar_numbers.append(kaprekar_number(n))

    # 2. sort kaprekar_numbers
    # 3. remove duplicates in kaprekar_numbers
    # 4. find each of kaprekar numbers in input_range and sum of them.
```
우선, 1번 과정에서는 이전과 같이 input_range만큼 반복하므로 마찬가지로 **O(n)**,<br/>
2번은 sorting이니, best와 worst가 어떤 정렬이냐에 따라 많이 차이나겠지만 가장 좋은 **O(nlogn)**이라 가정하고,<br/>
3번은 좋은 중복제거 알고리즘이 있겠지만, 현재 반복문 한번 도는 것 밖에 모르겠어서 **O(n)**,<br/>
4번은 input_range에서 search하는 것이므로 가장 빠른 b-search라 생각하면 O(logn)인데, 이를
kaprekar numbers의 갯수만큼 반복해야 하므로, worst로 **O(nlogn)**이라 생각된다.<br/>
따라서, 전체 time complexity를 worst로 **O(n) + O(nlogn) + O(n) + O(nlogn) = O(nlogn)**이라고 답변했다.<br/>
답변과 별개로, python의 set과 같은 pythonic함은 정말 대단한 것이라고 한번 더 생각하게됐고,
그 이전 소스인 set을 그대로 썼을 경우의 time complexity는 아래 링크에 의하면 O(n)이다.<br/>
참고: [How-is-set-implemented-internally-in-python](https://www.quora.com/How-is-set-implemented-internally-in-python)
