---
layout: single
title:  "가상 메모리(2/2) Virtual Memory(2/2)"
categories: Os
tags: [Os, ComputerScience]
toc: true

---

<br><br>

# 프레임의 할당

각 프로세스는 ‘최소 페이지 수’가 필요하다.

 • ISA에 의해 정의됨

 • 하나의 명령어가 (그게 어떤 명령어든간에) 참조할 가능성이 있는 모든 다른 페이지들을 hold 가능한 충분한 수의 프레임이 제공되어야 한다.<br>

local 교체

 • 프로세스가 자신이 할당받은 프레임 안에서만 교체 프레임을 선택한다. 다른 애 꺼 X

 • 프로세스에의 명시적 프레임 할당이 필요하다 : “Fixed allocation”<br>

global 교체

 • 프로세스가 모든 프레임 중에 교체 프레임을 선택한다. 다른 애 꺼 쌉가능.

 • 굳이 명시적인 프레임 할당 필요 無 : “Priority allocation”

​    <br><br><br>

​    

## Fixed Allocation

동일 할당

 • 프레임이 총 100개고 프로세스가 총 5개면, 20개씩 페이지 준다.

비율 할당

 • 프로세스의 size 혹은 우선순위에 따라 할당한다.

​    <br><br><br>

​    

## Priority Allocation

높은 우선순위의 프로세스가 낮은 애 꺼 프레임을 선택해서 교체하도록 허용

프로세스에서 page fault가 발생하면 그 내부 or 순위 더 낮은 애 프레임을 선택, 교체

​    

​    

# 스래싱(thrasing)

사용가능 프레임의 부족 : 프로세스가 물리적 메모리에 충분한 페이지 X → page fault ↑

낮은 CPU 활용률, OS의 멀티프로그래밍 수준 상향 판단 & 시스템에 다른 프로세스 추가 → “상황 악화”

스래싱 : 각 프로세스가 페이지를 안팎으로 움직이느라 바쁘다.

프로세스가 locality를 옮겨다니고, 이 locality들이 중복 가능하기에 paging 작동

사용 중인 페이지의 size의 합이 전체 메모리 size를 넘기면 스래싱 발생<br><br><br><br>

# Working-Set 모델

사용 중인 페이지들 모음을 Working Set, 페이지 참조의 고정된 수를 w-s window라 한다

 • window가 너무 작다 = 전체 locality를 포함하지 않는다

 • window가 너무 크다 = 여러 locality를 포함한다

 • window가 무한에 가깝다 = 전체 program을 포함한다

전체 필요 프레임의 수가 물리적 메모리 內 프레임 수보다 커지면 쓰레쉬 발생<br>

전략

 • OS는 각 프로세스의 working set을 모니터링하고, 각각의 working set의 size에 맞는 충분한 프레임을 할당한다.

 • 필요 프레임 양이 실제 프레임 양보다 많아질 겨우, 중지하고 프로세스 중 하나를 교체함.

 • 그러므로 이 전략은 멀티프로그래밍 수준을 가능한 높게 유지하면서 쓰레쉬를 방지함.

  <br><br><br>  

​    

## Working Set의 추적

“인터벌 타이머 + 참조 비트”를 사용한 근사치

예시 : window가 10,000이라 가정

 • 인터벌 타이머는 매 5천 time unit마다 인터럽트를 발생시킨다. 

 • 각 페이지마다 별도의 in-memory를 유지 (history bits, 2비트)

 • 인터럽트 발생 시마다, 두 history bit 中 하나를 copy 후 각 페이지의 참조 비트 값 지움

5천의 interval동안 그 안에 언제 참조가 발생했는지 알 수 없기에, 완전히 정확하지 않다.

개선된 방법 : 인터벌은 1천으로, im-memory를 10비트로.

​    <br><br><br><br>

​    

# Page Fault 빈도 할당

스래싱을 막기 위해 직접적으로 page fault를 측정하고, control함

감당 가능한 page fault 빈도를 설정한다.

 • 빈도가 너무 높으면, 프레임 수를 늘린다.

 • 빈도가 너무 낮으면, 프레임 수를 줄인다.

​    <br><br><br><br>

​    

# 다른 요소들

Prepaging

Page size

TLB reach

Program structure

I/O interlock<br><br><br>

## Prepaging

한 번에 여러 페이지를 load한다.

 • working set 모델을 쓰는 system 內에서는, 각 프로세스들의 working set의 페이지들의 list를 유지하고 있다. 어떤 프로세스를 중지하게 되면, 이 프로세스의 working set을 기억해두게 된다. 그리고 이 프로세스가 재개될 때, 프로세스가 다시 시작하기 전에 자동적으로 이 놈의 전체 working set을 메모리 안으로 가져온다.

s : prepaging된 페이지의 수

a : 실제 사용되는 페이지들 부분

  → a * s 만큼의 page fault를 save하지만, (1 – a) * s 만큼의 페이지 transfer 비용을 lose

​    <br><br><br>

​    

## Page Size

작은 page size

 • 장점 : 내부 단편화 ↓, 실제로 필요한 메모리만 isolate함으로써 메모리 활용도 ↑

 • 단점 : page table이 큼, page fault의 높은 빈도로 인한 오버헤드

큰 page size

 • 장점 : page table이 작음, 오버헤드도 적음

 • 단점 : 내부 단편화 ↑, 메모리 활용도 ↓

   <br><br><br> 

## TLB Reach

TLB에서 접근 가능한 메모리의 양을 일컬음 (TLB의 size   ![img](file:///C:\Users\Shako\AppData\Local\Temp\DRW000042ac68f8.gif)   Page의 size)

 • 각 프로세스의 working set이 TLB에 저장되는 것이 이상적이다. 안 그러면 page fault↑

page size 증가 : 모든 프로그램이 큰 page size를 요하지 않으므로, 내부 단편화 ↑

다양한 page size 제공

 • 더 큰 page size를 요하는 프로그램에게 단편화 증가 없이 사용할 기회를 준다.

​    <br><br><br>

​    

## Program Structure

프로그램의 구조 이해

 • 사용자나 컴파일러가 기본 demand paging에 대한 인식이 있으면 성능 향상 가능

​    <br><br><br>

​    

## I/O Interlock

문제점

 • global 페이지 교체라고 가정

 • 입출력 operation에 block된 프로세슨 이상적인 교체 후보로 보임

 • 그러나 교체하면, 입출력 operation이 system을 손상시킬 수 있다.<br>

해결책

 • 사용자 메모리에서 입출력을 절대 실행하지 않는다.

  → 항상 시스템 메모리와 사용자 메모리 사이에서만 데이터가 복사된다.

​     입출력은 시스템 메모리와 입출력 기기 사이에서만 발생한다.

 • ‘lock bits’를 이용해 물리적 메모리의 page를 잠근다.<br><br><br><br>

# 가상 메모리의 이점

프로세스를 생성하는 동안

 • Copy-on-Write

 • Memory-Mapped Files

​    <br><br><br>

​    

## Copy-on-Write

원칙

 • 부모&자식 프로세스가 처음에 메모리 內의 동일한 페이지들을 공유하도록 해준다.

   둘 중 하나가 공유 페이지를 수정하는 경우에만 페이지가 복사된다.

  → 공유 페이지들은 자식 프로세스에서 읽기 전용으로 보호됨

  → 읽기는 평소처럼 일어남

  → 쓰기는 protection fault를 발생시키고, OS를 trap한다. 이에 OS는 페이지를 복사하고, client page table의 page mapping을 변경하며, 쓰기 명령을 재실행한다.

 • 수정된 페이지만 복사되기 때문에, 프로세스 생성이 더욱 효율적이다.

 • 여러 OS에 의해 사용되는 ‘프로세스를 복사할 때 사용되는 기술’이다.

​    <br><br><br>

## Memory-Mapped Files

원칙

 • 메모리 內 페이지에 disk block을 mapping해서, 파일 입출력이 일반적인 메모리 접근처럼 여겨지도록 한다. (파일과 메모리에 대한 동일한 접근, 따라서 복사 감소)

 • demand paging을 이용해 파일이 처음 읽힌다. 파일의 page size 부분은 file system이 물리적 페이지로 읽는다. (이후의 파일 읽기/쓰기는 일반적인 메모리 접근으로 여겨진다.)

 • mmap() : 파일을 가상 메모리 영역으로 bind한다.

​             page table 엔트리가 가상 주소를 파일 데이터 가진 물리적 프레임에 mapping

 • 파일 입출력을 read/write 시스템 콜 사용으로 인한 오버헤드가 아니라 메모리를 통해 처리하므로, 파일 접근을 단순화한다.

 • 메모리 內 페이지의 공유를 허용함으로써, 여러 프로세스들이 같은 파일을 mapping 가능

 • 파일은 기본적으로 가상 주소 공간의 그 영역에 대한 backing store이다.

<br><br><br><br>    

​    

# 리눅스에서의 page fault 처리

가상 주소는 합법적인가? (vm_area_struct로 정의된 영역 내에 있는가?)

 • 아니라면 segmentation violation을 신호한다.

작업은 합법적인가? (프로세스가 이 영역을 읽기/쓰기 하는가?)

 • 아니라면 protection violation을 신호한다.

이상 없을 경우 fault를 처리한다.<br><br><br>

## 리눅스에서의 페이지 교체

2개의 LRU 리스트

 ① inactive_list : 아주 중요하다고는 알려지지 않은 페이지들

 ② active_list : 자주 참조된다고 알려진 페이지들 (= working set을 정의하는 페이지들)<br>

커널 swap daemon (커널 스레드)

 • 주기적으로 wake up한다. 메모리가 부족하면 더 자주 wake up한다.

 • 사용 안 된 페이지를 찾았거나, 페이지를 inactive list로 옮기거나, 디스크에 dirty한 페이지를 write하거나, 필요한 경우 페이지를 교체 방출할 때 메모리를 체크한다.<br>

페이지 교체 알고리즘도 존재하지만 있다고만 알아두자.
