---
layout: post
title: 홀수인 숫자들의 제곱의 합
subtitle: from 로켓펀치 채용공고
category: algorithm
tags: [algorithm, strings]
---
<br/><h4>문제</h4>
주어진 문자열에서 홀수인 숫자들의 제곱의 합을 출력한다.<br/>
예) "ab2v9bc13j5jf4jv21" -> 9^2 + 13^2 + 5^2 + 21^2 = 716
<br/>출처 - [로켓펀치 채용공고](https://www.rocketpunch.com/jobs/32429/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EA%B0%9C%EB%B0%9C%EC%9E%90%EC%8B%A0%EC%9E%85)

<br/><h4>풀이 및 답</h4>
개발팀 내에서 간단한 문제가 공유되어서 문제를 풀어보았다. regex를 사용하려 했지만 자체 constrains를 둬서 regex를 사용하지 않기로 했다.
 내가 풀이한 방법은 아래와 같다.
```python
sum = 0
digit = '0'
s = 'ab2v9bc13j5jf4jv21'
for ch in s:
    if ch.isdigit():
        digit += ch
    else:
        if int(digit) & 1:
            sum += int(digit) * int(digit)
        digit = '0'
sum += int(digit) * int(digit)
print(sum)
```
하지만 개발팀 내의 다른 분은 아주 간단하게 작성하셨다.
```python
import re
REG = re.compile(r'\d+')
s = 'ab2v9bc13j5jf4jv21'
sum(int(match.group())*int(match.group()) for match in REG.finditer(s) if int(match.group())&1)
```
regex constraints 이후,
```python
s = 'ab2v9bc13j5jf4jv21'
s_spaced = ''.join(c if c.isnumeric() else ' ' for c in s)
sum(int(d)*int(d) for d in s_spaced.split() if int(d)&1)
```
심지어 js 버전도 작성, 한줄짜리 코드가 완성해주셨다.
```javascript
 s.split('').map(c => isNaN(+c)? ' ': c).join('').split(' ').filter(c => +c > 0 && c&1).reduce((a,b) => (+a)+(+b)*(+b), 0)
```

오늘도 보람차게 자존감이 깎이는 하루인 것 같다.