---
layout: post
title: Gunmen in the Room
subtitle: from PB
category: problemsolving
tags: [problemsolving]
---
<h4>문제</h4>

**Note:** 문제 이름은 임의로 작성한 것 입니다.<br/>
Problem 2.<br/>
There is a room with n rows and n columns, and there are gunmen in the room.<br/>
A gunman can see vertical or horizontal way, and cannot see over the wall or diagonal.<br/>
If a gunman see another gunman, they will shoot each other and only one man will survive.<br/>
We want all gunmen to be alive. Let’s assume we mark each object as below.<br/>
■: Wall
□: Empty space
♂: Gunman<br/><br/>
For example, there is a room with 4 rows and 4 columns,<br/>
■■■□<br/>
□□□□<br/>
□■□□<br/>
■■■□<br/>

we can place 3 gunmen like below.<br/>
■■■□<br/>
□□□♂<br/>
♂■♂□<br/>
■■■□<br/>

However, we cannot place gunmen like below, because they are in the same line horizontally, one man will die.<br/>
■■■□<br/>
□♂□♂<br/>
□■□□<br/>
■■■□<br/>

We can place maximum 4 gunmen in this room, and there are 2 ways to achieve this.<br/>
■■■♂ &nbsp;&nbsp; ■■■□<br/>
□♂□□ &nbsp;&nbsp; □♂□□<br/>
♂■♂□ &nbsp;&nbsp; ♂■♂□<br/>
■■■□ &nbsp;&nbsp; ■■■♂<br/>

**\- Write a program to calculate the maximum number of gunmen we can place in the room below.**<br/>
□■□■□■□■<br/>
□□□□□■□□<br/>
■□■□□■□■<br/>
□□□□□□□□<br/>
□□□■□□□□<br/>
□□□□□■□■<br/>
□■□□□□□□<br/>
□□□□■□■□<br/>

<h4>풀이 및 답</h4>

이 문제를 보고, 일단 바로 CS의 알고리즘 수업에서 hello world처럼 다루는, N-queens문제의 변형이라는 것을 알았다.
문제는 내 머리가 리셋되어 있다는 점이었다.~~하아....~~<br/>
전혀 알고리즘 기법이 생각이 안나고, 문득문득 이전에 stack과 recursion을 이용해서 과제를 했던 기억이 있었다.~~이때 처음부터 제대로 복습했어야 했다~~<br/>
그냥 지금 상태로 해결해보자는 생각으로 달려들어서 굉장히 brute-force하게, 무식하게 코드가 작성됐다.
```python
import copy

max_num_gunmen = 0
len_board = 0


def place_a_gunman(position, board):
    global len_board
    # place a gunman
    row, col = position
    board[row][col] = 1

    # fill the shooting range as the wall
    for delta in range(1, len_board):
        # direction of positive row
        if row + delta < len_board:
            if board[row + delta][col] == 1:
                break
            board[row + delta][col] = 1
    for delta in range(1, len_board):
        # direction of negative row
        if row - delta > 0:
            if board[row - delta][col] == 1:
                break
            board[row - delta][col] = 1
    for delta in range(1, len_board):
        # direction of positive col
        if col + delta < len_board:
            if board[row][col + delta] == 1:
                break
            board[row][col + delta] = 1
    for delta in range(1, len_board):
        # direction of positive col
        if col - delta > 0:
            if board[row][col - delta] == 1:
                break
            board[row][col - delta] = 1


def recursion(board, num_gunmen, board_stack):
    global max_num_gunmen
    global len_board

    for i in range(len_board):
        for j in range(len_board):
            if board[i][j] == 1:
                continue
            # store gunman position and board before place a gunman
            board_stack.append(((i, j), copy.deepcopy(board)))
            # place a gunman
            place_a_gunman((i, j), board)
            num_gunmen += 1
            max_num_gunmen = max(max_num_gunmen, num_gunmen)
    else:
        if not board_stack:
            return

        if all([all(row) for row in board]):
            # backtracking
            (i, j), board = board_stack.pop()
            board[i][j] = 1
            num_gunmen -= 1
            recursion(board, num_gunmen, board_stack)


def solution(board):
    global len_board

    for i in range(len_board):
        for j in range(len_board):
            if board[i][j] == 1:
                continue
            recursion(copy.deepcopy(board), 0, [])    # copy of board, number of gunmen, empty stack
            board[i][j] = 1    # mark as the wall


room = "\
□■□■\n\
■□■□\n\
□■□■\n\
■□■□\
"

# change room to gunmen_board
room = room.replace('□', '0').replace('■', '1').splitlines()
gunmen_board = []
for row in room:
    gunmen_board.append([int(cell) for cell in row])

# assert board length
assert len(gunmen_board) == len(gunmen_board[0])
len_board = len(gunmen_board)

solution(gunmen_board)
print(max_num_gunmen)

```
그렇다, 코드를 보면 알겠지만 문제의 room이 아닌, 4x4사이즈의 room이다.<br/>
그렇다, 너무 구려서 stack overflow로 인한 MemoryException과 recursive call limit으로 인한
RecursionError가 발생한다.<br/>
다행스럽게도(?) 4x4 room에 대해서는 제대로 동작하는 듯이 보인다.<br/>
면접과정에서는 시간이 부족해서 이런 부끄러운 코드를 제출할 수 밖에 없었다.
운이좋았던 것은 다른 회사들처럼 문제풀이 사이트들을 사용했다면, 0점과 마찬가지였겠지만,
직접 사람이 읽고 판단하는 과정이 진행되어서 점수 배점이 10점이라면 2.5점은 받지 않았을까 생각해본다.<br/>

아래 링크를 참조하여, 알고리즘 복습 후 다시 소스코드를 작성해보려 한다.<br/>
참고: [n-queens-algorithm](http://sooyoung32.github.io/dev/2016/03/14/n-queen-algorithm.html)
