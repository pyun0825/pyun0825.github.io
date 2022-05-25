---
title: "[Algorithm] 위장"
excerpt: "프로그래머스 고득점 kit"
categories:
  - algorithm
tags:
  - algorithm
  - hash
---

## Problem Summary

[프로그래머스 링크](https://programmers.co.kr/learn/courses/30/lessons/42578)

각 의상 종류별로 최대 하나씩 입을 수 있을 때 서로 다른 옷의 조합 수를 찾는 문제이다.

의상을 무조건 하나는 입어야 한다.

한 종류만 입는 경우의 수 + 두 종류를 입는 경우의 수 + ... + n 종류를 입는 경우의 수 를 구하면 된다.

## What I Learned

> 최종 코드

```python
from collections import Counter

def solution(clothes):
    grouped = Counter([x[1] for x in clothes])
    nums = list(grouped.values())
    res = 1
    for num in nums:
        res *= (num + 1)

    return res - 1
```

처음에 `clothes`를 종류별 옷의 갯수를 나타내는 list로 잘 변환하기만 하면 쉬운 수학 문제이다.

각 옷 종류별로 입을 수 있는 옷의 수에 해당 종류를 입지 않는 경우를 +1 해줘서 모두 곱한 뒤, 마지막에 모든 종류를 안입는 경우 를 -1 해주면 되는 문제다.

그러나 그 단순한 수학 경우의 수 계산을 떠올리지 못해 많이 헤맨 문제이다..
