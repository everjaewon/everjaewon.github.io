---
layout: single
title:  "파일 시스템(2/2) File System(2/2)"
categories: Os
tags: [Os, ComputerScience]
toc: true

---

<br><br>

# 할당 방법

① 연속 할당

② 연결 할당

③ 인덱스 할당

​    <br><br><br>

​    

## 연속 할당

각 파일이 disk의 연속적인 block의 set를 점유한다.

 • 단순한 방법 : 시작 지점과 길이가 필요하다.

 • 효율적인 랜덤 접근

 • 동적 할당 문제로 인한 공간 낭비

 • 문제점 : ① 파일 크기 늘릴 수 없다

​           ② 필요한 공간을 과측정한다.

​           ③ 더 큰 공간을 찾고, 복사한 뒤 방출한다.

 • CD-ROM에서 실행 가능하며, 실제로 널리 쓰임.

   → 모든 파일 크기를 미리 알고 있으며, 파일은 절대 변경되지 않음

​    <br><br><br>

​    

## 연결 할당

각 파일이 disk block의 linked list이다. disk block들은 disk 아무데나 흩뿌려져 있다.

 • 단순한 방법 : 시작 지점만 필요하다.

 • 공간 낭비 無

 • 문제점 : ① 비효율적인 랜덤 접근

​           ② 포인터를 위한 공간 필요

​           ③ 포인터 손실에 관련한 ‘안정성’ 이슈

​    <br><br><br>

​    

## 인덱스 할당

모든 포인터를 index block으로 가져온다.

 • index table이 필요함.

 • 외부 단편화 없는, 상당히 효율적인 랜덤 접근

 • 다단계 or 결합된 방식으로 사용 가능

<br><br>

### 유닉스의 file system (인덱스 할당)

inode

 • 모든 파일에 有

 • 고정된 크기 & 상대적으로 작다 → 메인 메모리에 장시간동안 보관 가능

 • 파일의 크기가 작을수록 거의/전혀 간접적이지 않게 접근 가능 : 처리/접근 시간 ↓

 • 파일의 이론적으로 최대 크기는 거의 모든 프로그램을 충족할 수 있을만큼 크다.

​    <br><br><br><br>

​    

# 사용가능 공간 관리

⑴ 비트 벡터 / 비트맵

 • 연속된 파일 얻는 게 쉽다.

 • 비트맵을 위해 추가 공간이 필요하다.

 • block number 계산 : (word당 비트 수) * (0값 word의 수) + 첫 1bit의 offset

<br>

⑵ Linked list

 • 모든 사용가능 disk block을 첫 번째 block부터 다 pointer로 link한다.

 • 연속된 공간 얻는 건 안 쉬움

 • 공간 낭비는 없다.

<br>

⑶ 그룹화

 • 사용가능 block n개의 주소를 첫 번째 사용가능 block에 넣음

 • 첫 n-1개의 block은 실제로 사용가능 상태다.

 • 마지막 block은 다른 n개의 사용가능 block의 주소를 담고 있음

<br>

⑷ 카운트하기

 • list를 유지하기보단, 첫 번째 사용가능 block의 주소와, 첫 번째 block을 뒤따르는 연속적인 사용가능 block의 수를 유지한다.

​    <br><br><br><br>

​    

# 가상 file system (VFS)

커널 소프트웨어 layer이다. 표준 유닉스 file system에 관한 모든 system call을 처리함.

목적

 • 사용자 모드 프로세스와 동일 인터페이스 제공

 • 모든 file system 구현을 위한 커널 추상화의 제공

<br>

기능

 • 파일 & 파일시스템 관련 system call 서비스

 • 파일 & 파일시스템 관련 data structure 관리

 • 효율적 조회 & 파일시스템 슥훑기를 위한 routine

 • 특정 파일시스템 모듈과 상호작용

<br><br><br>

## 리눅스의 VFS

리눅스의 VFS model은 4개 object로 구성됨

 ① superblock : mount된 file system에 관한 정보 저장

 ② inode : 특정 파일에 대한 일반적인 정보 저장

 ③ file : 열린 파일과 프로세스 간 상호작용에 대한 정보 저장

 ④ dentry : 디렉토리 엔트리와 이에 상응하는 파일의 link와 관련한 정보 저장

 <br><br><br><br>   

​    

# Disk Caching

## 버퍼 캐시

 • 프로그램은 파일을 읽고 쓰는 데에 뚜렷한 locality를 보임

 • 따라서, locality를 알기 위해 메모리 內 파일 block을 cache한다.

 • cache는 시스템 전반으로 모든 프로세스에서 쓰고 공유함

 • cache로부터 읽어들이는 건 disk가 memory처럼 동작하게 함

 • 4MB짜리 cache도 아주 효과적이다.

 • 장점 : disk의 트래픽을 줄이고, 불필요한 disk 입출력을 없애줌

 • 단점 : cache 일관성의 문제

<br>

문제

 • buffer cache가 가상메모리와 경쟁함

 • 크기도 한계가 존재

 • 교체 알고리즘이 필요 : 참조가 상대적으로 드물다 → 모든 block을 정확한 LRU 순서로 유지하는 게 가능

<br><br><br>

## 페이지 캐시

 • 가상 메모리 기술을 이용, disk block보다는 page를 cache한다.

 • Memory-mapped 입출력이 이용한다.

<br><br><br>

## 통합 버퍼 캐시

 • 동일한 페이지 캐시를 이용해서 memory-mapped 페이지와 일반 file system 입출력을 모두 cache한다.

 • 페이지 캐시와 버퍼 캐시 간의 불일치의 가능성 & 이중 caching을 방지한다.

<br><br><br><br>

# 복구

⑴ file system 복구

 • 시스템 충돌로 인해 cache block들이 기록되지 않으면, file system이 inconsistent한 상태로 남는다.

 • 이 block들 중 일부가 inode block이거나, directory block이거나, free list 갖고 있는 block이면 치명적이다.

 • 그래서 대부분의 시스템은 file system의 일관성을 검사하는 utility 프로그램을 갖고 있음.

<br>

⑵ 시스템 백업

 • 시스템 프로그램을 이용해 디스크의 data를 다른 저장장치로 백업한다.

 • 백업으로 data를 복원해서, 손실된 파일/disk를 복구한다.

​    <br><br><br><br>

​    

# “Linux Ext2” file system

역사

 • 리눅스의 첫 버전은 Minix 파일시스템 썼음.

 • 리눅스는 후에 발전됐지만 불만족스런 퍼포먼스의 extended filesystem을 공개했음.

 • Ext2가 2년 뒤에 공개됐음.

<br>

주요 특징

 • block의 size는 1024 ~ 4096 bytes

 • inode 다수

 • disk block을 group으로 나눈다. data block과 inode가 인접한 track에 저장되는 group

 • 일반 파일에 data block을 사전 할당한다.

 • 시스템 충돌의 충격을 최소화할 file-updating의 신중한 구현

 • 부팅 시 자동 일관성 검사 지원

 • 불변 파일 & 추가 전용 파일 지원

<br>

모든 파티션의 첫 번째 block에는 항상 boot sector임.

파티션의 나머지 부분은 block group들로 나뉨.  
<br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/ext2.png){: .align-center}{: width="50%" height="50%"}
