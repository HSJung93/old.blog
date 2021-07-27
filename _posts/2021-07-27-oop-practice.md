---
title: "파이썬을 이용한 객체지향 연습"
date: 2021-07-27T15:03:00+09:00
categories:
  - code
tags:
  - object-oriented-programming
  - python
header:
  teaser: /assets/images/code.jpg
---

유머 감각은 참 중요합니다. 이왕에 OOP를 연습하기로 마음 먹은 것, 저 자신을 class로 만들어보려고 했습니다. 

```python
class HoesungJung:
    def __init__(self, fluent, intermediate, beginner):
        self.__name = "Hoesung Jung"
        self.fluent = fluent
        self.intermediate = intermediate
        self.beginner = beginner
    
    def isAvailable(self, language):
        if language in self.fluent:
            return f"{self.__name} is available to do complete tasks \
            with {language} without assistance."

        elif language in self.intermediate:
            return f"{self.__name} is available to start experimental projects \
            with {language} utilizing reference and resources of the others."

        elif language in self.beginner:
            return f"{self.__name} is available to recall a common knowledge \
            or an understanding of basic techniques \
            gained in a classroom with {language}"

        else:
            return f"Now {self.__name} needs to google the another new language \
            {language}... Thanks!"

hoesung = HoesungJung(
    ["Python", "R", "HTML/CSS"], 
    ["JavaScript/NodeJS", "Java"], 
    ["C/C++", "Ruby"]
)

print(hoesung.isAvailable("Python"))
```