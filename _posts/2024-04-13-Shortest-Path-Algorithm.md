---
layout: single
title: "Shortest Path Algorithm"
category: Algorithm
tags:
    - Floyd-Warshal Algorithm
    - Bellman-Ford Algorithm
    - Dijkstra Algorithm
toc: true
toc_sticky: true
---
# 플로이드-워셜 알고리즘(Floyd-Warshal Algorithm)
> 모든 정점 간 최단 거리를 구하는 알고리즘

## 탐색 과정
각 단계마다 노드 $K$를 거쳐 가는 경우의 거리가 최단 거리인지 확인하지를 반복하며 이를 점화식으로 나타내면 다음과 같다.

 > $D[S][E] = min(D[S][E], D[S][K] + D[K][E])$

1. 최단 거리 배열 $D[S][E]$의 $S$와 $E$가 같은 칸은 0, 다른 칸은 무한대로 초기화한다.
2. 출발 노드는 $S$, 도착 노드는 $E$, 간선의 가중치는 W일 때 $D[S][E] = W$
3. 점화식을 반복하면서 배열의 값을 갱신한다.

```python
for 경유지 K에 관해 (1 ~ N)
	for 출발 노드 S에 관해 (1 ~ N)
    	for 도착 노드 E에 관해 (1 ~ N)
        	D[S][E] = min(D[S][E], D[S][K] + D[K][E])
```

## 특징
- 다이나믹 프로그래밍을 활용하여 2차원 리스트에 최단 거리 정보를 갱신한다.
- 노드의 개수가 $N$개일 때 $N$번의 단계마다 $O(n^2)$의 연산을 하므로 $O(n^3)$의 시간 복잡도를 가진다.

## 구현
```python
INF = int(1e9) # 무한을 의미하는 값으로 10억을 설정

# 노드의 개수 및 간선의 개수를 입력받기
n = int(input())
m = int(input())
# 2차원 리스트(그래프 표현)를 만들고, 모든 값을 무한으로 초기화
graph = [[INF] * (n + 1) for _ in range(n + 1)]

# 자기 자신에서 자기 자신으로 가는 비용은 0으로 초기화
for a in range(1, n + 1):
    for b in range(1, n + 1):
        if a == b:
            graph[a][b] = 0

# 각 간선에 대한 정보를 입력 받아, 그 값으로 초기화
for _ in range(m):
    # A에서 B로 가는 비용은 C라고 설정
    a, b, c = map(int, input().split())
    graph[a][b] = c

# 점화식에 따라 플로이드 워셜 알고리즘을 수행
for k in range(1, n + 1):
    for a in range(1, n + 1):
        for b in range(1, n + 1):
            graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])

# 수행된 결과를 출력
for a in range(1, n + 1):
    for b in range(1, n + 1):
        # 도달할 수 없는 경우, 무한(INFINITY)이라고 출력
        if graph[a][b] == INF:
            print("INFINITY", end=" ")
        # 도달할 수 있는 경우 거리를 출력
        else:
            print(graph[a][b], end=" ")
    print()
```

# 벨만-포드 알고리즘(Bellman-Ford Algorithm)
> 특정 정점에서 다른 정점까지의 최단거리를 구하는 알고리즘

## 탐색 과정
1. 출발 노드를 설정한다.
2. 최단 거리 테이블을 초기화한다.
3. 다음 과정을 (V-1)번 반복한다.
    1. 모든 간선 E개를 하나씩 확인한다.
    2. 각 간선을 거쳐 다른 노드로 가는 비용을 계산하여 최단 거리 테이블을 갱신한다.

## 특징
- 가중치가 있는 그래프와 가중치가 없는 그래프 모두에 적용할 수 있다.
- 가중치가 음수인 경우에도 적용할 수 있지만 음의 사이클이 있는 경우 최단 비용이 음의 무한대가 되므로 적용할 수 없다.
- V-1번 E개의 간선을 탐색하므로 $O(VE)$의 시간 복잡도를 가진다.

## 구현
```python
def bellman_ford(graph, start):

  N = len(graph)
  distance = [INF] * N # 노드 개수 만큼 최단거리 테이블 생성
  distance[start] = 0 # 최단거리 테이블 초기화

  for _ in range(N-1): # 아래 과정을 노드 개수-1 만큼 반복
    for cur in range(N): # 현재 각 노드에서 
      for adj, cost in graph[cur]: # 그래프의 다른 노드를 거쳤을 때
        if distance[cur] + cost  < distance[adj]: # 최단거리가 있으면
          distance[adj] = distance[cur] + cost # 최단거리 갱신

  # 앞선 과정을 한번 더 반복하여 음의 순환이 있는지 판별
  for cur in range(N): 
    for adj, cost in graph[cur]:
      if distance[cur] + cost < distance[adj]:
        return [-1] #음의 순환 존재
  
  return distance
```

# 다익스트라 알고리즘(Dijkstra Algorithm)
> 간선의 가중치가 음수가 아닌 경우 특정 정점에서 다른 정점까지 최단 거리를 구하는 알고리즘

## 탐색 과정
1. 초기에 시작 정점에서 방문 가능한 정점에 대한 비용을 갱신하고 나머지 정점에 대한 비용은 무한대로 설정한다.
2. 방문하지 않은 정점 중 비용이 가장 적게 드는 정점에 방문한다. 해당 점점과 연결된 다른 정점을 거쳐 방문할 때의 비용을 확인하여 최소 비용을 갱신한다.
3. 모든 정점을 방문할 때까지 이와 같은 방식으로 정점 방문 비용을 갱신한다.

## 특징
- 매번 방문하지 않은 노드 중에서 최단 거리가 가장 짧은 노드를 선택한다.
- 우선순위 큐를 사용하여 구현하면 $O(ElogV)$의 시간 복잡도를 가진다.

## 구현
```python
import heapq  # 우선순위 큐 구현을 위함

def dijkstra(graph, start):
  distances = {node: float('inf') for node in graph}  # start로 부터의 거리 값을 저장하기 위함
  distances[start] = 0  # 시작 값은 0이어야 함
  queue = []
  heapq.heappush(queue, [distances[start], start])  # 시작 노드부터 탐색 시작 하기 위함

  while queue:  # queue에 남아 있는 노드가 없으면 끝
    current_distance, current_destination = heapq.heappop(queue)  # 탐색 할 노드, 거리를 가져옴

    if distances[current_destination] < current_distance:  # 기존에 있는 거리보다 길다면 다음 노드 탐색
      continue
    
    for new_destination, new_distance in graph[current_destination].items():
      distance = current_distance + new_distance  # 해당 노드를 거쳐 갈 때 거리
      if distance < distances[new_destination]:  # 알고 있는 거리 보다 작으면 갱신
        distances[new_destination] = distance
        heapq.heappush(queue, [distance, new_destination])  # 다음 인접 거리를 계산 하기 위해 큐에 삽입
    
  return distances
```
### 벨만-포드 VS 다익스트라
- 벨만-포드: 매번 모든 간선을 전부 확인하기 때문에 느리지만 음수 가중치가 있는 경우 사용 가능하다.
- 다익스트라: 매번 방문하지 않은 노드 중에서 최단 거리를 찾기 때문에 빠르지만 음수 가중치가 있으면 사용 불가능하다.

# Reference
- 이수진, 기술 면접 대비 CS 전공 핵심요약집 (IT 대기업 합격자의 비밀노트), 길벗
- https://justkode.kr/algorithm/python-dijkstra/
- https://randomsampling.tistory.com/195


