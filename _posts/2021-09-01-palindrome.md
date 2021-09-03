---
title: "백준 17609번"
date: 2021-09-01T17:55:00+09:00
categories:
  - 코드
tag:
  - 백준
  - 투 포인터
  - Palindrome
header:
  teaser: /assets/images/code.jpg
---

투 포인터를 사용해서 Palindrome인 string과 하나만 제거하면 Palindrome인 string을 구분하는 문제이다. 
* Palin 함수를 짜는 것은 예정된 수순인데, SemiPalin도 Palin 함수에서 조금 더 로직을 추가해서 구현이 가능하다.
* 핵심은 가능한 부분까지 포인터를 이동시키다가 막히면 양 옆 문자를 하나씩 제거한 string에 대해서 Palin을 돌려보고 하나라도 가능하면 SemiPalin이라는 점이다.
* SemiPalin을 구분할 시에 Palin을 사용하려고 하는 생각에 사로잡혔고, SemiPalin를 구하는 방법을 iteration을 이용한 방법 밖에 생각하지 못했다.
* Palin 자체를 투 포인터로 체크하는 로직에 익숙하다면 SemiPalin도 떠올릴 수 있게 된다. 

```python
import sys

def isPalin(w, l, r):
    while l < r:
        if w[l] == w[r]:
            l += 1
            r -= 1
        else:
            return False
    return True

def isPalinOrSemiPalin(w, l, r):
    while l < r:
        if w[l] == w[r]:
            l += 1
            r -= 1
        else:
            del_left = isPalin(w, l+1, r)
            del_right = isPalin(w, l, r-1)
            if del_left or del_right:
                return 1
            else:
                return 2
    return 0 
            

for _ in range(int(sys.stdin.readline().rstrip())):
    w = sys.stdin.readline().rstrip()
    l = 0
    r = len(w) - 1
    check = isPalinOrSemiPalin(w, l, r)
    print(check)
```
