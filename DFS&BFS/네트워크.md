# 네트워크

## 링크

[네트워크](https://programmers.co.kr/learn/courses/30/lessons/43162#)

- 프로그래머스 Lv3

## 문제 설명

- 네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다. 예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

- 컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때, 네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

**제한 사항**

- 컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.
- 각 컴퓨터는 0부터 n-1인 정수로 표현합니다.
- i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.
- computer[i][i]는 항상 1입니다.

**입출력 예**

|  n  |             computers             | return |
| :-: | :-------------------------------: | :----: |
|  3  | [[1, 1, 0], [1, 1, 0], [0, 0, 1]] |   2    |
|  3  | [[1, 1, 0], [1, 1, 1], [0, 1, 1]] |   1    |

<br></br>

## 풀이(DFS - 재귀함수 이용)

- 어렵게 풀려고 하지 말자! **코딩 테스트는 핵심 아이디어만 찾으면 길지 않은 코드로 풀 수 있게끔 만들어진다** (괜히 변수같은 거 추가해서 복잡도를 증가시키지 말자)

```python
answer = 0
def solution(n, computers):
    global answer
    visited = [False] * n
    for i in range(n):
        if not visited[i]:
            dfs(computers, i, visited)
            answer += 1

    return answer

def dfs(computers, start, visited):
    if visited[start]:
        return

    visited[start] = True

    for i in range(len(computers[0])):
        if computers[start][i] == 1 and not visited[i]:
            dfs(computers, i, visited)
```

## 풀이2(DFS - 스택 이용)

```python
answer = 0
def solution(n, computers):
    global answer
    visited = [False] * n
    for i in range(n):
        if not visited[i]:
            dfs(computers, i, visited)

    return answer

def dfs(computers, start, visited):
    global answer

    st = [start]

    while st:
        com_num = st.pop()
        if not visited[com_num]:
            visited[com_num] = True

        for i in range(len(computers[0])):
            if not visited[i] and computers[com_num][i] == 1:
                st.append(i)

    answer += 1
```
