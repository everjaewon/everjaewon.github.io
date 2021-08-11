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
<br><br><br><br><br>
# <span style="color:yellow">핵심 질문</span>
⑴ Allocation method에 대해 설명<br>
⑵ inode란 무엇인가?<br>
⑶ Free space management 설명<br>
⑷ VFS에 대해 설명<br>
⑸ 디스크 캐싱에 대해 설명<br>
⑹ Ext2 파일 시스템에 대해 설명
<br><br><br><br>
⑴<br>파일 할당 방법으로는 contiguous allocation, linked allocation, indexed allocation이 있다.
contiguous allocation은 파일의 시작지점부터 연속적인 disk block을 할당하는 방법으로, 시작 지점과 할당된 block의 수를 가지고 효과적인 임의 접근이 가능하다. 다만 시작 지점의 지정이 동적이기 때문에 파일을 수정하기 어렵고, 공간 낭비가 발생한다는 단점이 있다.<br><br>
linked allocation은 각 파일을 disk block의 연결 리스트로서 할당하는 것을 말한다. 연결 리스트 특징상 disk block들은 디스크 상 어디든 위치할 수 있다. 연속적이지 않기에 임의 접근에는 비효율적이고, 포인터를 위한 추가 공간과 포인터 손실 시의 안정성 문제는 단점이 된다.<br><br>
indexed allocation은 하나의 지정된 index block 안에 파일을 구성하는 모든 disk block을 가리키는 포인터를 index화하는 방법을 의미한다. 외부 단편화의 우려없이 상당히 효율적인 임의 접근이 가능하다. UNIX의 파일 시스템 inode가 이러한 할당 방법으로 구성되어있다.<br><br><br>
⑵<br>inode는 UNIX 계열 파일시스템에서 사용하는 자료구조이다. 모든 파일마다 존재하며, 파일 모드, 소유자 및 그룹 정보, 파일 크기와 주소 등 파일에 관한 일반적인 정보를 담고 있다. 상대적으로 작은 고정 크기 덕에 메인 메모리에서 장시간동안 보관이 가능하며, 파일의 크기가 작을수록 거의 간접적이지 않게 접근이 가능하다는 점에서 접근 시간이 줄어든다.<br><br><br>
⑶<br>사용가능 공간을 관리하는 방법으로는 크게 4가지가 존재한다.<br>
우선 비트벡터/비트맵을 이용하는 방식으로, n개의 block을 순번에 따라 차례대로 사용 중인지, 아니면 사용 가능한지를 체크하기 때문에 연속적인 파일을 얻기가 쉽다. 다만 해당 비트맵에 대한 추가 공간이 필요하다.<br>
연결 리스트를 이용하는 방식은 모든 사용가능 disk block을 첫 번째 block에서부터 포인터를 통해 쭉 연결하는 방식으로, 포인터를 사용하기 때문에 공간 낭비는 없지만 연속 공간을 확보하기는 어렵다.<br>
그룹화 방식은 사용가능 block을 n개씩 묶어서, n-1개 block을 사용가능하게 두고 마지막 block만 다음 n개 block에 대한 주소를 담고 있도록 하는 방식이다. 맨 처음 block은 다음 n개 block에 대한 주소를 담는다.<br>
카운팅 방식은 어떤 사용가능 block의 주소와, 그 block을 뒤잇는 사용가능 block이 몇 개나 연속적으로 있는지를 따지는 방식이다.<br><br><br>
⑷<br>가상 파일 시스템이란 표준 UNIX 파일 시스템에 관한 모든 시스템 콜을 다루는 커널 소프트웨어 계층을 말한다. 사용자 모드에서의 프로세스와 동일한 인터페이스를 제공하고, 모든 서로 다른 파일 시스템 구현에 대한 커널 추상화를 제공하는 것이 목표로, 파일 및 파일 시스템에 관한 시스템 콜, 자료구조, 검색 루틴 등의 기능을 갖추고 있다.<br><br>
Linux에서는 가상 파일 시스템을 4개의 객체로 구성하고 있다. 설치된 파일 시스템 정보가 저장된 superblock, 파일의 일반적인 정보가 저장된 inode, 열린 파일과 프로세스 간 상호작용 정보를 담은 file, 디렉토리 구조와 파일 간 연결 정보를 담은 dentry가 바로 그것이다. 커널 메모리 상에서 4개 객체로 이루어진 리눅스 VFS가 작동한다고 볼 수 있겠다.<br><br><br>
⑸<br>디스크 캐싱은 프로그램이 파일을 읽고 쓰는 데에 있어서 뚜렷한 locality를 보이기 때문에, 파일의 버퍼 캐시 內 disk block을 캐싱한다면 이 locality를 쉽게 파악할 수 있을 것이라는 발상에서 출발한 방법이다. 캐시 자체가 시스템 전반에서 널리 쓰이고 있고, 캐시로부터 읽어들이는 것이 마치 디스크를 메모리처럼 작동하게 만들어주기 때문에 저용량의 캐시만으로도 굉장히 효과적이며, 디스크 트래픽과 불필요한 디스크 I/O를 없애준다는 장점이 있다. 여기에 더해 가상 메모리 기술을 이용하여 disk block 대신 page를 캐싱하는 페이지 캐시가 존재한다. 보통은 페이지 캐시가 메모리에 매핑된 입출력을 캐싱하고, 버퍼 캐시가 시스템 콜을 사용하는 입출력을 캐싱한 뒤 두 캐시 간 일관성을 맞추는데, 이 때 캐시 간의 일관성이 없거나, 이중 캐싱이 발생할 경우를 방지하기 위해 unified 버퍼 캐시만을 사용하기도 한다. 이는 메모리에 매핑된 입출력과 시스템 콜을 이용한 입출력 모두에 동일한 캐시를 적용하는 방법이다.<br><br><br>
⑹<br>ext2는 1994년에 소개된 Linux의 파일 시스템이다. disk block을 data block과 inode가 인접한 위치에 저장되는 group으로 나눈다. ext2 파티션에서의 첫 번째 block은 항상 부팅 관련 부분이며, 나머지 파티션을 동일한 크기의 (앞서 언급한) group들로 나눈다. group 內 superblock과 group descriptors는 모든 group이 동일하게 복제되지만, 0번째 group에서의 superblock, descriptors만 커널에서 사용된다. 주어진 파일의 데이터는 커널에 의해 가능한 한 하나의 block group 안에 포함되기 때문에, 외부 단편화 문제를 줄일 수 있다.
