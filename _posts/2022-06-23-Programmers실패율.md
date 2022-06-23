---

title: Programmers Level 1. 2019 KAKAO BLIND RECRUITMENT 실패율 (Python)
layout: post
description: 코딩테스트 연습 2019 KAKAO BLIND RECRUITMENT 실패율
post-image: https://camo.githubusercontent.com/1f22eb52cdb65d272b543fddc2bbaa92e46e46bb65c69e6512693ce9579e9e02/68747470733a2f2f70726f6772616d6d6572732e636f2e6b722f6173736574732f696d672d6d6574612d70726f6772616d6d6572732d653030383632613763396163643865663531363466386338356233616230313237643038336162353962336139386437323139363930626433353730626633352e706e67

tags:
- Algorithm
- Implementation

---


# 문제 설명

실패율

![실패율](https://grepp-programmers.s3.amazonaws.com/files/production/bde471d8ac/48ddf1cc-c4ea-499d-b431-9727ee799191.png)

슈퍼 게임 개발자 오렐리는 큰 고민에 빠졌다. 그녀가 만든 프랜즈 오천성이 대성공을 거뒀지만, 요즘 신규 사용자의 수가 급감한 것이다. 원인은 신규 사용자와 기존 사용자 사이에 스테이지 차이가 너무 큰 것이 문제였다.

이 문제를 어떻게 할까 고민 한 그녀는 동적으로 게임 시간을 늘려서 난이도를 조절하기로 했다. 역시 슈퍼 개발자라 대부분의 로직은 쉽게 구현했지만, 실패율을 구하는 부분에서 위기에 빠지고 말았다. 오렐리를 위해 실패율을 구하는 코드를 완성하라.

    실패율은 다음과 같이 정의한다.
        스테이지에 도달했으나 아직 클리어하지 못한 플레이어의 수 / 스테이지에 도달한 플레이어 수

전체 스테이지의 개수 N, 게임을 이용하는 사용자가 현재 멈춰있는 스테이지의 번호가 담긴 배열 stages가 매개변수로 주어질 때, 실패율이 높은 스테이지부터 내림차순으로 스테이지의 번호가 담겨있는 배열을 return 하도록 solution 함수를 완성하라.

---

## 제한사항

    스테이지의 개수 N은 1 이상 500 이하의 자연수이다.
    stages의 길이는 1 이상 200,000 이하이다.
    stages에는 1 이상 N + 1 이하의 자연수가 담겨있다.
        각 자연수는 사용자가 현재 도전 중인 스테이지의 번호를 나타낸다.
        단, N + 1 은 마지막 스테이지(N 번째 스테이지) 까지 클리어 한 사용자를 나타낸다.
    만약 실패율이 같은 스테이지가 있다면 작은 번호의 스테이지가 먼저 오도록 하면 된다.
    스테이지에 도달한 유저가 없는 경우 해당 스테이지의 실패율은 0 으로 정의한다.

---

## 입출력 예

|N| 	stages| 	result      |
|---|---|--------------|
|5 	|[2, 1, 2, 6, 2, 4, 3, 3]| 	[3,4,2,1,5] |
|4| 	[4,4,4,4,4]|  	[4,1,2,3]  |

---

## 입출력 예 설명

### 입출력 예 #1

1번 스테이지에는 총 8명의 사용자가 도전했으며, 이 중 1명의 사용자가 아직 클리어하지 못했다. 따라서 1번 스테이지의 실패율은 다음과 같다.

    1 번 스테이지 실패율 : 1/8

2번 스테이지에는 총 7명의 사용자가 도전했으며, 이 중 3명의 사용자가 아직 클리어하지 못했다. 따라서 2번 스테이지의 실패율은 다음과 같다.

    2 번 스테이지 실패율 : 3/7

마찬가지로 나머지 스테이지의 실패율은 다음과 같다.

    3 번 스테이지 실패율 : 2/4
    4번 스테이지 실패율 : 1/2
    5번 스테이지 실패율 : 0/1

각 스테이지의 번호를 실패율의 내림차순으로 정렬하면 다음과 같다.

    [3,4,2,1,5]

---

### 입출력 예 #2

모든 사용자가 마지막 스테이지에 있으므로 4번 스테이지의 실패율은 1이며 나머지 스테이지의 실패율은 0이다.

    [4,1,2,3]

---

# 풀이

    def solution(n, stages):
        fail_rate = {}
        sub = len(stages)
    
        for item in range(1, n + 1):
            if sub != 0:
                fail_rate[item] = stages.count(item) / sub
                sub -= stages.count(item)
            else:
                fail_rate[item] = 0
    
        return sorted(fail_rate, key=lambda item: fail_rate[item], reverse=True)


풀릴듯 말듯 잘 안 풀리는 문제라 시간이 너무 오래걸려서 다른 풀이를 참고했다.

---

## 변수

fail_rate : 실패율을 담을 딕셔너리, Key = Stage 번호, Value = 클리어 하지 못한 사람 수

sub = Stage 의 갯수

--

## For-Loop 1.

    for item in range(1, n+1):

1 부터, n+1 까지 순회한다. 이 반복문에서 item은, 각 스테이지의 번호를 나타낸다.

<br>

    for item in range(1, n+1):
        if sub != 0:

sub 가 0이 아니면, 즉 실패율을 계산해야할 다른 스테이지가 남이있는 경우에 수행한다.

<br>

    for item in range(1, n+1):
        if sub != 0:
            fail_rate[item] = stages.count(item) / sub

실패율 딕셔너리의 Key 값을 item, 즉 현재 순회하고 있는 스테이지의 번호를 Key 로 넣는다.

그 Key 의 Value 에는, 각 스테이지에 해당하는 실패율을 넣어줄 건데,

실패율은, 스테이지에 도달했으나 아직 클리어하지 못한 플레이어 수를 뜻한다.

그러니까, Stages 에 들어있는 숫자는 클리어 중인 스테이지 번호를 나타내므로 각 번호가 몇갠지 세면, 그 스테이지를 아직 클리어 하지 못한 유저의 수를 알 수 있는 것이다.

그래서 이 각 숫자를 **item** 즉, 스테이지 번호마다 회차하면서 카운팅을 한다.

그리고 이 카운트한 수를 이전 단계에 있는 사람을 제외하고 나누어 주면 실패율이 된다.

<br>

    for item in range(1, n+1):
        if sub != 0:
            fail_rate[item] = stages.count(item) / sub
            sub -= stage.count(item)

실패율을 구했으면, 그 스테이지에서 멈춰있는 사람만큼 sub 에다가 빼준다.

왜냐하면, 그 스테이지에 머무르고있는 사람은 다음 스테이지 실패율 계산에 포함되면 안되기 때문이다.

<br>

    for item in range(1, n+1):
        if sub != 0:
            fail_rate[item] = stages.count(item) / sub
            sub -= stage.count(item)
        else:
            fail_rate[item] = 0

sub == 0 인 상황은, 모든 유저가 진행중인 스테이지를 다 확인 한 것이므로

그때의 실패율은 0 으로 한다.

## Return - Lambda

    return sorted(fail_rate, key=lambda item: fail_rate[item], reverse=True)

실패율 리스트를 리턴하되, 람다식을 활용하여 정렬까지 마치고 리턴한다.

---

## Dictionary Sort

<br>

### Key 를 기준으로 오름차순 정렬
sorted_dict = sorted(fail_rate)

### Key 를 기준으로 내림차순 정렬
sorted_dict = sorted(fail_rate, key = lambda item: item[0], reverse = True)

### Value 를 기준으로 오름차순 정렬
sorted_dict = sorted(fail_rate, key = lambda item: item[1])

### Key 를 기준으로 내림차순 정렬
sorted_dict = sorted(fail_rate, key = lambda item: item[1], reverse = True)