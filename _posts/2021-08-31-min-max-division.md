---
title: "코딜리티 Lesson 14-1 MinMaxDivision 문제"
date: 2021-08-31T15:12:00+09:00
categories:
  - 코드
tag:
  - codility
  - 부분합
  - 이진탐색
header:
  teaser: /assets/images/code.jpg
---
코딜리티의 Lesson 14-1 문제로 리스트를 K로 나눌 때 각 리스트의 합들의 최소값을 구하는 문제이다.
* 무엇을 이진탐색으로 구할 것인가? 각 리스트들의 합들의 최소값을 이진탐색으로 구한다.
* 탐색할 구간은 무엇인가? 최소값은 너무 많은 리스트로 나눠야 할 때(`K > len(A)`), 한 원소의 최대값인 `max(A)`이며, 최대값은 리스트를 나누지 않을 때(`K == 1`), 리스트의 모든 값의 합인 `sum(A)`이다.
* 어떻게 구간을 반으로 줄여나갈 것인가?
  * 임의의 각 리스트들의 합들의 최소값 x이 크면, 각 리스트에 값이 많이 들어가야 해서 리스트를 K만큼 못 나눌 수 있다. 따라서 임의의 각 리스트들의 최소값이 문제의 조건을 충족하는지 여부를 확인하는 로직을 함수로 작성한다.
    * 주어진 배열 `A`에 대해서 각 리스트들의 최소값이 x일 때 k개 이상으로 나누어야 한다면, 문제의 조건을 충족하지 못하므로 각 리스트들의 최소값은 더 커야만 한다.
    * x 여도 k개만큼 나눌 수 있다는게 확인되면, 각 리스트들의 최소값은 더 작을수도 있다.
* 무엇을 리턴할 것인가?
  * while문의 조건과 조건의 충족을 고려하여 `mls_min`을 리턴한다. 

```python
def enoughDivideKWithMinimalX(A, K, x):
    blockSum = 0
    blockCount = 1

    for a in A:
        if blockSum + a > x:
            # start new block
            blockSum = a
            blockCount += 1
        else:
            blockSum += a
        if blockCount > K:
            return False
    return True

def solution(K, M, A):
    mls_min = max(A)
    mls_max = sum(A)

    if K >= len(A):
        return mls_min

    if K == 1:
        return mls_max

    while (mls_min <= mls_max):
        mls_mid = (mls_min+mls_max) // 2
        print("------from--------")
        print(f"mls_max: {mls_max}")
        print(f"mls_mid: {mls_mid}")
        print(f"mls_min: {mls_min}")
        print("--------to---------")
        if enoughDivideKWithMinimalX(A, K, mls_mid):
            print(f"mls could be smaller than {mls_mid}")
            mls_max = mls_mid - 1
            if mls_min > mls_max:
                print(f"But mls was definitely bigger than {mls_max}")
                print(f"Therefore the mls is {mls_min}. Period.")
                break
        else:
            print(f"mls should be bigger than {mls_mid}")
            mls_min = mls_mid + 1
        print(f"mls_max: {mls_max}")
        print(f"mls_min: {mls_min}")

    return mls_min

print(solution(3, 5, [2, 1, 5, 1, 2, 2, 2]))
```
