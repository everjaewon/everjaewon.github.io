---
layout: single
title:  "프로세스 동기화(2/2) Process Synchronization(2/2)"
categories: Os
tags: [Os, ComputerScience]
toc: true

---

<br><br>

※ 지난 내용에 이어서 고수준(high-level) 동기화 방법들부터 학습한다.

<br>

# 세마포어

 \- 원리

  ① lock보다 높은 수준의 동기화 방법 (‘busy waiting’ 없음)

  ② 원자적 : 분리되지 않는 두 작업(③)으로 조작됨

  ③ wait(s) : 세마포어 s가 열릴 때까지 block하기 / P()

​     signal(s) : 다른 애가 들어오도록 허용하기 / S()

​    <br><br><br>

​    

## 세마포어 구현

 \- OS 지원

  : ‘바쁜 대기’를 방지하기 위해 최신 OS에서 제공됨

​    wait(), signal() 작업의 정의를 수정<br>

 \- 세마포어를 ‘기록 자료구조’로 정의

  : 각 세마포어는 프로세스/스레드 관련 queue를 갖고 있다 (리스트 & 정수값)<br>

 \- 두 가지 간단한 작업을 가정

  ① block() : 얘 호출하는 프로세스를 중단

  ② wakeup(P) : 중단된 프로세스 P의 실행 재개<br>

 \- 세마포어 연산

  ① 프로세스가 세마포어를 기다려야 하는 경우 : 프로세스 중단 & 세마포어의 큐에 배치됨

  ② 큐에 있는 프로세스 하나 빼고, 걔를 ready 프로세스 리스트에 추가함

 <br>
 <br>
 

### 원자성

  : 아무리 multi CPU 환경이어도 wait(S)와 signal(S)에 동시에 두 프로세스가 있을 수 없음

​    → wait(S)와 signal(S)는 코드 정의부이므로 이건 CS 문제임.

​       단, 몇 가지 명령어로만 구성된 매우 짧은 CS임.<br><br>

 ※ 해결법

  \- Uniprocessor : 이 작업 중에(= 매우 짧은 기간동안) 인터럽트를 사용하지 않음

  \- Multiprocessor : 소프트웨어/하드웨어 spinlock 방식을 사용 (= 바쁜 대기)

​    <br><br><br>

## 세마포어의 유형

① 이진 세마포어 (= **뮤텍스**)

 \- 리소스에 대한 ‘상호 배제’적 접근 보장

 \- 한 번에 하나의 프로세스/스레드만 입장 가능

 \- Counter 초기화는 1로<br>

② 카운팅 세마포어

 \- 많은 unit을 갖춘 리소스일 경우에 사용

 \- unit 사용 가능한 만큼 스레드/프로세스가 입장 가능

 \- Counter 초기화는 ‘사용 가능 unit의 개수’로<br><br><br>

## 세마포어의 문제점

 \- 세마포어는 ‘글로벌 변수’ → 프로그램 內 어디서나 접근 가능

 \- CS와 스케줄링 모두에 쓰이기 때문에 지나치게 강력

 \- 사용에 대한 통제 or 적절한 쓰임에 대한 보장 X

  : wait과 signal이 여러 프로세스에 분산되어 있으므로 효과를 이해하기 어려움

   프로세스 하나의 잘못이 전체 프로세스들의 실패로 직결 가능

   결론 :  사용하기 어려움 & 버그 생기기 쉬움!

​    <br><br><br>

​    

## Critical Region

 \- 세마포어의 잘못된 사용이 오류를 불러오므로 더 고수준의 방법을 요함: 그래서 나옴.

 \- 동일한 공유 변수를 참조하는 region에서, 시간 내에 서로 제외

 \- ‘region v when B do S’로만 공유 변수 v에 접근 가능

 \- B가 true면 S가 실행되고, false면 B가 true될 때까지 프로세스 연기됨.

  <br><br><br><br>  

​    

# 모니터

 \- 원칙

  ① 공유 데이터에 대한 제어된 접근을 지원하는 PL 구조

​    : 동기화 코드 → 컴파일러가 추가하고, 런타임에 강화됨

​      동시 프로세스들 간 추상 데이터 타입의 안전한 공유 가능

  ② ‘공유 자료구조 & 공유데이터에서의 작동절차 &

​     그 절차를 호출하는 동시 프로세스 간의 동기화’를 캡슐화하는 소프트웨어 모듈

  ③ unstructured된 접근으로부터 데이터를 보호 : 절차를 거쳐야만 데이터에 접근 가능

  ④ 여러 동시 PL에서 사용 가능

  ⑤ 세마포어에 의해 구현 가능<br>

 \- 상호 배제<br>

 \- 조건 변수

  : 프로그래머가 조건 변수를 이용해 계속하지 못하는 프로세스가 wait할지, 아니면 다른 프로세스에게 signal할지 선택 가능

  : 이벤트를 wait하는 메커니즘 (= 랑데부 포인트) 제공

  \- wait(c) : 다른 놈이 입장하도록 모니터 lock을 해제

​             다른 놈이 signal할 때까지 wait

​             조건 변수가 wait queue를 갖고 있음

  \- signal(c) : 최대 1개의 waiting 프로세스를 깨움

​               waiting 프로세스가 없으면 signal이 손실됨<br><br><br><br>

# Pthreads에서의 동기화

 \- “조건 변수 + 뮤텍스” : 모니터 없이도 뮤텍스와 함께 엮여 쓰일 수 있음

 \- 절차의 어느 부분에서든 세분화(?) 가능

​    <br><br><br><br>

​    

# Message Passing

 \- 원칙

  : IPC에 사용되는 일반적인 방법 (동일 PC 內 프로세스 / 분산 시스템 內 프로세스)

  : 프로세스 동기화와 상호 배제를 제공할 또다른 수단

  : send, received의 최소 2가지 요소로 구성, 둘 다 프로세스가 block될 수도 안 될 수도.<br>

 \- 동기화

  ① sender : send() 쓴 이후 block되지 않는 것이 일반적

​    → 여러 목적지에 여러 메시지 보내기 가능

​    → 메시지 수신 확인을 기대함

  ② receiver : receive() 쓴 이후 block되는 것이 일반적

​    → 계속하기 전에 정보 필요

​    → send() 이전에 sender 프로세스가 실패하면 무기한 block될 수 있음

  ③ send, receive 다 block하는 가능성도 존재

​    → 메시지 수신될 때까지 둘 다 block

​    → communication link가 unbuffered되었을 때 발생

​    → 매우 긴밀한 동기화 제공<br>

 \- Direct Communication / Indirect Communication

​    <br><br><br><br>

# OS에서의 스레드, 커널 동기화

<br><br>

## Solaris2 스레드 동기화

 \- 다양한 locks를 실행 : 멀티태스킹, 멀티스레딩, 멀티프로세싱 지원 목적

 \- 뮤텍스 locks / adaptive 뮤텍스 locks / Reader-Writer Locks

 \- 조건 변수들 존재

​    <br><br>

## 리눅스 스레드 동기화  

Pthread 동기화

​    <br><br>

## 윈도우 스레드 동기화  

뮤텍스 Locks / 세마포어 / 이벤트

<br><br><br>

## 리눅스 커널 동기화

커널

 : 프로세스 or 외부기기의 요청에 응답하는 서버

 : 직렬이 아닌 인터리브 방식으로 실행

 : 일부 계산 결과가 둘 이상의 인터리브 커널 제어 경로가 얼마나 중첩되었느냐에 달렸을 때

 : Critical region은 각 커널 제어 경로에 의해 완전히 실행되어야 하는 코드 부분임  <br>

목표 : 커널 자료구조의 보호

 : 공유 데이터 간의 윗윗윗줄 경우를 피할 수 있을 때, 커널 제어 경로들이 인터리브되는 메커니즘을 제공

​    <br><br>

### 리눅스 커널에서의 동기화 메커니즘

 ① 원자적 작업들

  : 작업이 칩(chip) 레벨에서 atomic함을 보장

 ② 인터럽트 비활성 : cli() & sti() 이용, 일련의 커널 statements가 CS에서 동작함을 보장

 ③ Locking (spinlock : 멀티프로세서에서만 / 커널 세마포어 : 멀티&유니 고루 사용)
<br><br><br><br><br>
# <span style="color:yellow">핵심 질문</span>
⑴ 세마포어, 뮤텍스, 모니터에 대해 설명하시오<br>
<br><br><br><br>
⑴<br>세마포어 자체는 정수 변수로, 원자성을 갖춘 두 함수로 세마포어 값을 조작하여 busy waiting 현상을 방지하는 동기화의 방식을 취한다. wait 함수를 통해 CS에 접근을 시도하고, CS에 접근한 이후에는 signal 함수를 이용하여 대기 중인 다른 프로세스가 접근할 수 있도록 허용하는데, wait하는 프로세스가 세마포어를 기다려야 하는 경우 해당 프로세스를 중단하고 세마포어가 가진 대기열에 배치하며, 이후 signal을 받으면 선입선출 등의 공정한 방법으로 대기열의 프로세스를 하나 꺼내 접근 준비 리스트에 추가한다.<br><br>
공유 리소스가 한 번에 하나의 프로세스/스레드만 접근 가능한지, 사용 가능한 unit의 수만큼 접근 가능한지에 따라 각각 이진 세마포어, 계수 세마포어로 유형을 분류하는데, 전자의 경우를 뮤텍스라고도 부른다. 리소스에 대한 상호 배제적 접근을 보장하며, 최대 한 개의 프로세스만 리소스에 접근할 수 있으므로, 카운터는 1로 초기화한다.<br><br>
세마포어를 조작하는 두 함수가 여러 프로세스에 분산되어 있고, 또한 세마포어 자체가 글로벌 변수이기에 프로그램의 어느 부분에서든 접근이 가능하다는 점이 있으며, 프로세스 하나에서의 잘못된 세마포어 사용이 전체 프로세스의 실패로 직결될 가능성이 있다. 이 때문에 세마포어는 사용하기 어렵고 버그가 생기기 쉽다는 문제점을 갖는다.<br><br>
이러한 문제점을 보완하는 동기화 방법 중 하나가 모니터로, 공유 객체, 공유 데이터에서 작동하는 프로시져들, 해당 프로시져들을 작동시키는 동시 프로세스들 간의 동기화를 하나로 캡슐화한 소프트웨어 모듈이다. 특정 공유 데이터에 모니터를 지정해놓으면, 해당 모니터 내부에 들어간 최대 하나의 프로세스에게만 접근 기능을 제공한다는 점에서 상호 배제가 성립한다. 또한, 프로세스가 특정 조건을 부합하지 못할 경우 '조건 변수'를 호출할 수 있는데, 이 경우 해당 조건의 ready queue에 프로세스를 보내 대기시키고 새로운 프로세스를 모니터 내에서 수행할 수 있도록 한다. 새 프로세스의 실행이 종료되면 특정 조건 ready queue에 signal을 보내 대기 중이던 프로세스의 수행을 다시 시작시킬 수 있다. 반드시 짜여진 프로시져들을 거쳐야만 하는 접근 방법만이 존재하므로, 세마포어와는 달리 구조적이지 않은 접근으로부터 데이터를 보호할 수 있다는 특징을 갖는다.
