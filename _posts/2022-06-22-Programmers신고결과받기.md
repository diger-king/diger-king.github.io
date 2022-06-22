---

title: Programmers Level 1. 2022 KAKAO BLIND RECRUITMENT 신고 결과 받기 (Python)
layout: post
description: 코딩테스트 연습 2022 KAKAO BLIND RECRUITMENT 신고 결과 받기
post-image: https://programmers.co.kr/assets/img-meta-programmers-e00862a7c9acd8ef5164f8c85b3ab0127d083ab59b3a98d7219690bd3570bf35.png

tags:
- Algorithm
- Implementation

---

# 문제 설명

신입사원 무지는 게시판 불량 이용자를 신고하고 처리 결과를 메일로 발송하는 시스템을 개발하려 합니다. 무지가 개발하려는 시스템은 다음과 같습니다.

    각 유저는 한 번에 한 명의 유저를 신고할 수 있습니다.
        신고 횟수에 제한은 없습니다. 서로 다른 유저를 계속해서 신고할 수 있습니다.
        한 유저를 여러 번 신고할 수도 있지만, 동일한 유저에 대한 신고 횟수는 1회로 처리됩니다.
    k번 이상 신고된 유저는 게시판 이용이 정지되며, 해당 유저를 신고한 모든 유저에게 정지 사실을 메일로 발송합니다.
        유저가 신고한 모든 내용을 취합하여 마지막에 한꺼번에 게시판 이용 정지를 시키면서 정지 메일을 발송합니다.

### 다음은 전체 유저 목록이 ["muzi", "frodo", "apeach", "neo"]이고, k = 2(즉, 2번 이상 신고당하면 이용 정지)인 경우의 예시입니다.

|유저 ID   |유저가 신고한 ID|설명|
|-----|---|---|
|"muzi" |"frodo" |"muzi"가 "frodo"를 신고했습니다. |
|"apeach" |"frodo" |"apeach"가 "frodo"를 신고했습니다. |
|"frodo" |"neo" |"frodo"가 "neo"를 신고했습니다.|
|"muzi" |"neo" |"muzi"가 "neo"를 신고했습니다.|
|"apeach" |"muzi" |"apeach"가 "muzi"를 신고했습니다.|

### 각 유저별로 신고당한 횟수는 다음과 같습니다.


|유저 ID| 신고당한 횟수 |
|---|---------|
|"muzi"| 	1      |
|"frodo"| 	2      |
|"apeach"| 0       |
|"neo"| 2       |

위 예시에서는 2번 이상 신고당한 "frodo"와 "neo"의 게시판 이용이 정지됩니다. 이때, 각 유저별로 신고한 아이디와 정지된 아이디를 정리하면 다음과 같습니다.

| 유저 ID                     | 유저가 신고한 ID       | 정지된 ID           |
|---------------------------|------------------|------------------|
| "muzi"                    | ["frodo", "neo"] | ["frodo", "neo"] |
| "frodo"| ["neo"]| ["neo"]          |
|"apeach"|["muzi", "frodo"]|["frodo"]|
|"neo"|없음|없음|

따라서 "muzi"는 처리 결과 메일을 2회, "frodo"와 "apeach"는 각각 처리 결과 메일을 1회 받게 됩니다.

이용자의 ID가 담긴 문자열 배열 id_list, 각 이용자가 신고한 이용자의 ID 정보가 담긴 문자열 배열 report, 정지 기준이 되는 신고 횟수 k가 매개변수로 주어질 때, 각 유저별로 처리 결과 메일을 받은 횟수를 배열에 담아 return 하도록 solution 함수를 완성해주세요.

---

# 제한사항

    2 ≤ id_list의 길이 ≤ 1,000
        1 ≤ id_list의 원소 길이 ≤ 10
        id_list의 원소는 이용자의 id를 나타내는 문자열이며 알파벳 소문자로만 이루어져 있습니다.
        id_list에는 같은 아이디가 중복해서 들어있지 않습니다.
    1 ≤ report의 길이 ≤ 200,000
        3 ≤ report의 원소 길이 ≤ 21
        report의 원소는 "이용자id 신고한id"형태의 문자열입니다.
        예를 들어 "muzi frodo"의 경우 "muzi"가 "frodo"를 신고했다는 의미입니다.
        id는 알파벳 소문자로만 이루어져 있습니다.
        이용자id와 신고한id는 공백(스페이스)하나로 구분되어 있습니다.
        자기 자신을 신고하는 경우는 없습니다.
    1 ≤ k ≤ 200, k는 자연수입니다.
    return 하는 배열은 id_list에 담긴 id 순서대로 각 유저가 받은 결과 메일 수를 담으면 됩니다.


---
# 풀이 (오답)

처음에는 아래와 같이 2중 반복문으로 풀었다.

    def solution(id_list, report, k):
    
        id_dict = {item: 0 for item in id_list}
        reported_dict = {item:0 for item in id_list}
        report = set(report)
    
        answer = list()
        blacklist = {}
    
        # report_item 에는, "muzi frodo", "apeach frodo" ... 등 중복이 제거된, 공백으로 구분된 신고 정보가 담겨있음
        for report_item in report:
            
            # reported_dict 에 신고된 아이디를 Key 로 잡고, 그 Value 또한 1씩 증가시킴
            reported_dict[report_item.split()[1]] += 1
    
        # reported_item 는, reported_dict 의 Key 값을 하나씩 순회함
        for reported_item in reported_dict:
            # Key 값을 통해서 Value를 얻었을 때 그 값이 k 이상이면, 그 아이디를 가지고 블랙리스트 처리
            if reported_dict[reported_item] >= k:
                blacklist[reported_item] = reported_dict[reported_item]
    
        for report_item in report:
            for black in blacklist:
                if report_item.split()[1] == black:
                    id_dict[report_item.split()[0]] += 1
    
        return list(id_dict.values())

하지만 역시나, 알고리즘 문제에서 2중반복문이 나오는 경우는 대부분 좋지 않은 경우였던것 같다.

마찬가지로 24가지 케이스 중 2가지 케이스에서 시간초과가 났다.

따라서 위 코드는 별도로 해설하지 않고, 내 문제점을 기록해 놓는 것으로 하겠다.

---

# 풀이(정답)

    def solution(id_list, report, k):
    
        id_dict = {item: 0 for item in id_list}
        reported_dict = {item:0 for item in id_list}
        report = set(report)
    
        # report_item 에는, "muzi frodo", "apeach frodo" ... 등 중복이 제거된, 공백으로 구분된 신고 정보가 담겨있음
        for report_item in report:
            # reported_dict 에 신고된 아이디를 Key 로 잡고, 그 Value 또한 1씩 증가시킴
            reported_dict[report_item.split()[1]] += 1
    
        for report_item in report:
            # 위 반복문에서 셋팅한 딕셔너리에
            # reported_dict[report_item.split()[1] 에는 신고당한 유저들 아이디가 있다.
            # 그래서, reported_dict 에서 신고당한 아이디를 Key 가지는 Value 를 뽑았을 때 그게 K보다 크면
            if reported_dict[report_item.split()[1]] >= k:
                # id_dict 에서 Key 에 해당하는 Value 를 1 증가시킨다.
                id_dict[report_item.split()[0]] += 1
    
        return list(id_dict.values())

### 변수
id_dict : 매개변수로 받은 id_list 를 딕셔너리 타입으로 형변환, Value = 0 으로 전체 초기화

reported_dict : id_dict 와 같음, 하지만 id_dict 는 출력을 위한 쓰임새라면 이 변수는, 실제 신고 당한 유저의 카운트를 담는 변수이다. Value = 0  으로 전체 초기화

report : 매개변수로 받은 report 를 중복을 제거한 변수이다.

### 반복문 1.

report 에 담긴 원소를 하나씩 순회할 것이다.

이때, report 의 원소에는 "muzi frodo", "apeach frodo" ... 의 꼴로 담겨있고

reported_dict의 Key 값에 report의 원소를 공백을 기준으로 split() 한 뒷부분, 즉 신고당한 유저를 넣고 그 Value 를 +1 한다.

이렇게 되면, 신고당한 유저의 Key값을 가진 reported_dict 에 신고 당한 횟수만큼 Value 가 지정된 딕셔너리가 완성된다. 

### 반복문 2.

report 에 담긴 원소를 하나씩 순회할 것이다.

반복문 1. 에서 셋팅한 딕셔너리를 활용할 것인데,

reported_dict 의 Key 값으로, report 의 순회하는 원소중, 뒷부분을 가져온다. 즉 신고당한 유저의 아이디를 Key로 가지는 reported_dict 의 Value 와

K 값을 비교한다. 그래서, K 보다 크다면

신고한 유저의 아이디를 가리키는 report_item.split()[0] 을 Key 값으로 가지는 id_dict 의 Value 를 +1 해준다.

### Return

---