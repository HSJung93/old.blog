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
        self.fluent = fluent
        self.intermediate = intermediate
        self.beginner = beginner
    
    def isAvailable(self, language):
        if language in fluent:
            return f"to do complete tasks with {language} without assistance."

        elif language in intermediate:
            return f"to start experimental projects with {language} utilizing reference and resources of others."

        elif language in beginner:
            return f"to recall a common knowledge or an understanding of basic techniques gained in a classroom with {language}"

HoesungJung(["Python", "R", "HTML/CSS"], ["JavaScript/NodeJS", "Java"], ["C/C++", "Ruby"])
```