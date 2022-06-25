---

title: Data Structure 4. Dictionary(HashMap)
layout: post
description: Python 에서 Dictionary(HashMap) 사용하기
post-image: https://i0.wp.com/masterprograming.com/wp-content/uploads/2019/10/python-dictionary.jpg?w=602&ssl=1

tags:
- Data Structure

---

# Dictionary

Java 의 HashMap 과 동일한 개념이다.

Key : Value 형태를 가진다.

---

# Dictionary - 사용법

    # 선언
    dict = {}
    dict = dict()

    #################################
    # 선언 및 초기화

    dict = {item: 0 for item in list}
    dict = dict(diger = 24, song = 17)
    
    #################################
    # 추가

    dict["do-song"] = 122

    #################################
    # 읽기

    print(dict) # Key-Value 모두 출력
    
    ## Key 읽기
    for item in dict:
        print(item)


    ## Value 읽기
    for item in dict:
        print(dict[item])

    #################################
    # 변경

    dict[diger] = 1009
    dict[song] = 618

    #################################
    # 제거

    del dict[diger]

    #################################
    # 추가적인 메서드

    ## dict.keys()
        Key 값들을 Return 한다.

    ## dict.values()
        Values 값들을 Return 한다.

    ## dict.get()
        해당하는 Key 에 대한 Value 를 Return 한다. 이때
        dict[key] 로 접근할 수도 있는데, get() 메서드를 사용하면, NPE 를 피할 수 있다.

    ## 특정 Key 가 존재하는가?
        **if Key in dict:**
            print(dict[Key])

    ## dict.items()
        Key-Value 의 Tuple 을 Return 한다.

    #################################

---

# Dictionary 문제적용

https://www.acmicpc.net/problem/10816

위 문제는, 딕셔너리 활용하여 풀이해야하는 문제이다. 이를 적용시켜 보겠다.

---


## 문제

숫자 카드는 정수 하나가 적혀져 있는 카드이다. 
상근이는 숫자 카드 N개를 가지고 있다. 정수 M개가 주어졌을 때,
이 수가 적혀있는 숫자 카드를 상근이가 몇 개 가지고 있는지 구하는 프로그램을 작성하시오.

## 입력



첫째 줄에 상근이가 가지고 있는 숫자 카드의 개수 N(1 ≤ N ≤ 500,000)이 주어진다. 둘째 줄에는 숫자 카드에 적혀있는 정수가 주어진다. 숫자 카드에 적혀있는 수는 -10,000,000보다 크거나 같고, 10,000,000보다 작거나 같다.

셋째 줄에는 M(1 ≤ M ≤ 500,000)이 주어진다. 넷째 줄에는 상근이가 몇 개 가지고 있는 숫자 카드인지 구해야 할 M개의 정수가 주어지며, 이 수는 공백으로 구분되어져 있다. 이 수도 -10,000,000보다 크거나 같고, 10,000,000보다 작거나 같다.


## 출력



첫째 줄에 입력으로 주어진 M개의 수에 대해서, 각 수가 적힌 숫자 카드를 상근이가 몇 개 가지고 있는지를 공백으로 구분해 출력한다.

## 예제 입력 1

10
6 3 2 10 10 10 -10 -10 7 3
8
10 9 -5 2 3 4 5 -10

## 예제 출력 1

3 0 0 1 2 0 0 2

---

# 덱 풀이, Count 메서드 호출 과 직접 카운팅(2중 반복문) [오답]

자료구조 deque 또한 시간 복잡도는 O(1) 이므로 deque 으로 풀이했었다.

하지만 원소를 count 를 하는 과정에서 O(n) 의 시간복잡도를 가지므로 

For-Loop 와 중첩되어 O(n^2) 이 되어버려 시간초과가 나던 풀이이다.

    import sys
    
    from collections import deque
    
    input = sys.stdin.readline
    
    # --------------------------------------------------
    
    n = int(input())
    sang_geun = deque(map(int, input().split()))
    
    m = int(input())
    target = deque(map(int, input().split()))
    
    # --------------------------------------------------

    # count 메서드로 카운팅
    answer = deque()
    
    for i in range(m):
        answer.append(sang_geun.count(target[i]))
    
    for item in answer:
        print(item, end=" ")
    
    # --------------------------------------------------

    # 2중 반복문으로 카운팅
    answer = deque(0 for i in range(m))
    
    for i in range(n):
        for j in range(m):
            if sang_geun[i] == target[j]:
                answer[j] += 1
    
    for item in answer:
        print(item, end=" ")
    
    # --------------------------------------------------

# 덱 풀이, Count 메서드 미호출(직접 카운팅) (오답)

이 풀이는 인덱스에 직접 접근하며 풀이를 했지만, 

    import sys
    
    input = sys.stdin.readline
    
    n = int(input())
    sang_geun = list(map(int, input().split()))
    
    m = int(input())
    target = list(map(int, input().split()))
    
    correct = {item: 0 for item in sang_geun}
    
    for item in sang_geun:
        if item in target:
            # correct[item] = correct[item] + 1
            correct[item] = correct.get(item) + 1
    
    for item in correct:
        print(item, end=" ")





# 딕셔너리 풀이 [오답]

    import sys
    
    input = sys.stdin.readline
    
    n = int(input())
    sang_geun = list(map(int, input().split()))
    
    m = int(input())
    target = list(map(int, input().split()))
    
    correct = {item: 0 for item in sang_geun}
    
    for item in sang_geun:
        if item in correct:
            correct[item] += 1
    
    # 정답 풀이와 다른 부분
    for item in correct:
        if item in target:
            print(correct[item], end=" ")
        else:
            print(0, end=" ")

# 딕셔너리 풀이 [정답]


    import sys
    
    input = sys.stdin.readline
    
    n = int(input())
    sang_geun = list(map(int, input().split()))
    
    m = int(input())
    target = list(map(int, input().split()))
    
    correct = {item: 0 for item in sang_geun}
    
    for item in sang_geun:
        if item in correct:
            correct[item] += 1
    
    # 오답 풀이와 다른 부분
    for item in target:
        if item in correct:
            print(correct[item], end=" ")
        else:
            print(0, end=" ")

---
# 해설 - 정/오답 비교

    for item in correct:

    for item in target:

correct 를 순회하는 반복문에서 item 에는, correct 의 Key 값 즉, 상근이가 가지고있는 Key를 순차적으로 순회한다.

target 을 순회하는 반복문에서 item 에는, target 의 Key 값 즉, 상근이 카드와 비교할 카드들의 Key를 순차적으로 순회한다.


위에서 이미 correct 딕셔너리에 각 값을 카운팅 해두었다.

이 때 Key값으로

correct 의 Key 를 통하여, 출력을 해야하는지
target 의 Key 를 통하여, 출력을 해야하는지 구분해야한다.

<br>

**correct 의 Key 를 통하여 출력을 하게 된다면**, 이미 카운팅 한 정보를 담고있는 딕셔너리에서, target 에 해당하는 Key 가 있는지를 확인하고

그 조건이 부합해야 출력을 하게된다.

즉, 카운팅 한 딕셔너리를 또 카운트 하게 되는 것이다.

<br>

**target 의 Key 를 통하여 출력을 하게 된다면,** 타겟 카드 뭉치 리스트를 하나씩 순회하면서

그 리스트 값에 해당하는 Key를 가진 Correct 딕셔너리의 Value 를 출력하게된다.
