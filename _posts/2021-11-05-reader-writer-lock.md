---
title: "Reader-Writer-Lock"
date: 2021-11-05T14:29:00+09:00
categories:
  - 메모
tags:
  - Reader-Writer-Lock
header:
  teaser: /assets/images/slipBox.jpg
---

뮤텍스 방식의 락에서는 한 스레드에서 공유 자원을 선점하고 있으면 다른 스레드의 접근을 막는다. Reader-Writer-Lock의 경우에는 스레드의 작업의 종류가 Read인지 Write인지에 따라서 락을 다르게 적용한다. Read 동작은 내용의 일관성을 해치지 않기 때문에 동시에 같은 내용을 읽어도 동기화 문제가 발생하지 않는다. 따라서 Read 동작을 하고 있는 스레드가 공유 자원을 선점할 경우에는 Read 동작에 대해서는 락을 하지 않고, Write 동작에 대해서만 락을 한다. Write 동작을 하고 있는 스레드의 경우 기존처럼 Read와 Write 동작 모두에 대해서 락을 건다. 결국 Reader-Writer-Lock은 뮤텍스 방식의 락에서 Read 작업의 스레드 접근들만은 허용하는 것이다.
