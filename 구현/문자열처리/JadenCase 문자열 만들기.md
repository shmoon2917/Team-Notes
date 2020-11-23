# JadenCase 문자열 만들기

## 링크

[JadenCase 문자열 만들기](https://programmers.co.kr/learn/courses/30/lessons/12951#)

- 프로그래머스 Lv2

## 문제 설명

JadenCase란 모든 단어의 첫 문자가 대문자이고, 그 외의 알파벳은 소문자인 문자열입니다. 문자열 s가 주어졌을 때, s를 JadenCase로 바꾼 문자열을 리턴하는 함수, solution을 완성해주세요.

**제한 사항**

- s는 길이 1 이상인 문자열입니다.
- s는 알파벳과 공백문자(" ")로 이루어져 있습니다.
- 첫 문자가 영문이 아닐때에는 이어지는 영문은 소문자로 씁니다. ( 첫번째 입출력 예 참고 )

**입출력 예**

|            s            |         return          |
| :---------------------: | :---------------------: |
| "3people unFollowed me" | "3people Unfollowed Me" |
|   "for the last week"   |   "For The Last Week"   |

<br></br>

## 나의 풀이

- 문제에 대해 가능한 케이스를 확보해보자

```python
def solution(s):
    data = list(s)
    isFirst = True
    for i in range(len(data)):
        if data[i] == " ":
            isFirst = True
            continue
        if isFirst:
            if data[i].isalpha():
                data[i] = data[i].upper()
            isFirst = False
        else:
            data[i] = data[i].lower()

    return "".join(data)
```
