---
title: "[Algorithm] 아기 상어"
excerpt: "백준 문제 16236"
categories:
  - algorithm
tags:
  - algorithm
  - implementation
  - bfs
---

## Problem Summary

[백준 링크](https://www.acmicpc.net/problem/16236)

BFS로 최소 거리 찾으면서 더 이상 반복 못할 때 까지 계속 iterate 하면 되는 문제.

어떤 식으로 답을 찾아야 할지는 어렵지 않으나 자잘한 조건들 때문에 구현상 쉽게 실수 할 수 있는 문제이다.

## What I Learned

초반에 문제를 제대로 읽지 않아 어이 없게 많이 틀렸다.. ( 아기 상어 초기 크기가 2인데 1로 설정했다거나,,, )

또한, 물고기들은 그래프 상 1 ~ 6 으로 표현되고 아기 상어의 초기 위치는 9로 표현 되는데 구현 상 조건문을 1, 6, 그리고 아기 상어의 현재 크기를 기준으로 작성했기 때문에 아기 상어의 크기가 9보다 커지는 경우 9를 물고기로 처리해서 틀린 경우도 있었다 ( 초기 위치부터 그래프상 '0' 처리 해줘야 함 ).

> 최종 풀이 코드

```python
from collections import deque

dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

def bfs(graph, x, y, size, visited):
    q = deque([(x, y)])
    visited[x][y] = True
    res = []
    count = -1
    while q:
        count += 1
        q_len = len(q)
        for _ in range(q_len):
            nowx, nowy = q.popleft()
            if size < graph[nowx][nowy] <= 6:
                continue
            elif 1<= graph[nowx][nowy] < size:
                res.append((nowx, nowy))
                continue
            for i in range(4):
                nx = nowx + dx[i]
                ny = nowy + dy[i]
                if 0 <= nx < len(graph) and 0 <= ny < len(graph):
                    if not visited[nx][ny]:
                        q.append((nx, ny))
                        visited[nx][ny] = True

        if len(res) != 0:
            res.sort()
            return (res[0], count)
    return (res, -1)

n = int(input())
graph = []
visited = [[False] * n for _ in range(n)]
baby_x = 0
baby_y = 0
baby_size = 2
found_baby = False

for x in range(n):
    row = list(map(int, input().split()))
    graph.append(row)
    if not found_baby:
        for y in range(n):
            if row[y] == 9:
                baby_x = x
                baby_y = y
                found_baby = True

time = 0
ate = 0

graph[baby_x][baby_y] = 0
result, count = bfs(graph, baby_x, baby_y, baby_size, visited)

while count != -1:
    ate += 1
    if baby_size == ate:
        baby_size += 1
        ate = 0
    time += count
    baby_x, baby_y = result
    graph[baby_x][baby_y] = 0
    visited = [[False] * n for _ in range(n)]
    result, count = bfs(graph, baby_x, baby_y, baby_size, visited)

print(time)
```

책에서는 전체 과정을 2, 3개의 함수로 쪼개서 풀이를 했다.

BFS로 먹을 수 있는 최단 지점 물고기를 바로 구하는 것이 아니라 distance 배열을 만든 다음에 distance 배열로 가장 가까운, 먹을 수 있는 물고기 위치와 거리를 구하는 식으로 구현을 했다.

구현이 너무 복잡한 경우 이런 식으로 다른 함수로 쪼개서 푸는 방법도 고려를 해야한다.

또한, 내 풀이의 마지막 iterarion을 `while count != -1` 로 하지 말고 `while True` 로 해서 반복문 탈출해야할 순간에 `break`를 통해 끝내는 방식을 썼으면 더 직관적이었을 듯하다.
