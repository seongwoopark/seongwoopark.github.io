---
layout: post
title: Counter game
subtitle: from HackerRank
category: problemsolving
tags: [problemsolving, bit manipulation]
---
<h4>문제</h4>
![problem](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-07-06-problemsolving-counter_game_1.png?raw=true)<br/><br/>
![problem](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-07-06-problemsolving-counter_game_2.png?raw=true)<br/><br/>

<h4>풀이 및 답</h4>
문제에서 정의하는 operations를 생각해보면 결국 아래와 같아진다는 것을 알았다.
- N보다 작으면서 가장 큰 2의 제곱을 뺀다.

이를 적용하여 로직을 작성했다.
```
#!/bin/python3

# read the number of testcases
T = int(input())

# iterate for T
for i in range(T):
    # read the initial number set in the counter
    N = int(input())

    # initial result of winner
    winner_is_louise = True

    while N != 1:
        # initial value is 2^0
        power_of_two = 1

        # get largest power of 2 and it must be less than N
        while N > power_of_two << 1:
            power_of_two = power_of_two << 1

        # reduce counter set
        N -= power_of_two

        # escape condition
        if N == 1:
            break

        # overturn result of winner
        winner_is_louise = not winner_is_louise

    # print output
    print('Louise' if winner_is_louise else 'Richard')
```