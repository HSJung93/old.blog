---
title: "@Transactional vs @Synchronized"
date: 2022-06-24T11:34:00+09:00
categories:
  - Memo
tags:
  - Transactional
  - Synchronized
# header:
#   teaser: /assets/images/code.jpg
---

just to point the extremely obvious: transactions can commit and rollback, synchronization ensures only exclusive access. There is nothing in common, as transactions are not even required to lock objects. For synchronized, if we already have monitor we proceed with it and if not we will attempt to acquire it.