---
title: "[Algorithm] 조합"
excerpt: "백준 문제 2407"
categories:
  - algorithm
tags:
  - algorithm
  - math
---

[백준 링크](https://www.acmicpc.net/problem/2407)

n과 m이 주어지면 nCm을 출력해야하는 간단한 문제.

푸는 로직은 정말 간단명료 했기 때문에 틀릴 이유가 없었으나 python의 특성을 몰라 초반에 삽질을 했었다.

> 초반 코드

```python
n, m = map(int, input().split())

div_1, div_2 = 1, 1

for i in range(m):
    div_1 =  div_1 * (n-i)
    div_2 = div_2 * (i+1)

print(int(div_1/div_2))
```

무엇이 문제였을까?

문제는 파이썬의 나누기 연산자 `/`에 있었다.

파이썬에서 `/`연산은 float형을 반환한다. Float 형은 큰 수 저장에 한계가 있다. `div_1`, `div_2`가 큰 수일 경우, 반환 값을 바로 int로 type 변환을 시킨다고 해도 이미 float형으로 반환이 되면서 데이터가 truncate 되기 때문에 정답이 틀리는 test case가 나왔던 것이다.

따라서 큰 수를 다르는 경우엔 `int(a/b)`연산이 아닌, int형을 반환하는 `//` 연산을 통해 문제를 해결해야 한다.

> 최종 코드

```python
n, m = map(int, input().split())

div_1, div_2 = 1, 1

for i in range(m):
    div_1 =  div_1 * (n-i)
    div_2 = div_2 * (i+1)

print((div_1//div_2))
```
