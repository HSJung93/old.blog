---
title: "이중 리스트의 내부 영역 대각선 뒤집기"
date: 2021-10-10T13:15:00+09:00
categories:
  - 코드
tags:
  - 대각선 뒤집기
  - 이중리스트
header:
  teaser: /assets/images/code.jpg
---

이중 리스트의 내부 영역을 대각선으로 뒤집는 코드입니다.

## 문제

주어진 이중 리스트의 내부 영역을 대각선으로 뒤집은 이중 리스트 값을 출력하라.

## 입력

이중 리스트인 a, 이중 리스트의 내부 영역을 나타내는 2개의 좌표 값과 뒤집는 타입을 나타내는 리스트인 qs가 주어진다.

```
a =
[[1, 2, 3, 4, 5],
[6, 7, 8, 9, 10],
[11, 12, 13, 14, 15],
[16, 17, 18, 19, 20],
[21, 22, 23, 24, 25]]

qs = [ [2, 2, 5, 4, 1],  [2, 2, 5, 4, 1], [1, 3, 4, 5, -1]]
```

## 출력

qs의 쿼리들에 해당하는 뒤집기 작업을 완료한 이중 리스트를 리턴한다.

```
[[1, 2, 15, 10, 5],
[6, 7, 20, 9, 4],
[11, 12, 19, 14, 3],
[16, 17, 18, 13, 8],
[21, 22, 23, 24, 25]]
```

## 풀이

- defaultdict(list)안에 대각선에 해당하는 값들을 넣은 뒤, 뒤집는다.
- 해당 값을 좌표에 맞춰서 대각선에 차례대로 넣어준다.

```python
from collections import defaultdict

### 이중리스트 인풋 값 a 만들기
a = []

for i in range(1, 22, 5):
  b = []
  for j in range(5):
    b.append(i+j)

  a.append(b)

# for ar in a:
#   print(ar)

# print()

### 또다른 인풋 값인 qs
qs= [ [2, 2, 5, 4, 1],  [2, 2, 5, 4, 1], [1, 3, 4, 5, -1]]

### 타입 1 일때 뒤집는 함수
def flipA(r1, c1, r2, c2):

  global a

  d = defaultdict(list)
  for r in range(r1, r2+1):
    for c in range(c1, c2+1):
      d[r+c].append(a[r][c])

  indexs_list = []

  for c in range(c1, c2+1):
    indexs_list.append((r1, c))

  for r in range(r1+1, r2+1):
    indexs_list.append((r, c2))

  # 0 1 2 3 4 5
  for indexs, k in zip(indexs_list, d.keys()):
    r0, c0 = indexs
    for val in d[k][::-1]:
      a[r0][c0] = val
      r0 += 1
      c0 -= 1

### 타입 -1일 때 뒤집는 함수
def flipB(r1, c1, r2, c2):

  global a

  d = defaultdict(list)
  for r in range(r1, r2+1):
    for c in range(c2, c1-1, -1):
      d[r-c].append(a[r][c])

  indexs_list = []

  for c in range(c2, c1-1, -1):
    indexs_list.append((r1, c))

  for r in range(r1+1, r2+1):
    indexs_list.append((r, c1))

  # 0 1 2 3 4 5
  for indexs, k in zip(indexs_list, d.keys()):
    r0, c0 = indexs
    for val in d[k][::-1]:
      a[r0][c0] = val
      r0 += 1
      c0 += 1

### solution 함수
def solution(a, qs):

  for r1, c1, r2, c2, typ in qs:
    if typ == 1:
      flipA(r1-1, c1-1, r2-1, c2-1)
    else:
      flipB(r1-1, c1-1, r2-1, c2-1)

    # for ar in a:
    #   print(ar)
    # print()

  return a

solution(a, qs)
```
