---
layout: single
title: "Minimum Spanning Tree"
category: Algorithm
tags:
    - MST
    - Prim
    - Kruskal
toc: true
toc_sticky: true
---
# Minimum Spanning Tree(최소 신장 트리)
> 주어진 그래프의 모든 정점들을 연결하고 사이클을 이루지 않는 부분 그래프 중에서 가중치의 합이 최소인 트리

![minimum spanning tree](https://github.com/seoyeon22/seoyeon22.github.io/assets/70433999/927ffd54-681e-478c-895f-c1dc3c510612)

최소 신장 트리를 찾을 수 있는 알고리즘은 크게 간선 중심의 크루스칼(Kruskal) 알고리즘과 정점 중심의 프림(Prim) 알고리즘이 있다. 크루스칼 알고리즘은 $O(E + log(E))$, 프림 알고리즘은 $O(V+E)log(V)$의 시간복잡도를 가지므로 간선이 적으면 크루스칼 알고리즘, 간선이 많으면 프림 알고리즘을 사용하는 것이 좋다.

# Prim's Algorithm
1. 최초 출발 노드와 연결되어 있는 간선들 중 가중치가 최소인 것을 선택
2. 최소 스패닝 트리에 포함된 모든 정점들과 연결된 간선들 중 가중치가 최소인 것을 선택
3. V - 1개의 간선이 선택될때까지 2번을 반복

```python
import heapq

def prim(start):
    heap = list()
    connected = [False] * (V+1)
    total_weight = 0
    heapq.heappush(heap, (0, start))
    while heap:
        w, v = heapq.heappop(heap)
        if not connected[v]:
            connected[v] = True
            total_weight += w
            for i in range(1, V+1):
                if graph[v][i] != 0 and not connected[i]:
                    heapq.heappush(heap, (graph[v][i], i))
```

# Kruskal's Algorithm
1. 간선 데이터를 비용에 따라 오름차순으로 정렬한다.
2. 간선을 하나씩 확인하며 현재의 간선이 사이클을 발생시키는지 확인하고 사이클이 발생하지 않는 경우 최소 신장 트리에 포함시킨다.
3. 모든 간선에 대하여 2번의 과정을 반복한다.

# Union-Find Algorithm
> 여러 개의 노드가 존재할 때 두 개의 노드를 선택해서 현재 노드가 서로 같은 그래프에 속하는지 판별하는 알고리즘으로 서로소 집합(Disjoint-Set) 알고리즘이라고도 불린다.

다음과 같은 과정을 거치고 트리 구조로 집합을 구현한다.
1. 초기화(makeSet) : 각 노드를 하나의 집합으로 초기화하는 과정
2. 합치기(union) : 두 집합을 합치는 과정
3. 찾기(find) : 주어진 노드가 속한 집합의 대표 노드를 반환

![path compression](https://github.com/seoyeon22/seoyeon22.github.io/assets/70433999/42ae0fb1-4b69-4773-9615-96207ab7d0c3)



아래 코드는 union-by-rank 알고리즘으로 $O(logN)$의 시간복잡도를 가진다.
```python
# 재귀 함수
parents = [i for i in range(MAX_SIZE)]

def find(parent, x):
    if parent[x] == x:
        return x

    # 경로 압축(Path Compression)
    # find 하면서 만나는 모든 값의 부모 노드를 root로 설정
    parent[x] = find(parent, parent[x])
    return parent[x]

def union(parent, x, y, rank):
    rootX = find(parent, x)
    rootB = find(parent, y)

    # 두 값의 root가 같으면 연결 X
    if rootX == rootY:
        return

    # rank가 높은 쪽을 root로 설정
    if rootX < rootY:
        parent[rootY] = rootX
    else:
        parent[rootX] = rootY
```

![union-find algorithm](https://github.com/seoyeon22/seoyeon22.github.io/assets/70433999/1efc119f-a499-4593-b25e-b7d46ebf5a08)


# 관련 문제
[백준 1197 - 최소 스패닝 트리](https://www.acmicpc.net/problem/1197) 

크루스칼을 활용한 코드

```python
import sys

input = sys.stdin.readline
V, E = map(int, input().split()) # 정점의 개수, 간선의 개수
parent = [0] * (V + 1)
edges = [] # 인접리스트
weights = 0

for _ in range(E):
    A, B, C = map(int, input().split()) # C: 간선의 가중치, 음수 가능
    edges.append((C, A, B))

edges.sort()

# 부모를 자기 자신으로 초기화
for i in range(1, V+1):
    parent[i] = i

# 원소 x가 속한 집합 찾기
def find(parent, x):
    if parent[x] == x:
        return x
    parent[x] = find(parent, parent[x])
    return parent[x]

# 간선 연결
def union(parent, a, b):
    rootA = find(parent, a)
    rootB = find(parent, b)

    if rootA < rootB:
        parent[rootB] = rootA
    else:
        parent[rootA] = rootB

for edge in edges:
    cost, a, b = edge
    # 사이클이 발생하지 앟을 경우 집합에 포함
    if find(parent, a) != find(parent, b):
        union(parent, a, b)
        weights += cost

print(weights)
```