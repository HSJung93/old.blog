---
title: "Prefix와 정렬, 이중 loop와 해쉬"
date: 2021-09-03T14:30:00+09:00
categories:
  - 코드
tag:
  - 정렬
  - 해쉬
header:
  teaser: /assets/images/code.jpg
---
프로그래머스 전화번호 목록 문제의 풀이입니다. 전화번호의 배열이 주어졌을 때, 다른 전화번호로 시작하는 전화번호가 없는지 확인해야 합니다. 
* 두 전화번호를 비교해야하는데 모든 전화번호의 짝을 비교하면 이중 loop로 O(N**2)가 나오므로 다른 방법을 찾아야 합니다.
* 이중 loop는 문제의 조건에 해쉬나 정렬을 잘 사용하면 해결되는 경우가 많은 것 같습니다. 
* 문자열 배열을 정렬하면 문자열의 앞자리부터 우선순위의 문자열일수록 앞에오게 됩니다. 결국 문자열의 앞부분이 유사하면 배열의 바로 앞뒤에 위치하게 됩니다. 이를 이용하면 됩니다.
* `zip`은 두 배열의 길이가 달라서 앞에서부터 하나씩 두 배열의 요소를 순서대로 불러올 수 있도록 해줍니다. 길이가 부족한 부분부터는 None이 출력됩니다. 
* `startwith()` 메소드는 문제에서 해당하는 작업을 정확히 수행해줍니다만 중요한건 아닙니다.

```python
def solution(phoneBook):
    phoneBook = sorted(phoneBook)

    for p1, p2 in zip(phoneBook, phoneBook[1:]):
        if p2.startswith(p1):
            return False
    return True
```
