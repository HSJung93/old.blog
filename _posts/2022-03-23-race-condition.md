---
title: "What is CAP Theorem"
date: 2022-03-23T09:23:00+09:00
categories:
  - Memo
tags:
  - deadlock
  - semaphore
  - mutex
# header:
#   teaser: /assets/images/code.jpg
---

A race conditoin or race hazard is the condition of an electronics, software, or othere system where the system's substantive behavior is dependent on the sequence of timing of other uncontrollable events. A race condition arises in software when a computer program, to operate properly, depends on the sequence or timing of the program's processes or threads. Critical race conditions often happen when the processes or threads depend on some shared state. Operations upon shared states are done in criticla sections that must be mutually exclusive. 

A race condtion can be difficult to reproduce and debug because the end result is nondeterministic and depends on the relative timing between interfering threads. Problems of this nature can therefore disappear when running in debug mode, addint extra logging, or attaching a debugger. A but that disappears like this during debugging attempts is often referred to as a "Heisenbug". It is therefore better to avoid race conditions by careful software design.