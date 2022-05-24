---
title: "[Algorithm] 청소년 상어"
excerpt: "백준 문제 19236"
categories:
  - algorithm
tags:
  - algorithm
  - implementation
  - backtracking
---

## Problem Summary

[백준 링크](https://www.acmicpc.net/problem/19236)

상어가 물고기를 먹는 모든 경우의 수를 따져가며 완전 탐색을 통해 최댓값을 구해야 하는 문제이다.

아기 상어 문제와 동일하게 특정 자료구조, 풀이 방식을 생각하는 것보다 구현하기가 더 까다로운 문제이다.

완전탐색을 DFS를 통한 backtracking을 사용해 구현했다.

## What I Learned

> 최종 코드

```python
dx = [-1, -1, 0, 1, 1, 1, 0, -1]
dy = [0, -1, -1, -1, 0, 1, 1, 1]
# input 받을 때 -1 해줘야
max_fish = 0

def fish_move(graph, positions):
    for ind in range(1, 17):
        x, y = positions[ind]
        if x == -1 :
            continue # 먹힌 경우
        num, dir = graph[x][y]
        for i in range(8):
            nx = x + dx[(dir + i) % 8]
            ny = y + dy[(dir + i) % 8]
            if 0 <= nx < 4 and 0 <= ny < 4 and graph[nx][ny][0] != -1:
                change_num = graph[nx][ny][0]
                positions[num] = (nx, ny)
                positions[change_num] = (x, y)
                temp = graph[nx][ny]
                graph[nx][ny] = [num, (dir + i) % 8]
                graph[x][y] = temp
                break

def dfs(graph, x, y, count, positions):
    global max_fish
    num = graph[x][y][0]
    dir = graph[x][y][1]
    positions[num] = (-1,-1)
    graph[x][y] = (-1, dir)
    count += num
    max_fish = max(max_fish, count)
    fish_move(graph, positions)
    for i in range(1, 4):
        nx = x + dx[dir]*i
        ny = y + dy[dir]*i
        cpy_graph = [x[:] for x in graph]
        cpy_graph[x][y] = (0, 0)
        cpy_positions = positions[:]
        if 0 <= nx < 4 and 0 <= ny < 4 and cpy_graph[nx][ny][0] >= 1:
            dfs(cpy_graph, nx, ny, count, cpy_positions)
    return

positions = [(0,0)] * (17)

graph = [[] for _ in range(4)]

for x in range(4):
    row = list(map(int, input().split()))
    for i in range(0, 8, 2):
        graph[x].append((row[i], row[i+1]-1))
        positions[row[i]] = (x, i//2)

dfs(graph, 0, 0, 0, positions)
print(max_fish)
```

1번부터 16번까지의 물고기들이 존재하기 때문에 해당 물고기들의 위치를 `position` 이라는 list에 담아 관리를 했다.

시간상 내가 한 방식이 빠를 순 있겠지만 logic이 상당히 복잡해진다. 이런 작은 크기의 인접 배열 DFS는 O(V^2) 의 time complexity를 가져 오래 걸리지 않으므로 따로 find_position 등의 함수를 만들어 직관적으로 코드를 작성했으면 더 좋지 않았을까 싶다.

또한, backtracking 문제를 풀면서 항상 느끼는 고민인데 'back tracking' 을 할 때 이전 상태로 돌려 놓기 위해 copy 된 array를 사용하고, 특정 변수에 더해준 값을 다시 빼주고 등의 작업을 어느 시점에 할지가 고민이 된다.

내 풀이처럼 dfs 중간에 copy를 하는 것보다 그냥 dfs 처음부터 copy를 해서 코드를 짜는 것이 덜 헷깔리지 않을까 싶다.
