---
title: "파이썬스러운 스왑"
date: 2021-08-10T21:30:00+09:00
categories:
  - 코드
tags:
  - pythonic 
  - swap
  - hackerrank
header:
  teaser: /assets/images/code.jpg
---

Hackerank의 인터뷰 준비 키트에 있는 링크드 리스트 문제 중 [Doubly Linked List 뒤집기][Reverse-a doubly-linked-list] 문제를 풀다보면 while 루프를 돌면서 노드의 다음 포인터와 이전 포인터를 스왑해야 하는 상황에 직면하게 됩니다. 

파이썬의 경우 temp 변수를 만들 필요 없이 `프레드, 조지 = 조지, 프레드` 와 같은 방식으로 두 변수의 값을 스왑할 수 있습니다. 이 때 while 문의 동작을 위하여 노드에 다음 노드 값을 할당할 때 조심스러워 지는데(이것도 되나 싶은 마음?), 그냥 콤마를 찍고 할당을 하면 해결됩니다(이것도 되네!). 

```python
def reverse(head):
    while head.next != None:
        # 포인터를 스왑하고 다음 노드로도 while 루프를 진행시킨다. 
        head.next, head.prev, head = head.prev, head.next, head.next
        
    # 테일 노드 바꾸기
    head.next, head.prev = head.prev, None
    
    return head
```

[Reverse-a doubly-linked-list]: https://www.hackerrank.com/challenges/reverse-a-doubly-linked-list/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=linked-lists