---
title: "JVM 과 자바 코드의 실행 "
date: 2021-02-17T12:30:27+09:00
draft: true
categories: [Java_Study]
tags: [Java, JVM, JDK JRE]
---

# 1주차 : JVM 과 자바 코드의 실행

## 📌 목표

자바 소스 파일(.java)을 JVM으로 실행하는 과정 이해하기.

## 📌 **학습할 것**

### ▶️ 메소드, 클래스

- **클래스**

  **:** 자바의 가장 작은 단위 메소드의 소속

  ⇒ `상태 (state)` 와 `행동 (behavior)`

  - 상태 : 클래스의 특성을 결정짓는 것 ex. variable (변수)

  - 행동 : 메소드

- **메소드**

  **:** 어떤 값을 주고 결과를 넣어주는 것

### **▶️ JVM (Java Virtual Machine)**

![컴파일 원리](/static/image/jvm.jpg)

: `자바의 가상 머신 (스택 기반)` / 자바 프로그램 실행 환경 만들어주는 소프트웨어

- .java 파일로 생성돼있는 소스코드를 Java Compiler를 통해 .class파일로 변환한 것을, 해당 OS와의 중재자 역할로 OS가 이해할수 있도록 .class ByteCode를 해석해주는 역할.

- 자바 장점 : Window/ Linux OS에 종속되지 않고 .java 파일만 생성해주면 어떤 운영체제에든 JVM을 통해 실행 가능.

- 한정된 메모리 효율에서 최대의 성능을 내기 위한 메모리 관리를 위해 자바 가상머신이 중요하다.

### **▶️ JVM 구성 요소**

**I. Class Loader**

: 런타임 시점에 클래스를 로딩하게 해주며 클래스의 인스턴스를 생성하면 클래스 로드를 통해 메모리에 로드

- `Loading`

  - Bootstrap :

  - Extension

  - Application

- `Linking`

  - Verify

  - Prepare

  - Resolve

- `Initialization`(초기화) = static 변수값 할당하고 static 블럭 실행

**II. Execution Engine**

- `Interpreter` : 바이트 코드를 명령어 단위로 실행하여 한줄씩 수행. 단점 : 느리다.
- `JIT(Just in time) Compiler` : Interpreter 보완; 네이티브 코드로 컴파일된 코드 캐시 보관하여 실행
- `Garbage Collector` : 동적으로 할당된 메모리중 사용하지 않는 메모리 반환

**III. Runtime Data Area**

- `Method Area`
- `Heap`
- `Java Thread (Stack)`
- `Program Counter Register`
- `Native Internal Thread (Stack)`

### **▶️ 컴파일 하는 방법**

소스코드(.java) —> Compiler -(bytecode: .class 변환) -> Disk -(bytecode)-> JVM -(기계어) -> OS

### **▶️ 자바 프로그램 실행 과정**

1. 프로그램 실행
2. JVM이 OS로 부터 메모리를 할당 받고, 용도에 따라 나누어 관리
3. `자바 컴파일러(javac)`가 소스 코드(_.java)를 읽어 `바이트 코드`로 변환(_.class)
4. JVM의 `Class Loader`가 변환된 파일(\*.class)들을 로드
5. `Execution engine`(실행 엔진) 은 로딩된 class 파일을 해석
6. 해석된 파일들은 `Runtime Data Area`(할당 메모리)에 배치되고 실질적 수행이 이루어짐

### **▶️ 바이트코드란 무엇인가**

: 컴퓨터가 이해할수 있도록 0 과 1 로 구성되어있는 코드

- 운영체제에 맞는 완벽한 기계어가 아닌 중간기계어

- 이 바이트코드를 실행하기 위해 JVM 필요

⭐ Java Compiler

: .java파일로 생성돼있는 소스코드를 이진수로 구성되어있는 .class 라는 Java Byte Code로 변환 시켜줌.

### **▶️ JIT 컴파일러란 무엇이며 어떻게 동작하는지**

: 처음 자바는 Interpreter방식으로 모든 코드를 컴파일 하였고 그때문에 속도가 느리다는 평가를 받았다.

JIT(Just in Time) 컴파일러는 하드웨어의 발전으로 native 코드로 직접 실행하는 방식으로 CPU + OS (기계)가 직접 읽고 수행하는 방식이다.

- 네이티브 캐쉬에 한번 컴파일된 코드를 보관하기 때문에 같은 코드 반복시, 매번 기계어 코드를 새로 생성하는 방식이 아닌 이전에 만든 기계어를 재사용함으로써 속도를 빠르게 했다.

- 운영체제에 맞게 byte 실행 코드를 한번에 변환하여 실행하여 이전 Java Interpreter방식보다 성능이 10-20배 증가했다고 한다.

**⭐ Interpreter or JIT Compiler**

- 무조건 JIT Comipler방식이 옳은건 아니다...

: 한번만 실행되는 코드라면 `Interpreter`!

메소드의 반복 수행이 일정 정도를 넘는다면 `JIT Compiler`!

### **▶️ JDK와 JRE의 차이**

![JDK](/static/image/jdk.jpg)

- `JDK (Java Development Kit)`

  : 자바 프로그래밍 도구

  - 개발을 위한 도구 (javac, java)

  - JDK 다운시, JRE도 함께 설치 ; `JDK = JRE + @`

  오픈소스 연동에 더 풍부한/최적화된 기능을 빠르게 구현하는 강점

- 기본 기능 제공 클래스

- 자료구조

- 네트워크

- 입출력

- 예외처리

- Algorithm Library 제공

- `JRE (Java Runtime Environment)`

  : 컴파일된 자바 프로그램 실행하는 자바 실행 환경 - 해당 OS에 맞는 환경 설치

  - JVM이 자바 프로그램 동작시킬때 필요한 라이버리 / 실행 환경 구현
