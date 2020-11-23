# 수식 최대화

## 링크

[수식 최대화](https://programmers.co.kr/learn/courses/30/lessons/67257)

- 프로그래머스 Lv2
- 카카오 인턴

## 문제 설명

- IT 벤처 회사를 운영하고 있는 라이언은 매년 사내 해커톤 대회를 개최하여 우승자에게 상금을 지급하고 있습니다. 이번 대회에서는 우승자에게 지급되는 상금을 이전 대회와는 다르게 다음과 같은 방식으로 결정하려고 합니다.

- 해커톤 대회에 참가하는 모든 참가자들에게는 숫자들과 3가지의 연산문자 `(+, -, *)` 만으로 이루어진 연산 수식이 전달되며, 참가자의 미션은 전달받은 수식에 포함된 연산자의 우선순위를 자유롭게 재정의하여 만들 수 있는 가장 큰 숫자를 제출하는 것입니다.

- 단, 연산자의 우선순위를 새로 정의할 때, 같은 순위의 연산자는 없어야 합니다. 즉, `+` > `-` > `*` 또는 `-` > `*` > `+` 등과 같이 연산자 우선순위를 정의할 수 있으나 `+,*` > `-` 또는 `*` > `+,-`처럼 2개 이상의 연산자가 동일한 순위를 가지도록 연산자 우선순위를 정의할 수는 없습니다. 수식에 포함된 연산자가 2개라면 정의할 수 있는 연산자 우선순위 조합은 2! = 2가지이며, 연산자가 3개라면 3! = 6가지 조합이 가능합니다.

- 만약 계산된 결과가 음수라면 해당 숫자의 절댓값으로 변환하여 제출하며 제출한 숫자가 가장 큰 참가자를 우승자로 선정하며, 우승자가 제출한 숫자를 우승상금으로 지급하게 됩니다.

- 참가자에게 주어진 연산 수식이 담긴 문자열 expression이 매개변수로 주어질 때, 우승 시 받을 수 있는 가장 큰 상금 금액을 return 하도록 solution 함수를 완성해주세요.

**제한 사항**

- expression은 길이가 3 이상 100 이하인 문자열입니다.
- expression은 공백문자, 괄호문자 없이 오로지 숫자와 3가지의 연산자 `(+, -, *)` 만으로 이루어진 올바른 중위표기법(연산의 두 대상 사이에 연산기호를 사용하는 방식)으로 표현된 연산식입니다. 잘못된 연산식은 입력으로 주어지지 않습니다.
  - 즉, `"402+-561*"`처럼 잘못된 수식은 올바른 중위표기법이 아니므로 주어지지 않습니다.
- expression의 피연산자(operand)는 0 이상 999 이하의 숫자입니다.
  - 즉, `"100-2145*458+12"`처럼 999를 초과하는 피연산자가 포함된 수식은 입력으로 주어지지 않습니다.
  - `"-56+100"`처럼 피연산자가 음수인 수식도 입력으로 주어지지 않습니다.
- expression은 적어도 1개 이상의 연산자를 포함하고 있습니다.
- 연산자 우선순위를 어떻게 적용하더라도, expression의 중간 계산값과 최종 결괏값은 절댓값이 263 - 1 이하가 되도록 입력이 주어집니다.
- 같은 연산자끼리는 앞에 있는 것의 우선순위가 더 높습니다.

**입출력 예**

|      expression       | return |
| :-------------------: | :----: |
| "100-200\*300-500+20" | 60420  |
|     "50\*6-3\*2"      |  300   |

<br></br>

## 내 풀이(순열 및 완전탐색으로 구현)

```python
from itertools import permutations
def solution(expression):
    answer = 0
    # operator = []
    operand = ''
    new_exp = []
    # 피연산자와 연산자로 구분
    for i in expression:
        if '0' <= i <= '9':
            operand += i
            continue
        else:
            new_exp.append(operand)
            operand = ''
            new_exp.append(i)
    new_exp.append(operand)

    # 연산자 순열 구하기
    operator = [x for x in ['+', '-', '*'] if x in new_exp]
    opt_perm = list(permutations(operator, len(operator)))

    copy_exp = []
    max_num, num = 0, 0
    for opts in opt_perm:
        copy_exp = new_exp
        num = 0
        for opt in opts:
            while opt in copy_exp:
                idx = copy_exp.index(opt)
                num = calculator(copy_exp[idx-1], copy_exp[idx+1], opt)
                copy_exp = copy_exp[0:idx-1] + [num] + copy_exp[idx+2:]
        # print('후보 값', num)
        if max_num <= abs(num):
            max_num = abs(num)
        # print('지금까지 최대값', max_num)

    return max_num

def calculator(a, b, str):
    a = int(a)
    b = int(b)
    if str == '*':
        return a * b
    elif str == '+':
        return a + b
    elif str == '-':
        return a - b
```

## 다른 풀이

- 정규표현식 사용

```python
import re
from itertools import permutations

def solution(expression):
    #1
    op = [x for x in ['*','+','-'] if x in expression]
    op = [list(y) for y in permutations(op)]
    ex = re.split(r'(\D)',expression)

    #2
    a = []
    for x in op:
        _ex = ex[:]
        for y in x:
            while y in _ex:
                tmp = _ex.index(y)
                _ex[tmp-1] = str(eval(_ex[tmp-1]+_ex[tmp]+_ex[tmp+1]))
                _ex = _ex[:tmp]+_ex[tmp+2:]
        a.append(_ex[-1])

    #3
    return max(abs(int(x)) for x in a)
```
