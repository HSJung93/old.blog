---
title: "백준 1806번"
date: 2021-09-01T16:55:00+09:00
categories:
  - 코드
tag:
  - 백준
  - 투 포인터
header:
  teaser: /assets/images/code.jpg
---
백준 1806번 문제로 투 포인터로 주어진 리스트에서 S이상의 부분합을 가진 범위의 최소값을 구하는 문제이다.
* low와 high 모두 0으로 시작하고 탐색할 값에는 0번째 값을 넣어두고 시작하는 것이 직관적이다
* while문의 조건문은 low와 high의 관계와 high의 최대값이다. 
* low을 올리고 high을 올릴 조건을 구분한다. 이때 구할 값에 어떤 변화가 생기는지 순서와 함께 고민한다. 보통 low나 high 인덱스에 해당하는 값에 대하여 어떤 연산을 할 것이다. 
* while문의 조건 안에 break문을 피하려고 하다가 문제를 어렵게 푸는 경우가 많은 것 같다. 일단 구현을 하고, 추후 시간이 남을 시에 리팩토링을 하면 된다. 코드의 단순성만이 코드의 아름다움은 아니다. 시간복잡도와 엣지 케이스 등 아름다움이 구현될 영역은 또 많다. 

```python
import math
N, S = map(int, input().split())
arr = []
for i in map(int, input().split()):
    arr.append(i)
    
sum_ = arr[0]
length = math.inf
low = 0
high = 0

while low <= high and high < N:
    if sum_ < S:
        high += 1
        if high == N:
            break
        sum_ += arr[high]
        
    elif sum_ == S:
        length = min(length, high-low+1)
        high += 1
        if high == N:
            break
        sum_ += arr[high]
        
    elif sum_ > S:
        length = min(length, high-low+1)
        sum_ -= arr[low]
        low += 1
        
if length == math.inf:
    print(0)
else:
    print(length)
```
