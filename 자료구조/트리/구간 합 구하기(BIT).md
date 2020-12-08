# 구간 합 구하기

## 링크

[구간 합 구하기](https://www.acmicpc.net/problem/2042)

- 백준

## 문제 설명

어떤 N개의 수가 주어져 있다. 그런데 중간에 수의 변경이 빈번히 일어나고 그 중간에 어떤 부분의 합을 구하려 한다. 만약에 1,2,3,4,5 라는 수가 있고, 3번째 수를 6으로 바꾸고 2번째부터 5번째까지 합을 구하라고 한다면 17을 출력하면 되는 것이다. 그리고 그 상태에서 다섯 번째 수를 2로 바꾸고 3번째부터 5번째까지 합을 구하라고 한다면 12가 될 것이다.

**입력**

첫째 줄에 수의 개수 N(1 ≤ N ≤ 1,000,000)과 M(1 ≤ M ≤ 10,000), K(1 ≤ K ≤ 10,000) 가 주어진다. M은 수의 변경이 일어나는 횟수이고, K는 구간의 합을 구하는 횟수이다. 그리고 둘째 줄부터 N+1번째 줄까지 N개의 수가 주어진다. 그리고 N+2번째 줄부터 N+M+K+1번째 줄까지 세 개의 정수 a, b, c가 주어지는데, a가 1인 경우 b(1 ≤ b ≤ N)번째 수를 c로 바꾸고 a가 2인 경우에는 b(1 ≤ b ≤ N)번째 수부터 c(b ≤ c ≤ N)번째 수까지의 합을 구하여 출력하면 된다.

입력으로 주어지는 모든 수는 -263보다 크거나 같고, 263-1보다 작거나 같은 정수이다.

**출력**

첫째 줄부터 K줄에 걸쳐 구한 구간의 합을 출력한다. 단, 정답은 -263보다 크거나 같고, 263-1보다 작거나 같은 정수이다.

**입출력 예**

```python
5 2 2
1
2
3
4
5
1 3 6
2 2 5
1 5 2
2 3 5

# 출력
17
12
```

<br></br>

## 풀이

- 참고: [2진수의 음수 표현](https://m.blog.naver.com/PostView.nhn?blogId=chgy2131&logNo=140186696889&proxyReferer=https:%2F%2Fwww.google.com%2F)

```python
import sys
input = sys.stdin.readline

# 데이터의 개수(n), 변경 횟수(m), 구간 합 계산 횟수(k)
n, m, k = map(int, input().split())

# 전체 데이터의 개수는 최대 1,000,000개
arr = [0] * (n + 1)
tree = [0] * (n + 1)

# i번째 수까지의 누적 합을 계산하는 함수
def prefix_sum(i):
  result = 0
  while i > 0:
    result += tree[i]
    # 0이 아닌 마지막 비트만큼 빼가면서 이동
    i -= (i & -i)
  return result

# i번째 수를 dif만큼 더하는 함수
def update(i, dif):
  while i <= n:
    tree[i] += dif
    i += (i & -i)

# start부터 end까지의 구간 합을 계산하는 함수
def interval_sum(start, end):
  return prefix_sum(end) - prefix_sum(start - 1)

for i in range(1, n + 1):
  x = int(input())
  arr[i] = x
  update(i, x)

for _ in range(m + k):
  a, b, c = map(int, input().split())
  # 업데이트(update) 연산인 경우
  if a == 1:
    update(b, c - arr[b]) # 바뀐 크기(dif)만큼 적용
    arr[b] = c
  else:
    print(interval_sum(b, c))
```
