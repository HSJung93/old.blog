---
title: "List Comprehension for Dictionary in Python"
date: 2021-07-07T17:07:00+09:00
categories:
  - code
tags:
  - list comprehension
  - python
header:
  teaser: /assets/images/code.jpg
---

3줄의 for이 아니라 1줄의 list comprehension을 통해서도,
원래의 딕셔너리의 값의 두배의 값을 가지는 새로운 딕셔너리를 만들 수 있습니다.

```python

original = {"apple":75, "banana":82, "mango":32 }

double = {}
for key, val in original.items():
  double[key] = val * 2

double_lc = {key: val * 2 for key, val in original.items()}
```