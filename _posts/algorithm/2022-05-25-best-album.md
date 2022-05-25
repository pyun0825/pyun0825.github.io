---
title: "[Algorithm] 베스트앨범"
excerpt: "프로그래머스 고득점 kit"
categories:
  - algorithm
tags:
  - algorithm
  - hash
---

## Problem Summary

[프로그래머스 링크](https://programmers.co.kr/learn/courses/30/lessons/42578)

문제에서 주어진 대로 장르별 재생된 총합으로 정렬하고 장르별로는 재생수, 고유번호 기준으로 정렬하면 풀 수 있는 문제이다.

주어지는 input들과 index를 어떻게 묶어서 필요한 자료구조를 만들지가 관건인 문제이다.

## What I Learned

> 최종 코드

```python
from collections import Counter

def solution(genres, plays):
    answer = []
    genre_dict = {}
    for genre in set(genres):
        genre_dict[genre] = []

    for i in range(len(genres)):
        genre_dict[genres[i]].append((plays[i], i))

    genre_list = list(genre_dict.values())

    genre_list.sort(key=lambda x: sum([i[0] for i in x]), reverse=True)
    for group in genre_list:
        group.sort(key=lambda x: (-x[0], x[1]))
        count = 0
        for i in group:
            answer.append(i[1])
            count += 1
            if count == 2:
                break

    return answer

```

Dictionary를 통해 장르별로 (재생수, 고유번호) 튜플을 묶었다.

장르 명은 중요하지 않기 때문에 dictionary에서 values만 추출했고, 이를 정렬하는 과정들을 거쳐 답을 구했다.

처음 dictionary를 장르별로 빈 list로 초기화하는 것이 귀찮았는데 dictionary도 list comprehension 처럼 dictionary comprehensiondmf 통해 쉽게 초기화 할 수 있었다.

```python
d = {e:[] for e in set(genres)}
```

또한, 나는 iteration을 직접 for문으로 돌면서 genres와 play list, 그리고 index를 묶었는데, 이를 `zip 함수`를 이용하면 더욱 쉽게 처리 할 수 있다.

> [zip 함수 공식 문서](https://docs.python.org/3/library/functions.html#zip)
