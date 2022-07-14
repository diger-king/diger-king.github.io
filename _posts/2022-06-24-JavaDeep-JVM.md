---

title: JVM - Java Virtual Machine
layout: post
description: JVM 조금 더 알기
post-image: https://www.programmersought.com/images/95/45af3109700753d7ccafb0e83efaaa7f.JPEG

tags:
- Java
- JVM

---

# JVM

![JVM: Architecture](https://www.programmersought.com/images/95/45af3109700753d7ccafb0e83efaaa7f.JPEG)

---

# JVM 이란?

<br> 

> Java Code 나 Application 을 실행시키기 위한 런타임 환경을 제공해주는 엔진이다.

> Java 코드를 ByteCode 로 변환해준다.

> JVM 은 Java Runtime Environment(JRE)의 일부이다.

> 다른 언어의 컴파일러와는 다르게, Java 컴파일러는 JVM 이 인식할 수 있는 코드를 생성해낸다.

---

# JVM 기능

> Java 코드를 ByteCode 로 변환한다. (ByteCode 는 인터프리팅 된다.)
>
> JVM 은 메모리 공간 할당을 담당한다.
> 
> Java Code <-> JVM <-> System

# Working of Virtual Machine (JVM)

> Class Loader -> Byte Code Verifier -> Execution Engine

---

# JVM 구조
![JVM: Architecture](https://www.programmersought.com/images/95/45af3109700753d7ccafb0e83efaaa7f.JPEG)

# Class Loader
클래스 파일을 로딩하기 위한 하위 시스템이다.

세 가지 주요 기능이 있다.
    **Loading**, **Linking**, **Initialization**
---

# Class Loader 기능 수행 과정

## Loading

클래스 로더는 .class 파일을 읽고, 바이너리 데이터를 생성하여 메소드 영역에 저장한다.

또한 각 .class 파일 마다 JVM은 아래의 세 가지 정보를 메서드 영역에 저장한다.

<br>

로드된 클래스와 그 모든 연관 부모 클래스

.class 파일이 Class or Interface, Enum 과 관련 있는 것인지 체크 후 저장

수정자, 변수와 메서드 정보 등

> 즉 .class 파일, 로드된 클래스와 연관있는 모든 것들을 JVM 메서드 영역에 저장한다.

<br>

.class 를 로딩을 마친 후에, JVM 은 Heap 영역에 이 파일들을 나타내기 위해

Class 유형의 객체를 생성한다. 이렇게 만들어진 Class 객체를

getClass() 등의 메서드로 코드 관점에서도 사용이 가능한 것이다.

---

## Linking
    
Verification, Preparation, Resolution 을 수행한다.

> **Verification**
>
> .class 파일이 올바른 형식으로 지정되고, 유효한 컴파일러에 의해 생성되었는지 확인한다.
> 
> 확인에 실패 했을 때 RuntimeException 혹은 java.lang.VerifyError 가 발생한다.
> 
> 이 과정이 끝났다면, 클래스 파일을 컴파일 할 준비가 완료 된 것이다.

> **Preparation**
> 
> 클래스 변수에 메모리를 할당하고 메모리를 기본값으로 초기화한다.

> **Resolution**
> 
> 간접 참조 방식을 직접 참조 방식으로 바꾸는 과정이다.
> 
> 참조된 엔티티를 찾기 위하여 메서드 영역을 검색한다.

---

## Initialization

> 모든 static 변수가 코드 및 static block 에 정의된 값으로 할당된다. (static 키워드가 들어간 요소들 메모리 할당)
>
> 부모에서 자식 순으로 수행된다.
>
> --> static 요소들 메모리 할당 --> 부모 클래스 메모리 할당 --> 자식 클래스 메모리 할당 --> 기타 요소들 할당
    

---


# Class Loader 의 종류

![Class Loader](https://github.com/MoveForword-Java/Diger-King/blob/main/src/Week3/img_1.png?raw=true)

## [ Bootstrap ]

모든 JVM 은 반드시 Bootstrap 클래스 로더를 구현하여, 신뢰성이 있는 loading 된 클래스를 사용할 수 있다.

JAVA_HOME/jre/lib 디렉터리에 존재하는 Java 핵심 API 클래스들을 로드 한다.

Bootstrap 은 C, C++ 과 같은 Native 언어로 구현된다.

> 근본 경로에 존재하는 클래스를 로딩

---

## [ Extension class loader ]

Bootstrap class loader 의 자식으로, JAVA_HOME/jre/lib/ext (확장경로) 또는 

java.ext.dirs 시스템 속성으로 지정된 기타 디렉터리에 있는 클래스를 로드한다.

> 확장 경로에 존재하는 클래스를 로딩

---

## [ System-Application class loader ]

extension class loader 의 자식으로, application 클래스 경로에 존재하는 클래스를 로드한다.

내부적으로 java.class.path 에 매핑된 환경 변수를 사용한다.

> 애플리케이션 경로에 존재하는 클래스를 로딩

---
<br>

지금까지는 JVM 의 구성요소인 Class Loader 를 살펴 보았으니, JVM 의 메모리를 구성하는 것들은 무엇이 있는지 살펴본다.

처음 올렸던 JVM Architecture 이미지가 잊혀졌을 수도 있기 때문에 다시 첨부한다.

<br>
---


# JVM Memory Element

![JVM: Architecture](https://www.programmersought.com/images/95/45af3109700753d7ccafb0e83efaaa7f.JPEG)

## Runtime Data Area
    
> **Method Area**
>
> 클래스 구조(클래스 이름, 부모 클래스 이름, 메서드 및 변수 정보) 등 **클래스 수준의 모든 정보**를 담고 있다.
> 
> **static 변수**를 가지고 있다.
> 
> JVM 당 **하나의 메서드 영역**을 가지고 있고, **공유 될 수 있는** 영역이다.

> **Heap**
> 
> 모든 **객체 및 연관된 인스턴스 변수/배열이 저장**되는 장소이다.
> 
> 이 공간은 **공유될 수 있다.**
> 
> 추후 **GC 의 대상**이 되는 동적인 영역이다.

> **Java Threads(Stack Area)**
> 
> JVM은 **모든 쓰레드에, 각 하나의 런타임 스택을 제공**한다.
> 
> 각 스택의 모든 블럭은 메서드 호출이 발생하면 이를 스택에 저장한다.
> 
> 지역 변수가 이곳에 저장된다.
> 
> **공유 가능**한 영역이다
> 
> 쓰레드가 메서드를 실행 -> 메서드를 쓰레드의 스택에 담음 -> 메서드 POP -> 메서드와 관련된 지역 번수들을 스택에 PUSH -> POP ... PUSH 후 메서드 종료

> **PC Register**
> 스레드가 현재 실행중인 JVM 명령의 주소를 저장한다. 각 스레드 별로 별도의 PC 레지스터가 있다.

> **Native Internal Threads (Native Method Stacks)**
> 
> Java 가 아닌 다른 언어로 작성된 Native Code 의 명령을 저장한다.

---

## Runtime Area 요약

### Method 영역 (공유 가능)
1. static 키워드로 선언된 요소들
2. 클래스 이름, 부모클래스, 클래스 메서드, 클래스 변수 등

### Heap (공유 가능)
1. 인스턴스
2. 인스턴스 변수

GC가 참조 되지 않은 메모리를 확인하고 제거하는 영역

### Thread (Stack) (공유 가능)
1. 메서드 내에서 사용되는 값들을 저장(매개변수, 메서드에서 선언한 변수, 리턴값 등)
2. 지역변수(메서드에 의한 변수)


---

## JVM 메모리 구조 도식화

![img_2.png](https://github.com/MoveForword-Java/Diger-King/raw/main/src/Week3/img_2.png)

---

## Execution Engine

> JIT Compier 와 GC 가 존재하는 영역, Native Code (타 언어로 만들어진 메서드 등) 을 다루는 영역

---

## 실행 엔진

**실행 엔진은 .class 파일을 실행**시켜준다.

또한 **byte 코드를 각 행마다 읽어들이며**, **메모리 공간에 존재하는 데이터와 정보들을 이용**하며 명령어를 실행시켜준다.

실행 엔진은 **세 부분으로 분류** 할 수 있다.

## Interpreter

**바이트 코드를 한줄 씩 해석한 다음 실행**한다.

이 부분의 단점으로는, **하나의 메서드를 여러번 호출** 할 경우, **매번 해석**을 해줘야한다.

## Just-In-Time Compiler(JIT)

인터프리터의 효율을 증가시키기 위해 사용된다.

**전체 바이트 코드를 컴파일**하여 **네이티브 코드(기계어)로 변경**하므로, 

인터프리터가 **반복되는 메서드 호출을 볼 때마다**

**JIT 에서 해당 부분에 관한 네이티브 코드를 제공**하므로

**재 해석이 필요하지 않아** **효율성을 증가**시켜주는 방법이다.

## Garbage Collector

**참조되지 않은 객체를 정리**해준다.

---

### 다음 정리할 내용 -> GarbageCollection(JVM Heap Changed by Java 7 to 8)

![anj](https://www.programmersought.com/images/272/8608bc9a09bac541ced5d76135cf3188.JPEG)

![Java7 vs Java 8](https://www.programmersought.com/images/220/8462f4bfba5f173585f6f8cd2dce12ac.JPEG)