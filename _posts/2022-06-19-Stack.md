---

title: Data Structure 1. Stack
layout: post
description: Python 에서 Stack 사용하기
post-image: https://user-images.githubusercontent.com/13075035/72049492-62471c00-3302-11ea-961f-9aea3b5d98df.png

tags:
- Data Structure

---

# 스택 구현

    # 스택을 구현할 때 필요한 메서드
    
    # 1. 스택 생성
    # 2. 스택 크기 반환
    # 3. 스택 원소를 문자욜로 변환
    # 4. Push
    # 5. Pop
    # 6. Peek
    # 7. Clear
    # 8. IsEmpty
    # 9. IsFull
    # 10. Size
    
    
    class Stack:
    
        # 스택 생성
        # limit 은 최대 스택의 크기를 담은 변수
        def __init__(self, limit: int):
            self.top = []
            self.limit = limit
    
        # 스택 크기 반환
        # -> bool : 타입 힌트로,
        def __len__(self) -> bool:
            return len(self.top)
    
        # 스택 내용 문자열 타입으로 변환
        def __str__(self) -> str:
            return str(self.top[::1])
    
        # 데이터 추가하기
        def push(self, item):
            if len(self.pop()) >= self.limit:
                self.top.append(item)

        # 데이터 꺼내기
        def pop(self):
            if not self.isEmpty():
                return self.top.pop(-1)
            else:
                print("Stack Underflow")
                exit()
    
        # 스택 맨 위의 값을 반환
        def peek(self):
            if not self.isEmpty():
                return self.top[-1]
            else:
                print("underflow")
                exit()
    
        # 스택이 비어있는가?
        def isEmpty(self) -> bool:
            return len(self.top) == 0
    
        # 스택을 비움
        def clear(self):
            self.top=[]
    
        # 스택 안에 특정 item이 포함되어 있는지를 bool값으로 반환
        def isContain(self, item) -> bool:
            return item in self.top
    
        # 스택이 비어있는 지를 bool값으로 반환
        def isEmpty(self) -> bool :
            return len(self.top)==0
    
        # 스택이 가득 차 있는 지를 bool값으로 반환
        def isFull(self) -> bool :
            return self.size()==self.limit
    
        # 스택의 크기를 int 값으로 반환
        def size(self) -> int :
            return len(self.top)


---

분명히 위 자료구조를 구현하면서 좋은점도 있을 것이다. 하지만 스택을 사용하려고할 때

약 50줄이 넘는 이 코드를 항상 구현하고 사용할 순 없는 노릇이다. 그래서 보통 사용하는 라이브러리가 있다.

---

# Python 에서 Stack 을 간편하게 사용하기

---

## collections Library's Deque

![Deque](https://mblogthumb-phinf.pstatic.net/MjAxOTA0MThfMjcx/MDAxNTU1NTMxOTk3NjM1.5AEXPRQeNYX8dxSzgTCiATI-xprr8WIQY52DVXk91_gg.lgVBOfBws5gg9nYlLEuotRHLLQv-exifkTNPxdb06YUg.PNG.sooftware/anod.png?type=w800)

그림과 같이, 좌측과 우측 으로 데이터를 삽입-삭제가 가능하다.

Deque 는 Double-Ended-Queue 를 의미한다.

---

    from collections import deque
    
    deque = deque()
    
    deque.append("Test Data")
    deque.appendleft("Test Data Insertion At LeftSide")
    deque.pop()
    deque.popleft()
    deque.clear()
    deque.copy()
    deque.count("해당 값과 일치하는 원소의 개수를 계산")

---

### 데이터 추가 하기
> deque.append()

### 데이터 추가 하기 (가장 좌측에 데이터 추가)
> deque.appendleft()

### 데이터 꺼내기
> deque.pop()

### 데이터 꺼내기 (가장 좌측에 데이터를 꺼낸다)
> deque.popleft()

### 특정 원소의 갯수 찾기
> deque.count(찾고 싶은 원소의 값)

### 원소 모두 삭제하기
> deque.clear()

### 원소 모두 복제하기
> deque.copy()

---

Deque 를 활용하면 Stack 의 기능을 사용할 수도 있고

deque의 메서드를 활용하여 직접 필요한 기능을 만들어

확장성을 높일 수 있는 장점이 있다.

---

# Stack 구현 문제적용

https://www.acmicpc.net/problem/10828

위 문제는, 스택을 구현하여 풀이해야하는 문제이다. 이를 적용시켜 보겠다.

---


## 문제

정수를 저장하는 스택을 구현한 다음, 입력으로 주어지는 명령을 처리하는 프로그램을 작성하시오.

명령은 총 다섯 가지이다.

    push X: 정수 X를 스택에 넣는 연산이다.
    pop: 스택에서 가장 위에 있는 정수를 빼고, 그 수를 출력한다. 만약 스택에 들어있는 정수가 없는 경우에는 -1을 출력한다.
    size: 스택에 들어있는 정수의 개수를 출력한다.
    empty: 스택이 비어있으면 1, 아니면 0을 출력한다.
    top: 스택의 가장 위에 있는 정수를 출력한다. 만약 스택에 들어있는 정수가 없는 경우에는 -1을 출력한다.

## 입력

첫째 줄에 주어지는 명령의 수 N (1 ≤ N ≤ 10,000)이 주어진다. 둘째 줄부터 N개의 줄에는 명령이 하나씩 주어진다. 주어지는 정수는 1보다 크거나 같고, 100,000보다 작거나 같다. 문제에 나와있지 않은 명령이 주어지는 경우는 없다.

## 출력

출력해야하는 명령이 주어질 때마다, 한 줄에 하나씩 출력한다.

---

# 풀이


    import sys
    
    
    input = sys.stdin.readline
    stack = list()
    
    n = int(input())
    instruction = ""
    value = ""
    
    for i in range(n):
        input_value = input()
    
        if " " in input_value:
            instruction = input_value.split(" ")[0]
            value = input_value.split(" ")[1]
        else:
            instruction = input_value.strip('\n')
    
    
        if instruction == "push":
            stack.append(value)
    
        elif instruction == "pop":
            if len(stack) == 0:
                print("-1")
            else:
                print(stack[len(stack)-1].split("\n")[0])
                stack.pop()
    
        elif instruction == "top":
            if len(stack) == 0:
                print("-1")
            else:
                print(stack[len(stack)-1].split("\n")[0])
    
        elif instruction == "size":
            print(len(stack))
    
        elif instruction == "empty":
            if len(stack) == 0:
                print(1)
            else:
                print(0)
        else:
            print("잘못 입력")

---
print(stack[len(stack)-1].split("\n")[0])


이 부분이 풀면서 가장 의아했다.

왜인지 개행문자까지 반영되어 출력되어 출력형태가 다르게 되어 strip() 으로 개행문자를 제거해줬다.