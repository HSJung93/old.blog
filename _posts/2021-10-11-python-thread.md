---
title: "왜 파이썬은 GIL이 있을까?"
date: 2021-10-11T09:53:00+09:00
categories:
  - 메모
tags:
  - 프로그래머스
  - SQL
header:
  teaser: /assets/images/Caspar_500x300.jpg
---

파이썬은 한 스레드만 하나의 바이트 코드를 실행시킬 수 있도록 해주는 락(=GIL)을 가지고 있다. 따라서 thread와 threading 모듈을 사용하면 오히려 느려지는 경우도 생긴다.

이는 파이썬의 메모리 관리 방식 때문이다. 파이썬은 자신을 참조하고 있는 객체들의 수를 세다가, 마침내 참조된 객체들의 수가 0이 되면 할당된 메모리를 지운다. 이렇게 메모리 관리를 할 경우 스레드가 하나의 객체를 사용할 때 reference count를 관리하기 위하여 모든 객체에 대한 lock을 하게 된다. 이러한 비효율을 막기 위하여 파이썬은 GIL을 사용하게 되었따.