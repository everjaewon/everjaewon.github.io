---
layout: single
title:  "MapReduce 기본 개념"
categories: DB
tags: [DB, ComputerScience]
toc: true

---
<br><br>
# 맵리듀스
## 개념
- 맵리듀스 프레임워크 → 개발자가 대량의 데이터를 병렬로 분석 가능 (in 대규모 분산 컴퓨팅 환경 or 단일 컴퓨터 환경)

- 맵리듀스 알고리즘에 맞는 분석 프로그램 개발<br>
  → 데이터 입출력, 병렬 처리 등 기반 작업을 프레임워크가 알아서 처리해준다

<br><br>
### 맵 / 리듀스 2개의 메서드로 구성

① 맵 : list(k1, v1) → list(k2, v2)<br>
- key와 value로 구성된 목록을 입력받아,<br>
  - 이를 가공 및 분류 後<br>
  - 새로운 key와 value로 구성된 목록을 출력<br>
- 맵 메서드가 반복해서 수행되다 보면, 새로운 key를 갖는 여러 개의 value가 만들어짐<br>
- 이 새로운 key로 grouping된 value의 목록을 리듀스 메서드의 입력 데이터로 전달

② 리듀스 : list(k2, list(v2)) → list(k3, v3)<br>
- value의 목록에 대한 집계 연산 실행 → 새로운 key와 value로 구성된 목록을 생성
<br><br>
ex) 두 줄 문장으로 구성된 txt 파일을 입력받아, 각 단어의 출현 빈도를 분석하는 프로그램

<br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/mapreduceexample.jpg) 
<br><br>

<br><br><br>
## 시스템 구성
<br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/mapreducesystem.jpg) 
<br><br>
### 클라이언트
 • 사용자가 실행한 ‘맵리듀스 프로그램’ & 하둡에서 제공하는 ‘맵리듀스 API’<br>
 • 맵리듀스 API를 통해 맵리듀스 프로그램을 개발하고, 이 프로그램을 하둡에서 실행 가능<br>
 • 클라이언트가 하둡으로 실행을 요청하는 맵리듀스 프로그램을, job이라는 작업단위로 관리<br>
<br><br>
### 잡트래커
 • 하둡 클러스터에 등록된 전체 job의 스케줄링을 관리 & 모니터링<br>
 • 전체 하둡 클러스터에서 하나의 잡트래커가 실행됨 (보통 네임 노드 서버에서 실행)<br>
 • 사용자가 새로운 job 요청 시, 해당 job 처리를 위해 몇 개의 맵, 리듀스를 실행할지 계산<br>
 • 계산된 맵과 리듀스를 어떤 태스크트래커에서 실행할지 결정 & 해당 태스크트래커에 할당<br>
<br><br>
### 태스크트래커
 • 잡트래커가 요청한 맵과 리듀스의 개수만큼, 맵 태스크 & 리듀스 태스크 생성<br>
 • 데이터 노드에서 실행되는 daemon<br>
 • 각 태스크가 생성되면, 새로운 JVM을 구동해 이들을 실행<br>
 • 잡트래커 & 태스크트래커 → ‘하트비트’라는 메서드로 네트워크 통신<br>
  → ‘태스크트래커의 상태 & 작업 실행 정보’ 교환<br>
 • 태스크트래커에 장애 발생 시, 잡트래커가 타 대기中 태스크트래커에 태스크 재실행 요청
