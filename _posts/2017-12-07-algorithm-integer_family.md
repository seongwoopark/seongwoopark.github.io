---
layout: post
title: Integer Family
subtitle: from Codility
category: algorithm
tags: [algorithm]
---
<h4>문제</h4>
**Note:** 문제 이름은 임의로 작성한 것 입니다.<br/>
![problem](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-12-07-algorithm-integer_family_1.png)<br/><br/>

<h4>풀이 및 답</h4>
python 인터프리터로 테스트하다보니 아래처럼 한 라인으로 해결이 되었다. 
```python
def solution(N):
    return int(''.join(sorted(str(N), reverse=True)))
```
그런데, 코딩테스트임을 다시 인지하고 그래도 조금은 나눠서 쓰는게 나을 것 같아서 아래처럼 바꿔서 제출했다.
int N을 str으로 변환한 이유는 N의 각 자리수를 분리하여 정렬하기 위함이다.
```python
def solution(N):
    stringified = str(N)
    descending_sorted = sorted(stringified, reverse=True)
    reassembled = ''.join(descending_sorted)
    return int(reassembled)
```