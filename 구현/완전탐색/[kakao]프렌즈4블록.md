# 프렌즈4블록

## 링크

[프렌즈4블록](https://programmers.co.kr/learn/courses/30/lessons/17679#)

- 프로그래머스 Lv2
- 2018 KAKAO BLIND RECRUITMENT

## 문제 설명

블라인드 공채를 통과한 신입 사원 라이언은 신규 게임 개발 업무를 맡게 되었다. 이번에 출시할 게임 제목은 `프렌즈4블록`.
같은 모양의 카카오프렌즈 블록이 2×2 형태로 4개가 붙어있을 경우 사라지면서 점수를 얻는 게임이다.

<img src="http://t1.kakaocdn.net/welcome2018/pang1.png" width="50%"></img>

만약 판이 위와 같이 주어질 경우, 라이언이 2×2로 배치된 7개 블록과 콘이 2×2로 배치된 4개 블록이 지워진다. 같은 블록은 여러 2×2에 포함될 수 있으며, 지워지는 조건에 만족하는 2×2 모양이 여러 개 있다면 한꺼번에 지워진다.

<img src="http://t1.kakaocdn.net/welcome2018/pang2.png" width="50%"></img>

블록이 지워진 후에 위에 있는 블록이 아래로 떨어져 빈 공간을 채우게 된다.

<img src="http://t1.kakaocdn.net/welcome2018/pang3.png" width="50%"></img>

만약 빈 공간을 채운 후에 다시 2×2 형태로 같은 모양의 블록이 모이면 다시 지워지고 떨어지고를 반복하게 된다.

<img src="http://t1.kakaocdn.net/welcome2018/pang4.png" width="50%"></img>

위 초기 배치를 문자로 표시하면 아래와 같다.

```
TTTANT
RRFACC
RRRFCC
TRRRAA
TTMMMF
TMMTTJ
```

각 문자는 라이언(R), 무지(M), 어피치(A), 프로도(F), 네오(N), 튜브(T), 제이지(J), 콘(C)을 의미한다

입력으로 블록의 첫 배치가 주어졌을 때, 지워지는 블록은 모두 몇 개인지 판단하는 프로그램을 제작하라.

**제한 사항**

- 입력으로 판의 높이 `m`, 폭 `n`과 판의 배치 정보 `board`가 들어온다.
- 2 ≦ `n`, `m` ≦ 30
- `board`는 길이 `n`인 문자열 `m`개의 배열로 주어진다. 블록을 나타내는 문자는 대문자 A에서 Z가 사용된다.
- 입력으로 주어진 판 정보를 가지고 몇 개의 블록이 지워질지 출력하라.

**입출력 예**

|  m  |  n  |                      board                       | answer |
| :-: | :-: | :----------------------------------------------: | :----: |
|  4  |  5  |           [CCBDE, AAADE, AAABF, CCBBF]           |   14   |
|  6  |  6  | [TTTANT, RRFACC, RRRFCC, TRRRAA, TTMMMF, TMMTTJ] |   15   |

<br></br>

## 내 풀이

```python
def solution(m, n, board):
    answer = 0
    board = [list(i) for i in board]
    start, change = True, False
    while start or change:
        if start: start = False

        move(m, n, board) # 지워진 블록 이동
        change, removed = remove(m, n, board) # 4블록 제거
        answer += removed # 제거된 블록 개수 추가
    return answer

def move(m, n, board):
    for i in range(m-1, -1, -1):
        for j in range(n-1, -1, -1):
            if board[i][j] == '0':
                k = i - 1
                while k >= 0:
                    if board[k][j].isalpha():
                        board[i][j], board[k][j] = board[k][j], board[i][j]
                        break
                    k -= 1

def remove(m, n, board):
    change = False
    removed = 0
    will_change = []
    for i in range(m-1):
        for j in range(n-1):
            if board[i][j] != '0' and (board[i][j] == board[i][j+1] == board[i+1][j] == board[i+1][j+1]):
                will_change += [[i,j], [i, j+1], [i+1, j], [i+1, j+1]]
                change = True
    for x, y in will_change:
        if board[x][y] != '0':
            removed += 1
            board[x][y] = '0'

    return change, removed
```
