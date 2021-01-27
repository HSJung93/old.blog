---
permalink: /
title: "Welcome! This is Hoesung Jung's homepage."
author_profile: true
classes: wide
redirect_from: 
  - /about/
  - /about.html
---

## As a graduate

Hoesung Jung is in the Department of Political Science and International Relations(International relations major) at Seoul National University. He hold a Bachelor's degree of Arts in Political Science and International Relations(Political science major). His research interests include political methodology, game thoery, and text analysis. For example, recent research *Matching with Network: How to Measure the Causal Effects of CAFTA* is dealing with reproducive procedure to infer causal effects with network data. He gave short presentation with this article in AsianPolmeth at January 13, 2021. *What Makes the Congressmen Aggresive: Fight Detection with Congress Committee Metting Log* is modeling motives of the congressmen's behaviour with subgame perfect Nash equilibrium and detecting fights in congress committee meeting log with distilled BERT model. 

## As a programmer 

```python
class HoesungJung:
    def __init__(self, fluent, intermediate, beginner):
        self.__name = "Hoesung Jung"
        self.fluent = fluent
        self.intermediate = intermediate
        self.beginner = beginner
    
    def isAvailable(self, language):
        if language in fluent:
            return f"{__name} is available to do complete tasks \
            with {language} without assistance."

        elif language in intermediate:
            return f"{__name} is available to start experimental projects \
            with {language} utilizing reference and resources of the others."

        elif language in beginner:
            return f"{__name} is available to recall a common knowledge \
            or an understanding of basic techniques \
            gained in a classroom with {language}"

        else:
            return f"Now {__name} needs to google the another new language \
            {language}... Thanks!"

HoesungJung(
    ["Python", "R", "HTML/CSS"], 
    ["JavaScript/NodeJS", "Java"], 
    ["C/C++", "Ruby"]
)
HoesungJung.isAvailable("Python")
```

## As a worker

* March 2015 ~ March 2017: Charge of Quaters in Air Force Academy(Military Service)
* November 2018 ~ April 2019: Big data and financial technology program in SNU
* Summer 2019: Intership in Aizen Global