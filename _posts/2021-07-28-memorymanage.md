---
layout: single
title:  "메모리 관리 Memory Management"
categories: Os
tags: [Os, ComputerScience]
toc: true

---

<br><br>

# 메모리 관리 

## 멀티프로그래밍

 • 메모리 內 한 번에 다수의 프로세스 필요 : 여러 작업의 입출력 & CPU 겹침

 • 요구사항

   ① 보호 : 프로세스가 사용할 수 있는 주소를 제한

   ② (보호와 translation을 위한) 메모리 하드웨어의 updating이 빨라야 함

​    <br>

## 메모리 관리

 • 메인 메모리에 다수의 프로세스들을 수용하기 위한 “OS & 하드웨어”의 작업

​    <br>

## 목표

 • 프로세스 간 고립(isolation) 제공

 • 경쟁 프로세스 간의 부족한 메모리 리소스를 할당 → 최소한의 오버헤드로 성능 극대화

​    <br>

## 쟁점

 • 다중 프로세스에 대한 지원

  : 각 프로세스는 논리적으로 연속적 공간에 있어야 하고, 각 공간의 크기는 가변적

 • 프로세스가 실제로 할당받은 메모리보다 많은 양을 사용할 수 있도록 한다!

  : 모든 메모리 공간이 동시에 사용되지 않고, 메모리 참조는 시간/공간적 인접성을 가짐

 • 프로세스 당 다수의 영역을 지원

 • 보호 & 공유

 • 성능

​    

​    <br><br><br>

# 주소 공간

## 물리적 주소 공간

 • 하드웨어에 의해 지원됨

 • “절대 주소”라고도 불림

 • 0번지에서 시작해 MAXsys 번지까지!

​    <br><br>

## 논리적 주소 공간

 • 프로세스가 자기 메모리를 보는 시점

 • 0번지에서 시작해 MAXprog 번지까지!

 • “상대 주소”는 프로그램 內 알려져있는 point에 대해 상대적인 위치로 표현되는 주소로,

   논리적 주소 中 하나이다.

​    <br>

**※ 사용자 프로그램은 논리적 주소를 다룬다!**

  : 실제 물리적 주소를 보는 일은 **절!대!** 없다.

​    CPU에 의해 실행되는 명령들은 논리적 주소와 관련 있다.

​    <br>

 → 명령어 수행 시 Linking 과정에서 논리적 주소를, Loading에서 물리적 주소를 다룬다.<br><br><br>

## 주소 변환을 위한 하드웨어

요구사항

 • 하드웨어가 상대 주소에서 실제 주소로 변환해야 함! why? 적절한 성능을 위해.

 • 실행 시간 때에 프로그램이 동적으로 “이전”됨.

 • Base 레지스터 + Limit 레지스터 이용해서 변환한다.

​    

​    

<br><br>

# 바꿔치기(swapping)

프로세스가 잠시 메모리 밖으로 나와서 backing store에 있다가, 다시 메모리 안으로 들어와서 마저 실행하는 현상

Backing store : 모든 사용자를 위한 모든 메모리 이미지의 복사본을 수용할 수 있을만큼 크고, 빠른 Disk. 이 메모리 이미지들에 대한 직접 접근을 제공할 수 있어야 한다.

효율성 : 바꿔치기 time의 큰 part는 “전송 시간”이다. 전송 시간 자체가 바꿔치기 되는 메모리의 양에 정비례한다.

​    <br><br><br><br>

​    

# “단순한” 메모리 관리

“가상 메모리 없는” 간단한 관리 : 실행 중 프로세스가 “메인 메모리에서” 전부 로드됨.<br>

방법

  ① Fixed partitioning

  ② Dynamic partitioning

  ③ Simple paging

  ④ Simple segmentation

​    <br>

※ 오늘날의 최신 첨단 OS에서는 사용되지 않는 기술임.

   but, 가상 메모리에 대해 제대로 논의할 수 있는 토대를 마련해준다.

<br><br><br>

## Fixed Partitioning

메커니즘

 • 메인 메모리를 “파티션”이라 불리는 겹침 없는 영역으로 나눈다.

 • 파티션들은 크기가 같을수도, 아닐수도 있습니다 like 안철수.

​    <br>

원칙

 • 크기가 파티션보다 작거나 같은 어떤 프로세스든 파티션 內에 load 가능

 • 모든 파티션이 사용 중이면? OS가 파티션 內 프로세스를 swap할 수 있음.

 • 프로그램이 너무 커서 파티션에 못 들어가면? 프로그램을 overlays로 설계할 수 있다.

  : 필요한 모듈이 없을 경우, 사용자 프로그램이 프로그램의 파티션 內에 해당 모듈을 load할 수 있다. 프로그램 안에 뭐가 있건말건 상관없이 걍 overlaying한다~ 이 말이 되겠다.

​    <br>

장단점

 • 메인 메모리 사용 자체가 비효율적이에요! 왜? 프로그램이 아무리 작아도 전체 파티션을 혼자 다 쓰거든. 이걸 Internal Fragmentation이라고 함.

 • 서로 다른 크기의 파티션들이 이걸 완화해주기는 하나, 없어지는 건 아님.

​    <br><br>

### 동일 크기 파티션들에서의 배치 알고리즘

 • 사용가능 파티션이 있으면 프로세스는 거기에 load될 수 있다. 어차피 파티션들 크기 다 똑같으니까 어떤 파티션을 쓰건 노상관.

 • 모든 파티션이 block된 프로세스들로만 가득 차 있으면 한 놈 골라서 쫓아내고 새로운 프로세스를 들일 공간을 확보한다. (swap)

​    <br><br>

### 크기 다른 파티션들에서의 배치 알고리즘 – 여러 개의 queue 사용하기

 • 각 프로세스를 들어갈 수 있는 가장 작은 파티션에 넣기

 • queue는 뭐에 대한 queue냐? 각각 크기의 파티션에 대한 queue이다.

 • Internal Fragmentation을 최소화하고자 함. (이게 뭐라고? 작은 쉑이 혼자 다 쓰는 것.)

 • 사이즈에 맞는 프로세스가 없을 경우 몇몇 queue는 empty로 남는다는 문제점 存在

​    <br><br>

### 크기 다른 파티션들에서의 배치 알고리즘 – 단 한 개의 queue만 사용하기

 • 프로세스를 메인 메모리 內로 load해야 할 때, 가장 작은 사용 가능 파티션이 선택됨

 • Internal Fragmentation을 감내하면서까지 multi-programming 레벨을 높임.

<br><br><br>

## Dynamic Partitioning 

원칙

 • 다양한 길이와 수의 파티션 存在

 • 각 프로세스는 요구하는 만큼의 메모리를 정확하게 할당받는다.

 • 최종적으로는 메인 메모리에 구멍들이 형성된다. 이걸 External Fragmentation이라 한다.

 • 프로세스들을 연속적으로 배치하고, 모든 free한 메모리가 한 block 안에 있도록 하기 위해서 프로세스들을 이동시켜야 하고, 이걸 위해서 “Compaction”을 사용한다.

​    <br>

배치 알고리즘

 • 어떤 free한 block에 프로세스를 할당할지 정할 때 쓰임

 • compaction의 사용을 줄여 시간을 절약하기 위해 씀

 ① Best-fit : 가장 작은 구멍 선택! (그렇게 남는 조각은 최대한 작다)

 ② First-fit : 첫 번째 구멍 선택!

 ③ Next-fit : 지난 배치에서 가장 처음 있는 구멍 선택!

 ④ Worst-fit : 가장 큰 구멍 선택~! 왜 함?

​    

​    

### Fragmentation

앞에서 다했죠? External은 구멍 송송, Internal은 안 쓰일 공간까지 독차지 쌉민폐 ㅇㅇ

​    <br><br>

​    

### Compaction

External Fragmentation 해결할 방법이라 했죠? 프로그램 옮겨서 구멍 하나로 합침.

**실행 시간**에 수행된다. 동적 파티셔닝에서만 쓴다!!!<br>

※ 입출력 문제 : job이 입출력과 관련있는 동안은 메모리 內에서 job을 못 움직이게 함.<br><br><br>

## 버디 시스템 

원칙

 • fixed & dynamic partitioning의 단점을 모두 극복하기 위한 합리적인 타협

 • 2^L = 할당 가능 block 中 가장 작은 size

   2^U = 할당 가능 block 中 가장 큰 size

   메모리 block은 2^K의 size 갖기가 가능하다. ( L <= K <= U )

​    <br>

※ 버디 시스템은 효율적임. 근데, paging이랑 segmentation 있는 가상 메모리가 걍 압살함.

   (단, 이거의 수정 버전이 커널 메모리 할당을 위해 UNIX SVR4나 리눅스에서 아직 쓰이기는 함)

​    <br>

알고리즘

 • 전체 block의 size를 2^U로 시작한다.

 • 걍 엄청 간단함. 전체보다 작고 절반보다 크면 그대로 할당함.

 • 근데 절반보다 작으면? 전체를 둘로 쪼갬. then, 쪼개진 둘 中 하나를 전체처럼 잡고 반복.

   → 요청된 size보다 크거나 같은 가장 작은 size의 block이 나오면! 그 때 멈춤.

​    <br>

※ 두 버디는 할당되지 않으면 다시 붙는다.

   block의 주소 & 크기 정해지면, 버디의 주소도 자동으로 결정된다.

   두 버디 간 주소는 정확히 한 비트 위치에서 다름.<br><br><br>

## 페이징 

원칙

 • 프로세스의 물리적 주소 공간은 no연속일 수 있음.

 • 물리적 메모리를 “프레임”이라는 고정된 크기의 block들로 나눈다.

   (size는 보통 512bytes ~ 8192bytes 사이. 무조건 2의 제곱수임.)

 • 논리적 메모리를 “페이지”라는 동일한 크기의 block들로 나눈다.

 • 사용 가능한 모든 프레임들을 계속 추적한다.

 • n개의 페이지 크기의 프로그램을 실행하려면, n개의 사용 가능 프레임을 찾아 load한다.

 • 논리적 주소를 물리적 주소로 translate하기 위해, “page table”을 설정한다.

 • Internal Fragmentation의 가능성 有

​    <br>

※ 논리적 주소(= 가상 주소)는 (page number, page offset)의 쌍으로 이루어져 있다.

   물리적 주소는 (frame number, frame offset)의 쌍으로 이루어져 있다.

​    <br>

※ 모든 페이지가 프레임에 mapping될 필요는 없다고 한다.

​    <br>

※ page table이 가상의 페이지를 물리적 프레임에 mapping시킨다.

​    <br><br>

​    

### Page Table의 구조

각 프로세스 당 table 하나

핵심 요소 : page frame 번호, 비트 플래그(present/valid, dirty, reference, protection)

​    <br><br>

​    

###  페이지 공유

다른 프로세스에 있지만 같은 프레임에 mapping된 페이지들에 의해 수행된다.<br><br>

### Page Table 실행

메인 메모리에 보관된다.

  ① Page-Table Base Register : page table을 가리킨다.

  ② Page-Table Length Register : page table의 size를 가리킨다.

​    <br>

메모리 접근 오버헤드

 : 모든 데이터/명령어 접근은 두 개의 메모리 접근을 필요로 한다.

   ① page table 접근

   ② 데이터/명령어 접근

   이 문제는 associative한 레지스터들로 구성된 특별한 초고속 메모리인

   “**Translation Look-aside Buffer(TLB)**”들에 의해 해결된다. 얘도 associative한 메모리다.

​    <br><br>

​    

### 연상 메모리(associative memory) 

기억된 내용의 일부를 이용하여 원하는 정보가 기억된 위치를 찾아내서 접근하는 메모리

'병렬 검색'이 가능하다.

 • **Content-Addressable Memory(CAM)**이라고도 한다.

 • 소프트웨어 루프보다는 단일 메모리 cycle에서 값을 검색할 수 있다.

 • associative한 레지스터들로 구성되어있다.<br>

주소 translation의 핵심 아이디어

 • A’이 associative 레지스터면, 프레임 번호를 꺼낸다.

 • 그렇지 않다면 메모리의 page table에서 프레임 번호를 가져온다.

​    <br><br>

​    

### 유효 접근 시간

“원하는 자료를 읽을 때까지 걸리는 시간” (cache의 적중률 고려)

  → CPU가 논리적 주소를 내면, page table을 거쳐 물리적 주소로 바꾼 뒤 메모리로 가는 속도

associative 조회 =   ![img](file:///C:\Users\Shako\AppData\Local\Temp\DRW000009484e45.gif)  시간 유닛

메모리 cycle 시간이 1마이크로초라고 가정

적중률(  ![img](file:///C:\Users\Shako\AppData\Local\Temp\DRW000009484e47.gif)  ) : TLB에서 페이지 번호를 찾은 횟수의 퍼센티지

유효 접근 시간 : (1 +   ![img](file:///C:\Users\Shako\AppData\Local\Temp\DRW000009484e49.gif)  )  ![img](file:///C:\Users\Shako\AppData\Local\Temp\DRW000009484e4b.gif)   + (2 +   ![img](file:///C:\Users\Shako\AppData\Local\Temp\DRW000009484e4d.gif)  )(1 -   ![img](file:///C:\Users\Shako\AppData\Local\Temp\DRW000009484e4f.gif)  )  =  2 +   ![img](file:///C:\Users\Shako\AppData\Local\Temp\DRW000009484e51.gif)   -   ![img](file:///C:\Users\Shako\AppData\Local\Temp\DRW000009484e53.gif)  

<br><br>

### 거대한 page table 처리

예시

 • 32비트의 논리적 주소 / 4KB의 페이지 size / 4byte의 page table 엔트리

   → page table의 size = 232 / 212 * 4 = 프로세스 당 4MB

 • 프로세스가 실행 중일 땐 그 page table은 전체가 물리적 주소에 있어야 한다.<br>

주요 쟁점

 • 실제로 사용 중인 주소 공간의 일부(전체 주소 공간의 극히 일부)만 mapping하면 된다!<br>

접근법

 • Multi-level paging / Hashed page table / Inverted page table

⑴ 계층적(Multi-level) paging : 논리적 주소 공간을 다수의 page table들로 쪼갠다.

 • 간단한 건 two-level인 page table.

⑵ Hashed page table : 가상 page 번호가 page table로 hash된다.

⑶ Inverted page table : 프레임(메모리의 각 실제 페이지) 당 하나의 엔트리.

<br><br>

#### 1. Multi-level paging  

단순한 형태인 two-level paging에 대한 그림을 삽입한다.

![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/twolevel.png){: .align-center}{: width="20%" height="20%"}

<br>

​    <br>

#### 2. Hashed Page Table

원칙

 • 32bit보다 큰 주소 공간에서 흔함 (64bit 아키텍처 etc.)

 • 가상 page 번호는 page table로 hash된다.

   이 page table에는 동일 위치로 hash하는 일련의 요소들을 담고 있다.

 • 가상 page 번호들을 이 일련의 요소들에서 비교하여 일치하는 내용을 찾는다.

   만약 일치하는 게 발견되면, 해당 실제 frame이 추출된다.

<br><br>

#### 3. Inverted Page Table

원칙

 • frame(memory의 real page) 하나마다 엔트리 하나씩

 • 실제 메모리 위치에 저장된 page의 가상 주소 & 그 page를 가진 프로세스의 정보로 구성

 • 각 page table을 저장하는 데에 필요한 메모리를 줄여준다.

   but, page reference가 발생하면, table을 찾는 데에 필요한 시간이 늘어난다.

 • hash table을 사용해서, 검색량을 1개, 또는 최대 몇 개만의 page table 엔트리로 제한함.

 • 이 녀석만으로 page fault를 handling하기에 충분한가?

​    <br><br><br>

##  Segmentation

배경

 • 사용자 관점의 메모리를 지원하는 메모리 관리 체계

 • 프로그램 = segment의 집합

 • code, data, stack 같은 논리적 unit을 다 segment라 한다.

 • 본래는 paging과 관련이 없으나, 대부분의 시스템에서는 paging과 함께 사용됨.

​    <br><br><br>

​    

### Segmentation 아키텍쳐

논리적 주소는 2개의 tuple, segment 번호 & offset으로 구성된다.<br>

Segment table

 • 2차원의 물리적 주소를 mapping한다.

 • 각 table 엔트리는 base와 limit을 갖고 있다.

  ① base : segment들이 메모리에 상주하는 곳인, 시작 물리적 주소를 포함함.

  ② limit : segment들의 길이를 지정함.<br>

Segment-Table Base Register (STBR)

 • 메모리 內 segment table의 위치를 가리킨다.

Segment-Table Length Register (STLR)

 • 프로그램이 사용하는 segment들의 수를 가리킨다.

 • 얘보다 segment 번호가 작으면 유효하다.<br>

재배치(혹은 이전)

 • 동적이며, segment table에 의해 이루어진다.<br>

공유

 • 코드 공유는 segment 레벨에서 이뤄진다. 보호 비트는 segment와 연관되어 있다.<br>

할당

 • segment는 길이가 다양하므로, 메모리 할당은 동적 storage 할당 문제가 된다.

 • First-fit, Best-fit 등 사용

 • 외부 단편화 문제 있음<br><br><br><br>

## Segmentation  VS  Paging

비교

 • Segmentation은 external fragmentation 문제를 겪는다.

 • Paging은 작은 internal fragmentation을 만든다.

 • Segmentation은 프로그램을 논리적으로 segment로 구성하고, 다양한 종류의 protection을 사용하기 위해 programmer에게 제공되는 상품으로 볼 수 있다.

   예시) 코드는 실행 전용이고, 데이터는 읽기-쓰기 전용이다.

​         이러한 경우, segment table의 엔트리에서 보호 비트를 써야 한다.

<br><br><br>    

​    

## paging을 사용한 segmentation

원칙

 • segmentation과 paging을 합친다.

 • 논리적으로 연관된 unit들을 관리하는 데에 segment를 쓴다.

  : 모듈, 프로시져, 스택, 파일, 데이터 etc.

​    segment는 size가 다양하지만 대부분 크다. (page의 몇 배)

 • page를 써서 segment를 고정 크기의 덩어리들로 분할한다.

  : segment가 물리적 메모리를 관리하기 용이해진다.

​    segment를 메모리 안팎으로 이동시키기보단 segment의 page 부분을 이동시킬 수 있다.

​    page table의 엔트리는 자신이 할당된 segment에 대해서만 할당해야 한다.<br>

개별 segment

 • paging된 가상 주소 공간으로 구현된다.

 • 논리적 주소는 3개로 구성된다. segment 번호, page 번호, page offset.<br>

추가적인 이득

 • 보호 : page당 기준이 아닌 segment당 기준으로 지정할 수 있다.

 • 공유

 <br><br>   

​    

### paging을 이용한 segmentation : Intel x86

인텔 x86 프로세서의 주소 3종류

 ① 논리적 주소 (가상 주소)

  • 피연산자, 명령어의 주소를 지정하는 기계어 instruction

 ② 선형 주소

  • 최대 4GB의 주소로 사용될 수 있는 32비트 정수

 ③ 물리적 주소

  • 메모리 칩에 포함된 메모리 cell을 주소로 사용?<br><br>

### 하드웨어에서의 segmentation (인텔 x86)

segmentation은 메모리의 논리적 파티션

 • 논리적 주소 : < segment 식별자, offset >

 • segment 식별자 : 16비트. segment selector라고 불린다.

 • offset : 32비트. segment 內 상대 주소.<br>

segment selector를 빠르게 검색하기 위해, 프로세서가 **segmentation** **레지스터**를 제공함.

 • cs(코드 segment 레지스터) : 프로그램 명령어를 포함한 segment를 가리킴

 • ds(데이터 segment 레지스터) : 정적 및 외부 데이터를 포함한 segment를 가리킴

 • ss(스택 segment 레지스터) : 현재 프로그램 스택을 포함한 segment를 가리킴

 • es, fs, gs : 일반적인 목적 or 임의의 segment를 참조

 <br><br>   

​    

### 하드웨어에서의 paging (인텔 x86)

paging unit은 선형 주소를 물리적 주소로 translate한다.

 • page의 접근 권한을 check한다.

 • 실패할 경우 page fault 예외를 발생시킨다.<br>

page

 • 선형 주소는 고정된 길이의 interval로 그룹화된다.

 • page의 연속적인 선형 주소는 연속적인 물리적 주소로 mapping된다.

 • page의 물리적 메모리(frame)는 page를 포함한다.<br>

CR0 레지스터 안의 PG 비트를 설정하여 활성화한다.<br>

2단계 paging 하드웨어

 • page 디렉토리 : page table들의 물리적 주소들 (CR3 레지스터)

 • page table : page들의 물리적 주소들

​    

  <br><br>  

### Regular paging (인텔 x86)

page table : 물리적 주소에 선형으로 mapping되는 자료구조

page의 size는 4KB

선형 주소는 32비트

2단계 paging<br><br>

### Page 디렉토리 & page table

table 구조 : 엔트리는 4byte, 총 1024개 엔트리 = 4KB. 하나의 page frame에 load됨.

page table 엔트리의 구조

 • Present : 메모리 유무

 • page frame의 실제 주소의 20 MSB를 포함한 field. (12개의 LSB는 전부 0이다.)

 • Accessed : page unit이 page frame에 주소 지정할 때마다 설정 (교체시킬 page 선택)

 • Dirty : page frame에 쓰기 작업 수행 시 설정

 • Read/Write : page의 접근 권한

 • User/Superviser : 접근에 필요한 권한 수준

 • PCD & PWT : 하드웨어 cache용

 • Page Size : page 디렉토리 엔트리에만 해당됨

​    <br><br>

​    

### 확장된 paging (인텔 x86)

page frame이 4KB or 4MB가 되도록 허용

32비트의 선형 주소

 • 디렉토리 10비트, 오프셋 22비트(= 4MB).

 • page size 비트가 설정되어야 한다.

​    <br><br>

​    

### 리눅스에서의 paging (인텔 x86)

4단계 paging : 각 프로세스는 자체 page global 디렉토리와 page table 집합을 갖는다.

​    

<br><br><br><br><br><br>

​    

​    

# 전체 요약

프로세스에서 논리적 주소가 사용된다

 : OS와 하드웨어가 논리적 주소를 물리적 주소로 translate한다.<br>

다양한 기법들

 • Fixed Partitioning : 사용하기 쉽다, but 내부 단편화

 • Dynamic Partitioning : 더 효율적이다, but 외부 단편화

 • Paging : 작은 고정 크기의 덩어리들을 쓰며, OS에 효율적이다.

 • Segmentation : 사용자 관점에서 덩어리들로 관리한다.

 • Paging과 Segmentation의 이점을 모두 살리기 위해 둘을 결합한다.
