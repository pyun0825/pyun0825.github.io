---
title: "[Algorithm] 완주하지 못한 선수"
excerpt: "프로그래머스 고득점 kit"
categories:
  - algorithm
tags:
  - algorithm
  - hash
---

## Problem Summary

[프로그래머스 링크](https://programmers.co.kr/learn/courses/30/lessons/42576)

Completion list에 없는 participant list 중 한명을 찾아내는 문제.

완전 탐색 시 Participant list 크기가 up to 100,000명 이기 때문에 시간 초과가 난다.

따라서 hash를 쓰는 자료구조인 set 혹은 dictionary를 사용해 문제를 풀어야 한다.

허나 동명이인이 존재할 수 있기 때문에 이를 고려해서 문제를 풀어야..!

## What I Learned

> 최종 코드

```python
from collections import Counter

def solution(participant, completion):
    completion = Counter(completion)
    for person in participant:
        if completion[person] != 0:
            completion[person] -= 1
        else:
            return person
```

Set을 쓰게 되면 동명이인들이 다 제거된다.

"Kim"이 participant 에 3명, completion에 2명 있으면 답이 "Kim"이 되어야 하는데 set을 쓰게 되면 participant, completion 둘 다 "Kim"은 한 명밖에 존재하지 않는 것으로 간주하기 때문에 답을 찾을 수 없다.

따라서 사용한 자료구조는 multiset이랑 비슷한, dictionary 기반의 Counter이다.

Counter는 input으로 들어온 iterable에 대해 각 element를 key, 해당 element의 등장 횟수 를 value로 하는 dictionary를 반환한다.

따라서 completion list를 Counter를 이용해 dictionary 형태로 변환시켜 주고, participant에 있는 참가자마다 dictionary의 value가 0인지, 아니면 1만큼 감소시키는 방식으로 풀이를 했다.

> [Counter 공식 문서](https://docs.python.org/dev/library/collections.html#collections.Counter)

> Counter는 Set처럼 `-`, `+` 연산들도 지원하므로 이를 이용해 풀 수도 있다.
