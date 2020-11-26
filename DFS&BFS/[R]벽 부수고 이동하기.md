# 벽 부수고 이동하기

## 링크

[벽 부수고 이동하기](https://www.acmicpc.net/problem/2206)

- 백준
- 못 품(복습 필수!)

## 문제 설명

N×M의 행렬로 표현되는 맵이 있다. 맵에서 0은 이동할 수 있는 곳을 나타내고, 1은 이동할 수 없는 벽이 있는 곳을 나타낸다. 당신은 (1, 1)에서 (N, M)의 위치까지 이동하려 하는데, 이때 최단 경로로 이동하려 한다. 최단경로는 맵에서 가장 적은 개수의 칸을 지나는 경로를 말하는데, 이때 시작하는 칸과 끝나는 칸도 포함해서 센다.

만약에 이동하는 도중에 한 개의 벽을 부수고 이동하는 것이 좀 더 경로가 짧아진다면, 벽을 한 개 까지 부수고 이동하여도 된다.

맵이 주어졌을 때, 최단 경로를 구해 내는 프로그램을 작성하시오.

**입력**

- 첫째 줄에 N(1 ≤ N ≤ 1,000), M(1 ≤ M ≤ 1,000)이 주어진다. 다음 N개의 줄에 M개의 숫자로 맵이 주어진다. (1, 1)과 (N, M)은 항상 0이라고 가정하자.

**출력**

- 첫째 줄에 최단 거리를 출력한다. 불가능할 때는 -1을 출력한다.

**입출력 예**

````python
6 4
0100
1110
1000
0000
0111
0000

# 출력: 15

```python
4 4
0111
1111
1111
1110

# 출력 -1
````

<br></br>

## 풀이

- 큐에 breakCount 를 포함하는 생각까지는 좋았으나, 벽을 뚫었을 경우의 최단 경로와 안 뚫었을 경우의 최단 경로를 구분할 수 있는 방법을 못 찾았다.
- 풀이에서는 3차원 리스트를 사용하여 구분한다. 좋은 풀이.

```python
from collections import deque

dx = [1, 0, -1, 0]
dy = [0, 1, 0, -1]

def bfs(data, visited):
  queue = deque([[[0, 0], 1]])
  visited[0][0][1] = 1

  while queue:
    coord, breakCount = queue.popleft()
    x, y = coord

    if x == n - 1 and y == m - 1:
      return visited[x][y][breakCount]

    for i in range(4):
      nx = x + dx[i]
      ny = y + dy[i]

      if nx < 0 or ny < 0 or nx >= n or ny >= m: continue

      if data[nx][ny] == 0 and visited[nx][ny][breakCount] == 0:
        queue.append([[nx, ny], breakCount])
        visited[nx][ny][breakCount] = visited[x][y][breakCount] + 1
      if data[nx][ny] == 1 and breakCount == 1:
        queue.append([[nx, ny], 0])
        visited[nx][ny][0] = visited[x][y][1] + 1
  return -1

n, m = map(int, input().split())
data = []
visited = [[[0] * 2 for i in range(m)] for _ in range(n)]
for _ in range(n):
  data.append(list(map(int, list(input()))))

print(bfs(data, visited))
```
