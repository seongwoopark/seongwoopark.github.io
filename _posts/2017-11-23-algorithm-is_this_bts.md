---
layout: post
title: Trees, Is This a Binary Search Tree?
subtitle: from HackerRank
category: algorithm
tags: [algorithm, Binary Search Tree, BTS]
---
<h4>문제</h4>
![problem](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-11-22-algorithm-bfs_shortest_reach_1.png)<br/><br/>
![problem](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-11-22-algorithm-bfs_shortest_reach_2.png)<br/><br/>
![problem](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-11-22-algorithm-bfs_shortest_reach_3.png)<br/><br/>

<h4>풀이 및 답</h4>
처음에는 shortest path하길래 무조건 다익스트라 알고리즘을 쓰면 되겠거니 했다. 주입식교육이란.. 그런데 문제의 BFS를 보고나서 다익스트라 말고, BFS 방법으로
풀어보려고 시도했다.
```python
""" Node is defined as
class node:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None
"""

def checkBST(root):
    if root is None:
        return True
    
    left_check_result = False
    if root.left is None or root.left.data < root.data:
        left_check_result = True
    right_check_result = False
    if root.right is None or root.right.data > root.data:
        right_check_result = True
        
    return all([left_check_result, right_check_result, checkBST(root.left), checkBST(root.right)])
```