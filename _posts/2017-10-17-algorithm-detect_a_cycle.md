---
layout: post
title: Detect a Cycle
subtitle: from HackerRank
category: algorithm
tags: [algorithm, linked lists]
---
<h4>문제</h4>
![problem](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-10-17-algorithm-detect_a_cycle_1.png)<br/><br/>
![problem](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-10-17-algorithm-detect_a_cycle_2.png)<br/><br/>

<h4>풀이 및 답</h4>
```
def has_cycle(head):
    if head is None:
        return False
    
    node_pool = []
    node = head
    while node:
        if node in node_pool:
            return True
        node_pool.append(node)
        node = node.next
    return False
```