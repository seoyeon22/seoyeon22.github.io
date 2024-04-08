---
layout: single
title: "Backtracking"
category: Algorithm
tags:
    - backtracking
    - pruning
    - search algorithm
---

# Backtracking

> 해를 찾는 도중 막히면 되돌아가서 다시 해를 찾는 기법

> 해결책에 대한 후보군을 구성하여 불필요한 탐색을 줄인다

> exhaustive searching(완전 탐색)을 위한 divide-and-conquer 방식

의사 코드

```python
def 재귀함수(n):
	if 정답이면 :
		출력 or 저장
	else : 정답이 아니면 :
		for 모든 자식 노드에 대해서:
			if 정답에 유망하다면(답의 가능성이 있으면) :
				자식노드로이동
				재귀함수(n+1)
				부모노드로 이동
```

## 활용
스도쿠, 순열&조합, N-Queen, 암호해독

[백준 15649 -  N과 M(1)](https://www.acmicpc.net/problem/15649)

```python
N, M = map(int, input().split())
ans = []

def backtrack():
    if len(ans) == M: # 배열의 길이를 확인(재귀함수를 마치는 조건)
        print(*ans)
        return 

    for i in range(1, N+1): # 1 ~ N 까지
        if i not in ans: # 중복 확인(백트래킹에서의 한정 조건)
            ans.append(i) # 배열 추가
            backtrack() # 백트래킹
            ans.pop() # 전 단계로 이동
    return

backtrack()
```


[백준 9663 - N-Queen](https://www.acmicpc.net/problem/9663)
