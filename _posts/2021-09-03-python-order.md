---
title: "Python에서 여러 기준으로 정렬하기"
date: 2021-09-03T17:30:00+09:00
categories:
  - 코드
tag:
  - 정렬
  - itemgetter
  - attrgetter
header:
  teaser: /assets/images/code.jpg
---
코딩 테스트를 풀다보면 종종 중첩 리스트의 값을 정렬해야합니다. 이 때 operator의 itemgetter나 attrgetter를 사용하면 직관적인 순서로 보다 짧게 여러 기준으로 우선순위를 나누어 정렬할 수 있습니다. 

```python
MixedTuples = [
    ("b", 2, "나"),
    ("a", 2, "나"),
    ("b", 1, "나"),
    ("a", 1, "나"),
    ("b", 2, "가"),
    ("a", 2, "가"),
    ("b", 1, "가"),
    ("a", 1, "가")
]

class Mixed:
    def __init__(self, eng, num, kor):
        self.eng = eng
        self.num = num
        self.kor = kor
        
    def __repr__(self):
        return repr((self.eng, self.num, self.kor))
    
MixedObjects = [
    Mixed("b", 2, "나"),
    Mixed("a", 2, "나"),
    Mixed("b", 1, "나"),
    Mixed("a", 1, "나"),
    Mixed("b", 2, "가"),
    Mixed("a", 2, "가"),
    Mixed("b", 1, "가"),
    Mixed("a", 1, "가")
]
        
MixedTuples.sort(key=lambda x:x[2])
MixedTuples.sort(key=lambda x:x[1])
MixedTuples.sort(key=lambda x:x[0])

print("0번째-1번재-2번째 요소 순 정렬 :")
print(MixedTuples)
print()

MixedObjects.sort(key=lambda x:x.kor)
MixedObjects.sort(key=lambda x:x.num)
MixedObjects.sort(key=lambda x:x.eng)

print("한국어-숫자-영어 순 정렬 :")
print(MixedObjects)
print()

from operator import itemgetter, attrgetter

MixedTuples.sort(key=itemgetter(1, 2, 0))

print("1번째-2번재-0번째 요소 순 정렬 :")
print(MixedTuples)
print()

MixedObjects.sort(key=attrgetter("num", "eng", "kor"))

print("숫자-영어-한국어 순 정렬 :")
print(MixedObjects)
```
