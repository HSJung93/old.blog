---
title: "bfs 응용 문제"
date: 2021-09-09T13:29:00+09:00
categories:
  - 코드
tag:
  - bfs
header:
  teaser: /assets/images/code.jpg
toc: true
toc_sticky: true
---

bfs의 응용 문제들을 모아봤습니다.

## 술래잡기 
* 백준 1697
* dp로 착각하기 쉽지만 양방향으로 이동한다는 점에서 dp bottom-up 방식으로 풀기 쉽지 않다.  

```python
import sys, math
from collections import deque

MAX = 10**5

subin, brother = map(int, sys.stdin.readline().split())

# q 만들고 초기값 넣기
# 시간 변수를 넣지는 않는다. 
q = deque()
q.append(subin)

# 언제 방문했는지를 리스트 만들고 초기값 넣기
# 시간 변수를 따로 정의하지 않는다.
# 위치별 방문 시간을 바로 리스트에 저장한다.
# 리스트[위치] = 시간
dist = [math.inf] * (MAX + 1) 
dist[subin] = 0

while q:
    # 큐에서 지금 값을 먼저 뽑기
    # 뒤에 q사이즈만큼 안뽑아도 되는 것이 복잡하지 않고, dist에 다 걸린 시간이 저장되어 있어서 후에 각각 더해주면 된다. 
    now = q.popleft()
    
    # 위치가 같을 때 종료 조건 제시
    if now == brother:
        # 현재 위치의 
        print(dist[now])
        break
    
    # 다음 moves for문
    for next in (now-1, now+1, now*2):
        # 이동 가능 조건 제시
        if 0 <= next <= MAX and dist[next] > dist[now] + 1:
            # 외부 리스트에 거리 업데이트
            dist[next] = dist[now] + 1
            # 새롭게 큐에 넣기
            q.append(next)
```

## 술래잡기 2: 움직이는 경우
* 라인 기출
* 큐를 while문 안에서 for문으로 먼저 다 빼서 시간을 맞춘다.

```python
            
from collections import deque

def solution(conyPosition, brownPosition):
    
    # 시간 변수를 따로 선언한다. 
    # while문 안에서 시간의 가속도를 표현할 수 있다. 
    time = 0
    MAX = 2 * 10**5 
    
    # 큐를 만들고 초기값을 넣는다. 
    q = deque()
    q.append((brownPosition, 0))
    
    # 브라운이 visit에 도달한 시간을 dict에 넣어서 업데이트 해간다. 나중에 in으로 검색할 때 O(n) 빠르게 검색하도록 한다. 
    # 초기값을 visit[0][0] = True로 visit에 세팅하지 않아도 잘 돌아간다. 
    visit = [{} for _ in range(MAX+1)] 

    # 적당히 돌려도 돌아간다. 가끔 그냥 True를 써서 돌려도 되는데 무한 루프 때문에 꺼려져서 안쓰는 경우가 있다. 
    while conyPosition <= MAX:
        
        # 종료 조건과 관련된 거리 변수가 q에서 나오는 위치값과는 달리 함수적이지 않고 상수적으로 변화한다. 그래서 뽑기 전에 정의한다.
        # 종료 조건 또한 뽑기 전이다.
        conyPosition += time
        
        # 종료 조건2: 보통 예외 조건이 더 강력하다
        if conyPosition > MAX:
            return - 1
        
        # 종료 조건1
        if time in visit[conyPosition]: # 코니의 현 포지션에 브라운이 도착한 시간이 있다면 
            return time
        
        #  q에 동시에 돌아가 있는 것을 for문으로 다 뽑는다. i는 필요 없다. 
        for _ in range(len(q)):
            cur = q.popleft()
            curPosition = cur[0]
            # 값을 넣어두지 않으면 외부에 저장한 값에서 불러와야 하는데 외부에 저장한 값이 여러 개이기 때문에 나중에 그 중에 하나를 불러올 수가 없다. 외부 저장 리스트에 여러 값을 저장해야 문제를 풀 때(1대1이 아닐 때) 그 때 상태를 기록하기 위해서 값을 저장해둔다. 
            newTime = cur[1]+1
            
            for newPosition in (curPosition-1, curPosition+1, curPosition*2):
                if 0 <= newPosition <= MAX:
                    
                    # 외부 리스트에 걸린 시간 없데이트
                    # True는 밸류 값이 필요해서 넣은 장식값
                    visit[newPosition][newTime] = True
                    
                    # 시간도 같이 집어 넣는다. 
                    q.append((newPosition, newTime))
        
        time += 1
        
print(solution(11, 2))
```

## 아기상어
* 이중리스트 공간에 조건부의 갈수 있는 공간이 있는 경우, graph 에 직접 변형한다. 직접 변형해야할때는 bfs() 함수에 변수로 받지 않는다.
* 조건부 이동이 있는 경우, 다음 이동 큐를 돌리기 전에 조건부 이동에 대한 리스트를 저장해 두고 필요한 조작을 가한다. 일단 새로운 기능이 있으면 새로운 리스트가 필요하다.

```python
        
import sys
from collections import deque

N = int(sys.stdin.readline())
graph = [[0] * N for _ in range(N)]

for i in range(N):
    graph[i] = list(map(int, sys.stdin.readline().split()))

moves = [
    (-1, 0), # 위
    (0, -1), # 왼쪽
    (1, 0), # 아래
    (0, 1) # 오른쪽
]
                 
for i in range(N):
    for j in range(N):
        if graph[i][j] == 9:
            x, y = i, j
            graph[i][j] = 0
        
weight, time, eat = 2, 0, 0

def isIn(nx, ny, N):
    return 0<= nx < N and 0<= ny < N
    
def bfs(x, y, weight, time, eat):
    q, can_eat = deque(), [] # 종료 조건을 위하여 큐를 하나 더 만드는 경우
    
    q.append([x, y])
    # 그냥 이동거리는 -1를 디폴트로 주고 거리를 늘려나가면 visited처럼 활용할 수 있어서 편리하다. 
    c = [[-1]*N for _ in range(N)]
    c[x][y] = time
    
    while q:
        for _ in range(len(q)):
            x, y = q.popleft()
            
            for dx, dy in moves:
                nx, ny = x + dx, y + dy
                
                # 그냥 이동이 있고, 먹는 이동이 있다. 
                if isIn(nx, ny, N) and c[nx][ny] == -1:
                    # 그냥 평범한 이동 조건 / 나뉘는 두가지 이동 조건을 층을 나눠서 코드를 짠다.
                    if graph[nx][ny] == 0 or graph[nx][ny] == weight:
                        c[nx][ny] = c[x][y] + 1
                        q.append([nx, ny])
                        
                    elif 0 < graph[nx][ny] < weight:
                        can_eat.append([nx, ny])
            
        # 종료 조건이 이동 후에 생기는 경우    
            
        if can_eat:
            nx, ny = min(can_eat)
            eat += 1
            if eat == weight:
                eat = 0
                weight += 1
                
            graph[nx][ny] = 0
            return nx, ny, weight, c[x][y] + 1, eat
            
def search(weight, graph):
    for row in graph:
        for elm in row:
            if elm != 0 and elm < weight:
                return True
    return False

# while을 이용하여 bfs를 그때 그때 새롭게 돌린다. graph에 외부에 값을 저장해야 한다. 
while search(weight, graph):
    x, y, weight, time, eat = bfs(x, y, weight, time, eat) # 그래프는 역시 변수에 없음
    
print(time)
```

## 백준 17142
* 여러 초기값, 최대 최소값
* 아예 간편하게 길이를 위한 이중 리스트를 만들어 버리는게 편하다. 

```python

from itertools import combinations
from collections import deque
import sys, math

input = sys.stdin.readline
         
moves = [(1, 0), (-1, 0), (0, -1), (0, 1)]        
n, m = map(int, input().split())

graph = []
starts = []
everySpace = 0

for i in range(n):
    row = list(map(int, input().split()))
    graph.append(row)
    for j in range(n):
        if row[j] == 2: starts.append([i, j])
        if row[j] != 1: everySpace += 1

combi = list(combinations(starts, m))
result = math.inf

def isIn(nx, ny, n):
    return 0 <= nx < n and 0 <= ny < n

def bfs():
    # q에 넣는 것을 함수 밖에서 해도 된다. 
    # 함수의 인자가 없는게 편리?
    
    while q:
        x, y = q.popleft()
        
        # qsize로 구분할 필요 없음
        
        for dx, dy in moves:
            nx, ny = x + dx, y + dy
            # visited == 0/ visited != 1/ visited == False/ dist == -1
            if isIn(nx, ny, n) and dist[nx][ny] == -1 and graph[nx][ny] != 1:
                q.append([nx, ny])
                dist[nx][ny] = dist[x][y] + 1

for i in combi:
    dist = [[-1] * n for i in range(n)]
    q = deque()
    
    for x, y in i:
        dist[x][y] = 0
        q.append([x, y])
        
    bfs()
    
    notVisited = 0
    for row in dist: notVisited += row.count(-1)
    
    if everySpace == (n * n - notVisited):
    
        max_n = 0
        for j in range(n):
            for k in range(n):
                # 어디서 조건을 확인하는것이 편할지, bfs 밖에서 해결할 수 있고 그러면 편리
                if graph[j][k] != 1 and [j, k] not in starts:
                    max_n = max(max_n, dist[j][k])
                    
        result = min(result, max_n)

print(result if result != math.inf else -1)
```
