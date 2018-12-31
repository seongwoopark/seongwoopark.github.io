---
layout: post
title: File Iterator
subtitle: from Codility
category: problemsolving
tags: [problemsolving]
---
<h4>문제</h4>
**Note:** 문제 이름은 임의로 작성한 것 입니다. 아래는 문제의 전체가 아닌 일부 캡쳐입니다.<br/>
![problem](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-12-07-problemsolving-file_iterator_1.png)<br/><br/>
이어지는 내용:
```python
# your iterator should return the following sequence of integers:
[137, -104, 0, 1, 0, -1]
```

<h4>풀이 및 답</h4>
이 문제가 가장 찝찝하게 풀었던 문제이다.
문제에서 왜 class구현을 요구하는 지와 string처리가 메인인 문제에서 왜 굳이 File의 개념을 넣었지가 아직도 이해되지 않는다.<br/>
아무튼 문제가 시키는대로 하기위해 껍데기를 만들었다.
```python
class FileIterator(object):
    def __init__(self, f):
        self.valid_integers = []
        for line in f:
            pass

    def get_iterator(self):
        return self.valid_integers


def solution(file_object):
    iterator = FileIterator(file_object)
    return iterator.get_iterator()
```
이후, 다시 문제를 읽었다. 역시나 위의 요구사항들은 별 상관이 없다. 그냥 string처리 하는 것이 메인이다.
하나의 라인(\n으로 분리된) 안에서 valid integer의 정의는 아래와 같았다.
- 한 자릿수 이상의 숫자다.
- 단, leading zeros가 없어야 한다.
- 선택적으로, 숫자 앞에 + 또는 - sign이 한번만 붙을 수 있다.(선택적이므로 없어도 된다는 의미)
- 숫자의 범위는 -1,000,000,000 ~ 1,000,000,000 이다.
- 숫자 양옆에 white spaces(나는 margin이라 칭함)는 허용한다.
- 숫자 사이사이에 white spaces는 허용하지 않는다.
- integer이므로 당연히 정수만 허용한다.

위의 조건을 만족해야만 valid integer이고, 나머지는 invalid이다. invalid는 코멘트처리(무시)하고 valid한 값들만
리턴하는 것이 문제이다.<br/>
우선, python인터프리터로 그냥 int 형 변환이 얼마나 잘 되는지 문제에서 예제로 들어주는 string들에 대해서 테스트해봤다.
```python
# invalid cases based on problem definition
int('2u1')       # raise ValueError Exception
int('23.9')      # raise ValueError Exception
int('#12')       # raise ValueError Exception
int('00')      
int('++1')       # raise ValueError Exception
int('2000000000')
int('2 58')      # raise ValueError Exception
int('++3')       # raise ValueError Exception
int('five')      # raise ValueError Exception

# valid cases based on problem definition
int('137')      
int('-104')
int('+0')
int('+1')      
int('-0')
int('-1')
```
오... 기본 built-in 함수인 int 형 변환이 거의 다 해준다. ~~날로먹을 수 있겠다 라는 생각을 잠깐했다.~~
위의 테스트를 진행하고 나서, regex를 이용해서 white spaces, leading zeros조건을 판단하고나서 int 형 변환을 한 후에 range 조건만 체크하면 될거라 생각했다.
이를 로직으로 구현하기 위해 regex패턴 구현을 아래와 같이 시도했다.
```python
import re


class FileIterator(object):
    def __init__(self, f):
        self.valid_integers = []
        for line in f:
            if re.match("(\s*)([+-]?)([0-9]{1,10})(\s*)", line):
                try:
                    integer = int(line)
                    # range check
                    if  -1000000000 < integer < 1000000000:
                        self.valid_integers.append(integer)
                except:
                    pass
...
..
.
```
위의 코드에서 re.match는 하나의 string안에서 패턴이 계속 반복되어도 match object가 리턴되기 때문에 if문이 True처리 된다.
다시 말하면, 위의 regex패턴을 적용하면 " 12  34 "가 valid integer처리가 된다는 의미이다.
문제를 풀때는 침착하지 못해서 이를 strip() built-in 함수를 사용해서 해결하고 regex패턴을 바꿨는데, 사실 re.fullmatch를 사용하면 해결되는 문제였다.<br/>
그리고 이 패턴은 여전히 leading zeros를 처리하지 못했다. leading zeros를 처리하는 regex 패턴은 많은 시간 고민해도 생각나지 않아서,
tricky한 방법을 쓰기로 하여 다음과 같은 코드가 작성됐고, 이를 최종 제출하였다.
```python
class FileIterator(object):
    def __init__(self, f):
        self.valid_integers = []
        for line in f:
            line = line.strip()
            if re.fullmatch("([+-]?)([0-9]{1,10})", line):
                try:
                    integer = int(line)
                    # range check
                    range_check = -1000000000 < integer < 1000000000
                    # leading zeros check
                    removed_sign = re.sub("[+-]", "", line)
                    leading_zeros_check = removed_sign == str(int(removed_sign))
                    if range_check and leading_zeros_check:
                        self.valid_integers.append(integer)
                except:
                    pass
...
..
.
```
이 코드를 위의 문제 정의에 맞춰서 보면,
- 한 자릿수 이상의 숫자다. **(regex패턴이 처리)**
- 단, leading zeros가 없어야 한다. **(leading_zeros_check가 처리)**
- 선택적으로, 숫자 앞에 + 또는 - sign이 한번만 붙을 수 있다.(선택적이므로 없어도 된다는 의미) **(regex패턴이 처리)**
- 숫자의 범위는 -1,000,000,000 ~ 1,000,000,000 이다. **(range_check가 처리)**
- 숫자 양옆에 white spaces(나는 margin이라 칭함)는 허용한다. **(strip 내장함수 처리)**
- 숫자 사이사이에 white spaces는 허용하지 않는다. **(regex패턴이 처리)**
- integer이므로 당연히 정수만 허용한다. **(regex패턴, int 내장함수 처리)**

와 같이 처리됐다. 찜찜하지만 더 이상 테스트할 케이스가 생각나지 않아 제출하였고, 결과는 다음과 같았다.<br/>
![problem](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-12-07-problemsolving-file_iterator_2.png)<br/><br/>
task 3의 score를 보니 하나의 테스트 케이스를 통과하지 못한 것 같다. 역시 찜찜하긴 했는데, 어떤 케이스가 통과하지 못한 건지는 여전히 알 수 가 없다. 