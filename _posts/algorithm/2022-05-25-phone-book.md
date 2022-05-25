---
title: "[Algorithm] 전화번호 목록"
excerpt: "프로그래머스 고득점 kit"
categories:
  - algorithm
tags:
  - algorithm
  - hash
  - sorting
---

## Problem Summary

[프로그래머스 링크](https://programmers.co.kr/learn/courses/30/lessons/42577)

전화번호부에 적힌 전화번호 중 다른 전화번호의 접두어가 되는 전화번호가 있는지, 있으면 False를 반환해야하는 문제이다.

Input list의 길이가 최대 1,000,000 이므로 완전탐색이 아닌 다른 방식을 써야한다.

## What I Learned

> 최종 코드

```python
def solution(phone_book):
    phone_book.sort()
    for i in range(len(phone_book) - 1):
        if phone_book[i] == phone_book[i+1][:len(phone_book[i])]:
            return False
    return True
```

`phone_book`의 element들이 string이므로 `.sort()`시 가장 앞 글자부터 기준으로 작은 것 부터 정렬이 된다.

또한, String B가 String A + '~' 이하면 sort 시 A가 B의 앞에 오게 된다.

따라서 `phone_book` sorting 후 각 단어마다 바로 다음 단어에 자신이 포함 되는지만 확인하면 된다.

나는 앞 단어의 length를 기준으로 뒷 단어를 slicing 해서 비교를 했지만 이를 `.startswith()`으로 직관적으로 풀었을 수도 있다.

Hash 자료구조를 사용해 풀이를 할 수도 있다.

> Set을 사용해서 푼 코드

```python
def solution(phone_book):
    phone_book = set(phone_book)
    for tel in phone_book:
        temp = ""
        for chr in tel:
            temp += chr
            if temp in phone_book and temp != tel:
                return False
    return True
```
