# 최소 공통 조상

## 링크

[최소 공통 조상](https://www.acmicpc.net/problem/11437)

- 백준

## 문제 설명

N(2 ≤ N ≤ 50,000)개의 정점으로 이루어진 트리가 주어진다. 트리의 각 정점은 1번부터 N번까지 번호가 매겨져 있으며, 루트는 1번이다.

두 노드의 쌍 M(1 ≤ M ≤ 10,000)개가 주어졌을 때, 두 노드의 가장 가까운 공통 조상이 몇 번인지 출력한다.

**입력**

첫째 줄에 노드의 개수 N이 주어지고, 다음 N-1개 줄에는 트리 상에서 연결된 두 정점이 주어진다. 그 다음 줄에는 가장 가까운 공통 조상을 알고싶은 쌍의 개수 M이 주어지고, 다음 M개 줄에는 정점 쌍이 주어진다.

**출력**

M개의 줄에 차례대로 입력받은 두 정점의 가장 가까운 공통 조상을 출력한다.

**입출력 예**

```python
15
1 2
1 3
2 4
3 7
6 2
3 8
4 9
2 5
5 11
7 13
10 4
11 15
12 5
14 7
6
6 11
10 9
2 6
7 6
8 13
8 15

# 출력
2
4
2
1
3
1
```

<br></br>

## 풀이

```python
import sys
sys.setrecursionlimit(int(1e5))
n = int(input())

parent = [0] * (n + 1) # 부모 노드 정보
d = [0] * (n + 1) # 각 노드까지의 깊이
c = [0] * (n + 1) # 각 노드의 깊이가 계산되었는지 여부
graph = [[] for _ in range(n + 1)] # 그래프 정보

for _ in range(n - 1):
  a, b = map(int, input().split())
  graph[a].append(b)
  graph[b].append(a)

# 루트 노드부터 시작하여 깊이(depth)를 구하는 함수
def dfs(x, depth):
  c[x] = True
  d[x] = depth
  for y in graph[x]:
    if c[y]: # 이미 깊이를 구했다면 넘기기
      continue
    parent[y] = x
    dfs(y, depth + 1)

# A와 B의 최소 공통 조상을 찾는 함수
def LCA(a, b):
  # 먼저 깊이(depth)가 동일하도록
  while d[a] != d[b]:
    if d[a] > d[b]:
      a = parent[a]
    else:
      b = parent[b]
  # 노드가 같아지도록
  while a != b:
    a = parent[a]
    b = parent[b]
  return a

dfs(1, 0) # 루트 노드는 1번 노드

m = int(input())

for i in range(m):
  a, b = map(int, input().split())
  print(LCA(a, b))
```
