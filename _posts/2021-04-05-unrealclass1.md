---
layout: single
title:  "언리얼 수업 (1) : 언리얼엔진4 설치 및 프로젝트 생성, 기본 구성 요소"
categories: UnrealClass
tags: [UnrealClass, UnrealEngine]
toc: true

---
<br><br>
# 언리얼엔진 설치
<br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-1.png)
<br><br>
에픽게임즈 런쳐에서 상단의 '라이브러리'를 눌러, '엔진 버전' 란에서 버전을 선택해 설치한다.
설치한 버전을 누르면 언리얼 엔진이 켜진다.
<br><br><br><br>
# 새 프로젝트 생성
<br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-2.png)
<br><br>
'언리얼 프로젝트 브라우저' 라는 새 창이 뜬다.<br>
기존 프로젝트들은 위에 나오고, 게임용 프로젝트를 선택하려면 맨 위의 게임을 눌러주면 된다.<br>
(언리얼마다 기본적으로 타입마다 다르게 제공해주는 템플릿이 있고, 게임은 당연히 게임 관련 템플릿)


<br><br><br>
## 1. 템플릿 선택
<br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-3.png)
<br><br>
이후 다음을 누르자. 기본으로 만들어서 처리를 할 것이다. (아무 것도 없는 상태에서 시작)
<br><br>
## 2. 프로젝트 세팅
<br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-4.png)
<br><br>
프로젝트 세팅 화면이 나온다.<br>
여기 세팅들은 나중에 변경이 가능하다. (하지만 이왕이면 지금 확정해놓고 덜 바꾸는 게 좋을 것이다.)

### 2-1. 블루프린트 / C++
첫 번째 항목은 블루프린트로만 만들거면 블루프린트를 선택하지만, C++도 사용할 예정이라면 C++을 선택한다.
블루프린트란, 언리얼에서 노드들을 배치해서 연결해서 코드를 짤 수 있게끔 만들어놓은 스크립트 시스템으로,
c++이 아니라 기능 단위의 노드를 배치, 연결해서 코드를 만들어나가는 방식이다.
장점은 쉽다. 잘 짜여져 있으며, 잘 만들어져 있다. 단점은 c++에 비하면 역시 무겁고 느리다는 것이다.
간단한 프로젝트면 블루프린트만으로 충분히 가능하지만 덩치 있는 애 만들려면 블프만으로는 힘들 수 있다.

### 2-2. 타겟 플랫폼
두 번째 항목은 타겟 플랫폼을 설정하는 것으로, '데스크톱/콘솔'과 '모바일/태블릿'이 있다.<br>
모바일이 많이 좋아지긴 했지만 그래도 pc에 비하면 한참 떨어진 성능이다.<br>
"고퀄 세팅을 할 것이냐, 모바일용으로 적당히 설정할 것이냐" 우리는 고퀄 세팅으로 간다.

### 2-3. 프로젝트 성능 특성
세 번째 항목인 프로젝트 성능 특성은 최대 퀄리티로 설정한다.

### 2-4. 시작용 컨텐츠 유무
네 번째 항목인 시작용 콘텐츠 : 언리얼에서 제공하는 기본 애셋 (메쉬, 이미지, 애니메이션 etc.)<br>
없이 할 것이다.

### 2-5. 레이 트레이싱 활성화 여부
마지막 항목은 레이 트레이싱이다.<br>
레이 트레이싱이란, 3d 공간에서 어떤 조명 계산을 좀 더 현실적으로 해줄 수 있는 기술을 말한다.<br>
실제 레이 트레이싱으로 하느냐, 아니면 언리얼에서 기본적으로 제공하는 물리 기반 렌더링으로 하느냐?<br>
물리 기반 렌더링도 굉장히 현실감 있는 화면을 디테일하게 보여줄 수 있지만, 레이 트레이싱에 비하면 많이 부족하다.<br>
그런데 레이 트레이싱 하려면 최소 그래픽카드가 RTX 20후반급은 되어야 한다. PC는 당연히 하이엔드급이 되어야 함.<br>
그래서 비활성화를 선택할 것이다.

이제 하단에 프로젝트 저장할 위치 폴더 지정한 다음에 프로젝트 이름 적고 '프로젝트 생성'을 눌러준다.
<br><br><br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-5.png)
<br>
아마 새 화면이 뜨기까지 시간이 오래 걸릴 것이다. 언리얼은 빌드 시간이 반이라고 한다. 이제부터 **"기다림의 시간"**이 시작되는 것이다.
<br><br><br><br>
# 프로젝트 생성 후 열리는 파일은 2개
c++ 베이스이므로 비주얼스투디오도 하나 열리고 언리얼 프로젝트도 따로 열린다. 총 2개 열림.

<br><br><br><br>
# 프로젝트 저장 폴더 內 파일
프로젝트 저장 폴더가 생성되었을텐데, 구성품을 하나씩 살펴보도록 하겠다.
<br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-6.png)
<br><br><br>
## 1. 프로젝트 파일
맨 밑에가 프로젝트 파일. 얘 더블클릭해서 실행할 수도 있다. 절대 지워서는 안 됨!
<br><br>
## 2. .vs
.vs 폴더는 비주얼스투디오 때문에 같이 생성된 애임. 지금 필요하진 않다
<br><br>
## 3. Binaries
Binaries 폴더에는 실제 실행 파일같은 것들이 나옴
<br><br>
## 4. Config
Config는 .ini 파일들이 들어와있는데, 언리얼 설정 정보들이 들어와있는 파일들임.
<br><br>
## 5. Content
Content는 우리가 만들어놓거나 받아놓은 애셋, 리소스들이 들어온다. 당연히, 절대 지워져서는 안 됨! 플레이어 메쉬, 몬스터 메쉬 다 들어오니까.
<br><br>
## 6. DerivedDataCache
딱히 노필요
<br><br>
## 7. intermediate
intermediate는 빌드할 때 필요한 것들이 들어와있다.
<br><br>
## 8. Saved
Saved는 스크린샷 등등 일시적으로 저장되는 것들이 있다. 언제든 새로 만들어질 수 있으므로 급하게 필요하지 않다.
<br><br>
## 9. Source
Source는 실제 c++ 코드가 들어온다.
<br><br>
## 10. vs 솔루션 파일
비주얼 솔루션 파일은 지워져도 전혀 상관이 없다.<br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-7.png)
<br>
이렇게 프로젝트 파일 우클릭해서 코드 파일을 만들 수 있기 때문.
<br><br><br><br>

# 프로젝트 창 살펴보기
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-8.png)
<br>
이제 프로젝트 화면으로 돌아오자.<br>
하단에 '콘텐츠 브라우저'가 있다. 맨 왼쪽의 버튼을 누르면,
<br><br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-9.png)
<br><br>
이렇게 양쪽으로 디렉토리가 나온다.<br>
블루프린트나 asset은 'contents' 폴더로, c++ 코드는 'c++ 클래스' 폴더로 들어온다.
<br><br><br>
## 프로젝트에서 visual studio 파일 열기
<br><br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-11.png)
<br><br>
C++ 클래스 폴더를 누르면 프로젝트 폴더가 나오고, 이걸 누르면 'GameModeBase' 파일이 하나 있다. 이 파일을 누르면 visual studio 파일을 열어준다.
<br><br><br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-10.png)
<br><br>
UE4가 '실제' 언리얼 엔진 소스코드다.<br>
언리얼은 오픈 소스이다. 엔진 실제 내부 코드를 전부 볼 수 있고, 고쳐서 쓸 수도 있다.<br>
언리얼 엔진도 범용 엔진이다 보니 비효율적인 부분도 존재하기에, 큰 회사들에서는 엔진을 원하는대로 커스터마이징해서 사용한다.<br>
하단의 source 파일에 소스 파일이 존재한다.
<br><br><br>
## 뷰포트
<br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-11.png)
<br><br>
다시 프로젝트로 돌아와서,<br>
가운데에 크게 보이는 화면을 '뷰포트'라고 하는데,<br>
'우클릭 + 드래그' 시 '카메라 회전'이 가능하고,<br>
'우클릭 + W/A/S/D' 시 '카메라 이동'이 가능하다.
<br><br><br>
## 액터
뷰포트 안에 몇 가지가 배치가 되어 있다.<br>
하늘도 바닥도, 조명 등등이 배치되어있는데, 배치되어 있는 것들은 오른쪽 상단의 '월드 아웃라이너'에 있다.<br>
'액터'란, 이렇게 **월드 상에 배치된 물체 클래스**를 의미한다.<br>
지금 배치된 모든 것들은 전부 액터를 상속받아서 만들어진다. (액터도 클래스다. 월드상에 배치 가능한 클래스)
<br><br><br>
## 기본 물체 배치 및 조작
화면 좌측에 액터 배치가 있는데, 언리얼에서 기본으로 만들어 놓은 액터 종류들이 나열되어 있다.<br>
드래그하면 바로 배치된다. 큐브를 드래그해보자
<br><br><br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-12.png)
<br><br>
큐브에 나온 축을 '기즈모'라고도 부른다.<br>
<br><br>
### 물체의 좌표 시스템
기즈모의 좌표 시스템은 정면의 빨간색이 x축, 오른쪽이 y축, 위가 z축이다.<br>
기즈모를 잡고 마우스를 끌어서 이동시킬 수 있다.<br>
뷰포트 상단 우측 버튼들 중 맨 왼쪽 세트에서, 두 번째를 누르면 원하는 축을 기준으로 회전시킬 수 있다.<br>
세 번째를 누르면 해당 축방향으로 크기 조절을 할 수 있다.
<br><br>
### 물체 제거
물체에 대고 delete를 누르면 물체가 제거된다.
<br><br>
### 바닥에 물체 붙이기
<br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-13.png)
<br><br>
사진과 같이 물체가 떠있는 상태에서, 키보드의 End 키를 누르면 바닥에 딱 붙는다. 이걸 통해서 바닥 위에 물체들을 편하게 배치시킬 수 있다.
<br><br><br>
## 디테일 tab
화면 우측 중단에 '디테일' 탭이 있는 것을 확인할 수 있을 것이다.<br>
우측 상단의 '월드 아웃라이너'에서 선택한 물체의 세부 사항을 보여주는 곳이 바로 '디테일' 탭이다.<br>
트랜스폼은 월드상에서의 위치, 회전, 크기 정보를 갖고 있다. 이 곳의 값을 조작하는 것으로도 물체 정보를 변경할 수 있다.<br>
모빌리티는 추후에 다루도록 한다.
<br><br><br>
## 탭 바
맨 위에 탭 바에는 자주 사용하는 기능들이 즐겨찾기 식으로 올라와 있다.<br>
'플레이' 버튼은 실제로 플레이해보는 것. 누른 후 다시 esc를 누르면 빠져나올 수 있다.


# 액터, mesh, 컴포넌트
언리얼에서 기본으로 제공하는 액터가 정말 많은데, 웬만해서는 거의 사용하지 않을 것이다.<br>
대신, 어떤 기능들을 가진 액터를 만들어서 배치해 사용하게 될 것이다.<br>
액터는 배치할 수 있다.<br>
그런데 액터는 말 그대로 액터일 뿐, "**빈 껍데기**"다.<br>
큐브를 배치했을 때 나오는 큐브의 모양, 구체를 배치했을 때 나오는 구체 모양, 이렇게 3차원 공간에 출력하는 건 액터가 아니라, **mesh**이다.
<br><br><br>
## Mesh
mesh같은 경우에는 두 가지 종류가 있다. static mesh, skeletal mesh가 있다.
<br><br>
### 1. Static Mesh
정적인 mesh를 말한다. 모션이 동작되거나 그런 게 없음. 돌, 나무 같은 것들이 보통 static mesh로 제작된다.
<br><br>
### 2. Skeletal Mesh
bone을 가지고 있으면서, 애니메이션을 넣을 수 있는 것들을 일컫는다. 주로 캐릭터 같은 애들을 skeletal mesh라 부른다.
<br><br><br>
## 컴포넌트
언리얼의 기본 동작 방식은 컴포넌트 구조이다.<br>
액터는 월드에 배치할 수 있는 단순 껍데기일 뿐이고, 어떤 액터에 기능을 추가하고 싶으면, 이미 언리얼에서 굉장히 많은 컴포넌트를 만들어놨다.<br>
그래서 내가 액터에 어떤 컴포넌트들을 조합했느냐에 따라서 기능이 달라지는 것이다.<br>
이것이 바로 컴포넌트 구조이다. 유니티와 언리얼 모두 컴포넌트 시스템으로 구성되어 있다.
<br><br>
### static mesh 컴포넌트
mesh가 화면에 나올 수 있는 이유는 static mesh 컴포넌트 때문.<br>
스static mesh를 가지고 있으면서, 그걸 화면에 출력해주는 컴포넌트가 바로 static mesh 컴포넌트이다.<br>
빈 액터만 넣으면, 그냥 껍데기만 있어서 출력이 안 된다. 따라서 반드시 출력되는 컴포넌트가 들어와야 한다.
<br><br>
컴포넌트는 종류가 크게 2가지가 있다. 세분화하면 아주 다양하지만 정말 크게 분류하자면 그렇다는 의미이다.
<br><br>
### 1. Scene 컴포넌트
트랜스폼 정보를 갖고 있는 컴포넌트를 일컫는다. (위치, 크기, 회전)
<br>
#### Primitive 컴포넌트
scene 컴포넌트의 한 종류인데, 실제 화면에다가 어떤 '모양'을 "출력"하는 그런 컴포넌트를 primitive 컴포넌트라고 부른다.<br>
scene 컴포넌트를 상속받아서 만들어지기에, scene 컴포넌트가 가진 트랜스폼 정보를 갖고 있다.
<br><br>
### 2. 액터 컴포넌트
위치 등 정보를 갖고 있지는 않지만 액터가 특별한 기능을 수행할 수 있도록 보조하는 역할의 컴포넌트이다.
scene 컴포넌트도 액터 컴포넌트를 상속 받아 만들어진다.<br>
scene 컴포넌트는 트랜스폼 정보를 갖고 있지만, 트랜스폼 정보가 없는 컴포넌트들도 많이 있다.
<br>
#### Movement 컴포넌트
가령, 물체를 움직일 수 있는 movement 컴포넌트가 있는데, 이 경우 위치 정보가 필요없다.<br>
그냥 물체를 움직이는 역할만 수행할 수 있으면 되는 것이다.<br>
<br>
다시 말해서 액터 컴포넌트란 "액터에 어떤 기능을 추가해주는!" 컴포넌트라고 보면 된다. 액터는 스스로는 할 수 있는 게 없다.<br>
**어떤 컴포넌트가 구성이 되고, 그 구성된 컴포넌트에 맞춰 내가 코드를 짜서 특별한 액터 하나를 만들어내는 방식**으로 동작이 된다.

# 기타
## 클래스의 이름
<br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-14.png)
<br><br>
GameModeBase 클래스 앞에 A가 붙어있음을 알 수 있다.<br>
앞에 A가 붙으면 전부 액터 종류이다.<br>
앞에 U가 붙으면 액터가 아닌 일반 클래스들을 의미한다.<br>
F12로 쭉 상속 구조를 따라가면 Actor 상속 받고 있다는 걸 알 수 있을 것이다.<br>
배치되는 모든 아이들이 결과적으로는 actor를 상속 받고 있다는 의미이다.
<br><br><br>
## 게임 모드 클래스
제일 먼저 만들어져 있는 클래스가 게임 모드 클래스이다.<br>
언리얼에 보여지는 화면뿐 아니라, 다른 화면들을 여러 개 만들어놓고 원할 때 그쪽으로 넘어가고 식이 가능하다.<br>
하나의 월드에 내가 원하는 게임 모드를 각자 따로 지정하는 것도 가능해서,<br>
게임 모드가 속한 월드의 규칙을 정하고 전체적으로 월드 상에서 처리해야 하는 기능의 경우 게임 모드를 통해 관리하고 정해줄 수 있다.<br>
매니저 같은 느낌이라고 생각하면 된다. (단, 싱글톤으로 되어 있는 것은 아니다.)<br>
아무튼 월드에 들어갈 때 기본적으로 생성되는 녀석이다.
<br><br><br>
## 3D에서의 조명과 성능
뷰포트에 배치된 물체들을 간단하게 살펴보자.<br>
언리얼을 이용해 3d 게임을 우리는 만들고자 한다. (paper 2d라는 걸로 2d 만들 수도 있지만, 아무튼 언리얼은 기본은 3d 베이스이다.)<br>
3d이다 보니 기본적으로 조명이 필요하다. 조명은 많이 쓰면 성능 저하와 직결된다.
<br><br><br>
## player start
<br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-15.png)
<br><br>
'월드 아웃라이너'에서 'player start'는, '여기서 플레이어 캐릭터가 시작한다'는 의미를 담은 녀석이다.
<br><br><br><br>
# 저장 및 관련 설정
우선 프로젝트를 성공적으로 생성하고, 기본적인 요소를 알아보았으니, 이제 슬슬 작업을 시작할 것이다.<br>
제일 먼저 작업해볼 것은 캐릭터 플레이어. 우리가 만들어놓은 공간을 움직이면서 탐험할 수 있어야 하니 가장 먼저 작업하고자 한다.
<br>
작업하기에 앞서 일단 현재 '**레벨**' 저장을 할 것이다. 정확하게는 하나의 월드에는 한 개 이상의 레벨이 들어온다. 레벨 하나로도, 또는 여러 레벨을 합쳐서 월드를 구성할 수 있다.<br>
비동기 레벨 스트리밍 시에 이에 대해 더욱 자세히 알아보자. 지금은 레벨 하나로만 월드를 구성할 것이다.<br>
<br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-17.png)
<br><br>
위의 사진과 같이, '콘텐츠' 폴더 내에 '**levels**' 폴더를 만들어서 저장.
<br><br>
그런데 이렇게 해서 프로젝트 창을 끄고 다시 켜면 우리가 만들어놓은 메인 레벨이 안 나오고, 처음의 그 빈 뷰포트만 나온다.<br>
아래 컨텐츠 브라우저에서 메인 레벨을 찾아서 열어줘야 나오는데, 이렇게 번거로운 일 없이 아예 켤 때부터 메인 레벨이 나오게 하려면 조금 설정을 해주어야 한다.
<br>
상단 탭 바에서 '세팅'을 누르고, 거기서 또 '프로젝트 세팅'을 눌러준다.
그러면 다음과 같은 창이 켜진다.
<br>
![image](https://raw.githubusercontent.com/everjaewon/everjaewon.github.io/master/assets/images/1-18.png)
<br><br>
좌측에 '맵 & 모드' 탭을 누르자.

가장 위의 Default Modes부터 살펴보면, 모든 월드에서 여기서 설정한대로의 모드를 사용한다.<br>
만약 각각의 월드에 내가 원하는 게임모드를 따로 설정하면 기본 게임모드는 무시된다.<br>
이 곳은 c++ 코드에 만들어진 UE88GameModeBase로 선택해주도록 하자.

밑의 Default Maps에서, 에디터 시작 맵을 main으로 설정.<br>
그 밑의 '게임 기본 맵'은 게임을 다 만들고 실행 파일을 시작했을 때, 동작되는 맵을 의미한다.<br>
이런 식으로 기본 동작되는 레벨을 변경할 수 있다.

레벨 저장 관련 세팅도 완료하였다! 이제 다음 시간부터는 플레이어 캐릭터를 작업해보자.
