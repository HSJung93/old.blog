---
title: "Value냐 Reference냐 그것이 문제로다"
date: 2021-09-06T18:28:00+09:00
categories:
  - 코드
tag:
  - python 
  - itertools
header:
  teaser: /assets/images/code.jpg
---
python에서 itertools는 표본 추출의 4가지 경우를 구분하여 지원해줍니다. 통계학을 공부한 사람은 패키지를 사용하기 어려워하고, 컴퓨터 공학을 공부한 사람은 표본 추출을 헷갈려하는 상황인 것 같습니다. 이 포스팅에서는 네가지 개념과 메소드를 정리합니다.

* 중복 순열 : 복원 O, 순서 O : n**k
* 중복 조합 : 복원 O, 순서 X : (n+k-1) C k
* 순열 : 복원 X, 순서 O : n P k = k! * n C k
* 조합 : 복원 X, 순서 X : n C k

```python
from itertools import combinations, permutations, product, combinations_with_replacement

l = ["a", "b", "c"]

print("product: 복원 O 순서 O")
print(list(product(l, repeat=2)))

print("combi_repla: 복원 O 순서 X")
print(list(combinations_with_replacement(l, 2)))

print("permu: 복원 X 순서 O")
print(list(permutations(l, 2)))

print("combi: 복원 X 순서 X")
print(list(combinations(l, 2)))
```