---

title: Data Structure 2. Queue
layout: post
description: Python 에서 Queue 사용하기
post-image: https://w3cschoool.com/public/file/Python/python-stack-and-queue-3.png

tags:
- Data Structure

---


# 큐 구현

[앞선 포스트에서](https://diger-king.github.io/blog/Stack) 스택을 직접 구현을 해보았지만 생산성이 떨어진다는 결론으로 Python에서 제공하는 라이브러리를 활용하기로 했다.

자료구조를 조금이라도 공부를 해보았다면 Queue 동작 방식은 익히 알고 있을 것이므로 설명은 생략하고

Python 에서 Queue 를 통한 문제해결이 필요할땐 어떻게 해결하는지 알아보겠다.

![Queue](https://w3cschoool.com/public/file/Python/python-stack-and-queue-3.png)
---

## Queue Library

    import queue
    
    queue = queue.Queue()

    # 데이터 삽입 (Queue 속성에 맞추어 삽입됨)
    queue.put(1)
    queue.put(2)
    queue.put(3)

    # 데이터 꺼내기 (Queue 속성에 맞추어 꺼냄)
    queue.get()

---

[앞선 포스트에서](https://diger-king.github.io/blog/Stack) 사용한 Deque 를 언급했을 때 Double-Enable-**Queue** 라고 했던 것을 본다면

이제는 Queue 라이브러리를 써야하느냐, 아니면 Queue 라이브러리를 써야하는가를 비교해보는 것이 더 좋을 것이다.

사실 두 자료구조는 한 뿌리에서 나온 것이라고 볼 수 있기 때문이다.

---

# Queue vs Deque

## Queue
> Queue 는 서로 다른 스레드간 통신을 위해 메세지/데이터를 담은 용도로 사용되는 자료구조이다.


## Deque
> Deque 는 우리가 흔히 생각하는 자료구조로써 사용되는 것이다. (단순하게 데이터 넣고 빼고 찾고 등등...)

>그래서 Queue 의 메서드에는 스레드간 통신을 위한 put_nowait(), get_nowait(), join() 등..
>
>노골적으로 스레드를 위한 자료구조임을 나타내주는 메서드가 존재한다.

아래 글을 참고하면 더 좋은 의견들에 대한 정보를 얻을 수 있다.

[StackOverflow : Queue vs Deque](https://stackoverflow.com/questions/717148/queue-queue-vs-collections-deque)

## 성능 비교

Queue = O(n) vs Deque = O(1)

---

이 글을 정리하고자 하는 목적은 스레드를 활용하기 위한 자료구조가 아닌

데이터를 삽입/삭제/탐색 등 조작을 위한 자료구조를 정리하는 것이 목적이었다.

그래서 다시 한 번 Deque 를 살펴보겠다.

---

    from collections import deque
    
    deque = deque()
    
    deque.append("Test Data")
    deque.appendleft("Test Data Insertion At LeftSide")
    deque.pop()
    deque.popleft()
    deque.clear()
    deque.copy()
    deque.count(해당 값과 일치하는 원소의 개수를 계산)

    # ++ 이전 포스팅에서 추가된 사항
    deque.remove(삭제하고 싶은 값)
    deque.extend(배열) --> 해당 배열을 순환하며, 오른쪽에 변수를 추가한다.
    deque.extendleft(배열) --> 해당 배열을 순환하며, 왼쪽에 변수를 추가한다.
    deque.rotate(숫자) --> 해당 숫자 만큼 deque 를 회전시킨다.

---