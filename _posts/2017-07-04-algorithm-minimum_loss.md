---
layout: post
title: Minimum Loss
subtitle: from HackerRank
category: algorithm
tags: [algorithm, search]
---

<h4>문제</h4>
![problem]https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-07-04-algorithm-minimum_loss_1.png?raw=true)<br /><br />
![problem]https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-07-04-algorithm-minimum_loss_2.png?raw=true)<br /><br />

<h4>풀이 및 답</h4>
인풋 값을 입력 받고, 문제 정의에 의해서 price list의 인덱스가 year가 되기에 price와 인덱스 값을 year로 갖는 tuple의 list를 생성한다.<br />
이후, price를 기준으로 sort하고, loss를 구한다. 문제 정의에 의해서 valid loss를 구해야 하기 때문에 year가 작은 값 에서 큰 값을 빼야한다.
(year가 시간의 흐름이기 때문에 과거의 팔고 미래에 산 경우를 배제하는 것)
또한 loss가 음수인 경우도 제외한 후, loss의 list에서 minimum을 출력한다.

```
#!/bin/python3
from operator import itemgetter

# read input
n = int(input())
prices = list(map(int, input().split()))

# sort
prices_and_years = [(prices[i], i) for i in range(n)]  # year is equal to index
sorted_prices_and_years = sorted(prices_and_years, key=itemgetter(0))

# get valid losses
valid_losses = []
prev_price, prev_year = sorted_prices_and_years[0]
for i in range(1, n):
    price, year = sorted_prices_and_years[i]
    loss = price - prev_price if year < prev_year else prev_price - price
    if loss > 0:
        valid_losses.append(loss)
    prev_price, prev_year = price, year

# print output
print(min(valid_losses))
```