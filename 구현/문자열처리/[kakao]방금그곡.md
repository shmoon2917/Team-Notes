# 방금그곡

## 링크

[방금그곡](https://programmers.co.kr/learn/courses/30/lessons/17683#)

- 프로그래머스 Lv2
- 2018 KAKAO BLIND RECRUITMENT

## 문제 설명

라디오를 자주 듣는 네오는 라디오에서 방금 나왔던 음악이 무슨 음악인지 궁금해질 때가 많다. 그럴 때 네오는 다음 포털의 '방금그곡' 서비스를 이용하곤 한다. 방금그곡에서는 TV, 라디오 등에서 나온 음악에 관해 제목 등의 정보를 제공하는 서비스이다.

네오는 자신이 기억한 멜로디를 가지고 방금그곡을 이용해 음악을 찾는다. 그런데 라디오 방송에서는 한 음악을 반복해서 재생할 때도 있어서 네오가 기억하고 있는 멜로디는 음악 끝부분과 처음 부분이 이어서 재생된 멜로디일 수도 있다. 반대로, 한 음악을 중간에 끊을 경우 원본 음악에는 네오가 기억한 멜로디가 들어있다 해도 그 곡이 네오가 들은 곡이 아닐 수도 있다. 그렇기 때문에 네오는 기억한 멜로디를 재생 시간과 제공된 악보를 직접 보면서 비교하려고 한다. 다음과 같은 가정을 할 때 네오가 찾으려는 음악의 제목을 구하여라.

- 방금그곡 서비스에서는 음악 제목, 재생이 시작되고 끝난 시각, 악보를 제공한다.
- 네오가 기억한 멜로디와 악보에 사용되는 음은 C, C#, D, D#, E, F, F#, G, G#, A, A#, B 12개이다.
- 각 음은 1분에 1개씩 재생된다. 음악은 반드시 처음부터 재생되며 음악 길이보다 재생된 시간이 길 때는 음악이 끊김 없이 처음부터 반복해서 재생된다. 음악 길이보다 재생된 시간이 짧을 때는 처음부터 재생 시간만큼만 재생된다.
- 음악이 00:00를 넘겨서까지 재생되는 일은 없다.
- 조건이 일치하는 음악이 여러 개일 때에는 라디오에서 재생된 시간이 제일 긴 음악 제목을 반환한다. 재생된 시간도 같을 경우 먼저 입력된 음악 제목을 반환한다.
- 조건이 일치하는 음악이 없을 때에는 `(None)`을 반환한다.

**입력 형식**

입력으로 네오가 기억한 멜로디를 담은 문자열 `m`과 방송된 곡의 정보를 담고 있는 배열 `musicinfos`가 주어진다.

- `m`은 음 `1`개 이상 `1439`개 이하로 구성되어 있다.
- `musicinfos`는 `100`개 이하의 곡 정보를 담고 있는 배열로, 각각의 곡 정보는 음악이 시작한 시각, 끝난 시각, 음악 제목, 악보 정보가 '`,`'로 구분된 문자열이다.
  - 음악의 시작 시각과 끝난 시각은 24시간 `HH:MM` 형식이다.
  - 음악 제목은 '`,`' 이외의 출력 가능한 문자로 표현된 길이 `1` 이상 `64` 이하의 문자열이다.
  - 악보 정보는 음 `1`개 이상 `1439`개 이하로 구성되어 있다.

**출력 형식**

조건과 일치하는 음악 제목을 출력한다.

**입출력 예**

|         m          |                         musicinfos                         | answer  |
| :----------------: | :--------------------------------------------------------: | :-----: |
|     "ABCDEFG"      | ["12:00,12:14,HELLO,CDEFGAB", "13:00,13:05,WORLD,ABCDEF"]  | "HELLO" |
| "CC#BCC#BCC#BCC#B" |  ["03:00,03:30,FOO,CC#B", "04:00,04:08,BAR,CC#BCC#BCC#B"]  |  "FOO"  |
|       "ABC"        | ["12:00,12:14,HELLO,C#DEFGAB", "13:00,13:05,WORLD,ABCDEF"] | "WORLD" |

<br></br>

## 내 풀이

- 풀긴 풀었으나 깔끔하지 못한 느낌임. 시간도 오래 걸림.
- 더 발전합시다!

```python
def solution(m, musicinfos):
    candidates = []
    order = 1

    for idx, music in enumerate(musicinfos):
        start, end, title, sheet = music.split(',')
        mins = getMinutes(start, end) # 재생 시간
        melody_list = splitMelody(sheet) # 멜로디 리스트
        m_list = splitMelody(m) # 기억한 멜로디 리스트

        # 멜로디가 재생 시간보다 길다면
        if len(melody_list) > mins:
            melody_list = melody_list[:mins]
        # 멜로디가 재생 시간보다 짧다면
        elif len(melody_list) < mins:
            melody_list = (melody_list * (mins//len(melody_list))) + melody_list[:mins%len(melody_list)]

        if len(melody_list) < len(m_list): continue

        for i in range(len(melody_list) - len(m_list) + 1):
            matcher = melody_list[i:i + len(m_list)]
            if "".join(m_list) == "".join(matcher):
                candidates.append([mins, title, order])
                order += 1
                break


    if len(candidates) == 0:
        return "(None)"
    elif len(candidates) == 1:
        return candidates[0][1]
    else:
        candidates.sort(key=lambda x:x[0], reverse=True)
        candidates = [c for c in candidates if c[0] == candidates[0][0]]
        if len(candidates) == 1:
            return candidates[0][1]
        else:
            candidates.sort(key=lambda x:x[2])
            return candidates[0][1]

def splitMelody(s):
    melody_list = []
    for mel in s:
        if mel == '#':
            melody_list[-1] += '#'
        else:
            melody_list.append(mel)
    return melody_list

def getMinutes(start, end):
    mins = 0
    if start[0:2] == end[0:2]:
        mins = int(end[3:]) - int(start[3:])
    else:
        mins += int(end[3:]) + (60 - int(start[3:])) + (int(end[0:2]) - int(start[0:2]) - 1) * 60

    return mins
```
