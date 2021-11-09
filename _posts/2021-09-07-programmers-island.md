---
title: "프로그래머스 섬 연결하기"
date: 2021-09-07T17:11:00+09:00
categories:
  - 코드
tag:
  - 크루스칼 알고리즘
  - 그래프 연결
  - 다익스트라
  - 힙
header:
  teaser: /assets/images/code.jpg
---

프로그래머스의 섬 연결하기 문제는 정렬 뒤에 union_find로 연결시키는 방법으로도, heapq를 사용하는 다익스트라 알고리즘으로도 풀 수 있습니다. 크루스칼 알고리즘에 의하여 local하게 그때 그때 최소의 비용의 노드를 연결하는 것이 전체적으로 최소의 비용 노드의 연결로 이어지기 때문입니다. 결국 방향은 맞았는데 에러가 났을 때 당황했던 것이 문제였습니다. 다익스트라 알고리즘의 경우 cost를 어디에서 더할 것인가에서 조심해야합니다. 힙에서 빼자마자 거리 값을 더해주는 것이 직관적이고 기억하기 쉽습니다.

```python
# 크루스칼 알고리즘으로 푸는 방법

def solution(n, costs):
    parent = [i for i in range(n+1)]
    
    edges= []
    
    for fr, to, cost in costs:
        edges.append((cost, fr, to)) 
        
    edges.sort()
    
    answer = 0
    
    for cost, fr, to in edges:
        if find_parent(parent, fr) != find_parent(parent, to):
            union_parent(parent, fr, to)
            answer += cost
    
    return answer

def find_parent(parent, node):
    if parent[node] != node:
        parent[node] = find_parent(parent, parent[node])
    return parent[node]

def union_parent(parent, a, b):
    parent_a = find_parent(parent, a)
    parent_b = find_parent(parent, b)
    
    if parent_a < parent_b:
        parent[parent_b] = parent_a
    else:
        parent[parent_a] = parent_b
        
print(solution(4, [[0,1,1],[0,2,2],[1,2,5],[1,3,1],[2,3,8]]))

# dijkstart_heap으로 푸는 방법

import heapq

def solution(n, costs):
    answer = 0
    
    g = [[] for _ in range(n)]
    visited = [False] * n
    priority = []
    
    for a, b, cost in costs:
        g[a].append((b, cost))
        g[b].append((a, cost))
        
    heapq.heappush(priority, (0, 0))
    
    while False in visited:
        now_cost, now = heapq.heappop(priority)
        if visited[now]:
            continue
        
        visited[now] = True
        answer += now_cost
        
        for next, next_cost in g[now]:
            if visited[next]:
                continue
            else:
                heapq.heappush(priority, (next_cost, next))
                          
    return answer

print(solution(4, [[0,1,1],[0,2,2],[1,2,5],[1,3,1],[2,3,8]]))
```