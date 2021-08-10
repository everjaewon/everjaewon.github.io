---
layout: single
title:  "대용량 기억 Mass Storage"
categories: Os
tags: [Os, ComputerScience]
toc: true

---

<br><br>

# 왜 디스크를 쓰는가?

① 메모리 불충분 : 가상 메모리 시스템에서의 swap space로서의 역할

② 휘발성 메모리 : 장기 기억장치로서의 역할

​    <br><br><br><br>

​    

# 디스크 해부

![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/disk.png){: width="70%" height="70%"}    

​    <br><br><br><br>

# 디스크의 읽기/쓰기 작업

① 디스크를 sector address와 함께 준비

② head가 target 트랙으로 이동

③ 적절한 head 활성화

④ head 밑에 target sector가 나타낼 때까지 대기

⑤ sector 읽기/쓰기

​    <br>

※ 읽기/쓰기 시간 = 탐색 시간 + 지연 시간 + 전송 시간

   <br><br><br><br> 

​    

# 디스크의 주소 지정

3D 구조를 1D 구조에 mapping

<br>

Cylinder-based Mapping

 • sector 주소에서 logical block 주소로 mapping

<br><br><br><br>

# 디스크(head) 스케줄링

디스크 queue에 2개 이상의 요청이 있을 가능성 有

 • 요청에 대한 head의 이동 최소화 → 디스크의 입출력 성능 극대화

<br>

디스크 스케줄링 알고리즘

 • FCFS : 순서대로 / fair하나 비효율적

 • SSTF : 현 head 위치에서 최소 탐색 시간으로 다음 선택 / starvation 가능

 • SCAN : head가 한 방향으로 끝까지 이동 後 반대 방향으로 이동 / 엘리베이터 알고리즘

 • C-SCAN : head 방향 오직 하나로만 외길 / SCAN보다 균일한 대기 시간

 • Look : SCAN 개선판, 끝까지 이동이 아니라 끝 요청까지만 이동

 • C-LOOK : C-SCAN 개선판, 끝까지 이동이 아니라 끝 요청까지만 이동

<br>

디스크 스케줄링 알고리즘의 선택

 • 요청의 종류와 수에 따라 성능이 달라진다.

 • 요청은 파일 할당 방식에 영향을 받을 수 있다.

 • 필요한 경우 다른 알고리즘으로 교체할 수 있도록, OS의 별도 모듈로 작성해야 한다.

 • SSTF : 일반적, 자연적인 어필 가능

 • SCAN/C-SCAN은 디스크에 부하가 많이 걸리는 system에서 더 낫다.

 • SSTF나 LOOK이 기본 알고리즘에 적합한 선택이다.

   <br><br><br><br> 

​    

# 디스크 관리

저수준 포맷/물리적 포맷

 • disk controller가 읽고 쓸 수 있는 sector들로 disk를 분할

디스크에 파일을 보관하려면, OS가 고유 자료구조를 디스크에 기록해야 함

 • 디스크를 하나 이상의 cylinder group으로 분할

 • 논리적 포맷 or 파일 시스템 만들기

부팅 block은 bootstrap을 저장한다. (컴퓨터에 프로그램을 처음으로 입력하는 방법)

<br>

디스크 분할하기

 • 디스크 1개가 여러 파티션을 가질 수 있음

 • 파티션 = 연속된 실린더의 집합

 • 각 파티션은 논리적으로 분리된 디스크이다.

<br>

좋지 않은 section을 다시 mapping하기

 • 디스크 섹터가 실패하고, 하드웨어에 의해 자동으로 예비 위치로 다시 mapping된다.

 • 섹터 스페어링 : 각 실린더에 여분의 섹터를 설정함 (OS는 못 봄)

<br><br><br><br>

# Swap Space 관리

Swap Space

 • 가상 메모리 시스템에 최고의 처리량을 제공해야 함

 • 일반 file system에서 분할하거나, 별도의 디스크 파티션에 배치할 수 있음 (후자가 더 일반적)

<br>

Swap File 접근 (전자)

 • 일반 file system 루틴을 사용하여 공간을 생성/이름/할당합니다.

 • file system 자료구조를 이동하는 비용 때문에 비효율적입니다.

Swap Partition 접근 (후자)

 • 디스크 파티셔닝 중에 고정된 스왑 공간 양이 생성됩니다.

 • 별도의 스왑 공간 storage 관리자를 사용하여 블록을 할당 및 할당 해제합니다.

 • storage 관리자는 storage의 효율성보다는 속도에 최적화된 알고리즘을 사용합니다.

<br>

예시

 • 초기 UNIX는 연속적인 디스크 영역과 메모리 사이의 전체 프로세스들을 복사하는 데에 swapping을 사용했음

 • paging 하드웨어를 쓸 수 있게 되면서, UNIX는 swapping과 paging의 조합으로 발전함

  → 4.3BSD : 프로세스 시작 시 swap space 할당 : text와 data 세그먼트를 보관

  → Solaris 2 : 페이지가 물리적 메모리에서 강제 제거될 때에만 swap space 할당

​    <br><br><br><br>

​    

# 저렴한 디스크의 중복 array : “RAID”

원래 목표

 • 저렴&신뢰성 낮은 PC급 디스크 드라이브 구성품에서 고수준의 저장 신뢰성을 달성

<br>

핵심 메커니즘

 • 높은 처리량을 위해, 여러 디스크에 걸쳐 striping함

   ( = segment 단위로 데이터를 분할해 여러 디스크에 분산시킴)

 • 높은 시스템 가용성을 위해, 데이터 중복성 확보

 • 중복성은 어떻게 확보하느냐?

  ① 여러 드라이브에 동일한 데이터를 작성 (= 미러링)

  ② array 전체에 걸쳐 추가 데이터(= parity data) 작성

<br>

RAID의 설계 목표 2가지

 • 데이터 신뢰성↑  or  입출력 성능↑

<br>

\- RAID 단계

 • 데이터 손실, 용량, 속도의 균형에 다양한 변화를 줘서 만드는 여러 설계 접근 방식

 • RAID level이라고 하는 여러 표준 체계의 발전

 • 0, 1, 5가 가장 일반적으로 사용되고, 대부분의 요구사항을 충족함

  → 0은 스트라이핑, 1은 미러링, 5는 두 개 섞은 것?

<br>

RAID 0

 • 데이터를 여러 디스크에 분산, but 중복 없음. 향상된 속도 제공.

<br>

RAID 1

 • 100% 용량 오버헤드로 완전한 redundancy 제공

 • 읽기 작업은 최적화 가능, but 쓰기 작업은 2개의 physical writes 필요

<br>

RAID 3 : 전용 parity가 있는 striped set

 • parity block : 단일 디스크 block 실패에 대한 데이터 복구

 • 단일 디스크 저장 오버헤드 (예: parity 디스크)

 • parity 디스크에 bottleneck 발생 가능 & 작은 write 문제

 • 작은 write 문제가 뭔데? : 1 논리적 쓰기 = 2 물리적 읽기 + 2 물리적 쓰기

<br>

RAID 5 : 분산 parity가 있는 striped set

 • 분산 parity block들이 system의 모든 디스크에 분산되어 있음

 • 모든 드라이브 장애는 교체가 필요하지만, 단일 드라이브 장애로 인해 array가 파괴되진 않음

<br><br><br><br>    

​    

# 3차 저장

3차 저장 장치

 • 결정적인 특징 : “저렴함”

 • 이동식 미디어를 사용해 구축됨

 • DVD와 CD-ROM이 대표적인 예

<br>

계층적 저장 관리(HSM)

 • 저장 계층을 기본 메모리 & 보조 저장 이상으로 확장하여 3차 저장을 통합함

 • file system을 확장해서 통합함

  : 빈번히 사용되는 작은 파일들이 디스크에 남음

​    크고 오래된 비활성 파일들은 주크박스에 보관됨

 • 슈퍼컴퓨팅 센터나, 대용량 데이터를 가진 대규모 설치에서 찾을 수 있음

<br>

성능 문제

 • 비용 : 3차 저장의 낮은 비용은 저렴한 카트리지가 여러 비싼 드라이브를 공유해서 그럼. 그렇기 때문에 카트리지 수가 드라이브 수보다 훨씬 많을 때만 비용이 절감됨.

 • 속도 & 대역폭 : 이동식 라이브러리는 자주 안 쓰는 데이터 보관에 가장 적합함.

   unit time마다 비교적 적은 수의 입출력 요청만 충족할 수 있기 때문임.

 • 신뢰성 : 고정된 하드 디스크의 head 충돌은 보통 데이터가 파괴되는데, 광 디스크 드라이브의 실패는 데이터 카트리지를 손상시키지 않음.

  <br><br><br><br>  

​    

# storage 중심 인프라

언제 어디서나 : 어떤 사람, 프로그램, 시스템이든 접근 가능한 단일 대용량 데이터 저장소

디스크 속도 or 디스크 內 embedded 프로세서의 속도 빨라지면?

 → Network Attached Sotrage(**NAS**)  &  Storage Area Network(SAN)

<br><br><br>

## 산업 정의

**NAS** (Network Attached Storage)

 • TCP/IP 같은 통신 프로토콜을 사용하는 메시지 네트워크에 통합 storage 시스템이 연결된 기술

 • 클라이언트-서버 관계에서의 전문 서버 역할을 한다.

 • 저장소와 file system을 모두 제공

<br>

**SAN** (Storage Area Network)

 • 공유 접근 네트워크에 저장 장치를 연결하는 걸 도와주는 기술

 • storage 전용 사설 네트워크를 통해 접근

 • SCSI, Fibre 채널 등의 storage 프로토콜 사용

 • block 기반 storage만 제공, file system은 안 제공함

​    

​    

### NAS (Network Attached Storage)

네트워크에 연결해서 storage 기능 제공하는 모든 storage 장치

​    <br><br>

### SAN (Storage Area Network)

서로 연결된 컴퓨터 시스템과 storage 장치의 구성

허브, 스위치 같은 전통적인 네트워킹 개념을 이용해 서로 연결함

   <br><br><br> 

## NAS와 SAN의 장점

전통 architecture의 단점

 • 서버 기계를 이용한 복사 데이터의 저장 및 전달

  : translate해서 요청을 전달하고, 데이터를 저장 및 전달함

​    데이터 전송 中 bottleneck 현상 가능

 • 서버 기계에는 slot과 대역폭에 제한 有 : 확장성 無

<br>

NAS  /  SAN의 장점

 • 가용성

 • 확장성 : 용량, 연결성 & 대역폭

 • storage 장치의 모음

 • 부하 분산

 • 중앙 집중식 storage 관리
