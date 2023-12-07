---
title: "컴퓨터 사이언스와 프론트 엔드 기초"
author: Jeremiah Lee
date: 2022-12-16
categories: [ 컴퓨터 사이언스, 기초 ]
tags: [컴퓨터 사이언스, 부트캠프]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Academic_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

### **컴퓨터 사이언스**
```
인풋 -> 컴파일 -> 프로그램 -> 처리 -> 저장 -> 아웃풋   
       (              빌드               )
```


소프트웨어   
시스템 : Mac, Window, Linux, Node.js, JRE 등의 런타임 환경   
응용 : 앱, 모든 프로그램   

하드웨어   
중앙처리장치(CPU)   

제어장치 =버스= =버스= 연산장치   

레지스터 =버스=   

제어장치 = 리소스 통제, 관리   
연산장치 = 명령어 실행   
레지스터 = 데이터 임시 보관   

기억 장치
== 저장장치, CPU 동작에 필요한 데이터 장기/단기 보관   
주기억장치 : 데이터 처리속도 빠름 // 프로그램 실행 중 모든 데이터 저장 // RAM, ROM   
보조기억장치 : 정보를 반영구 보관 // 처리속도 느린 대신 용량 큼 // SSD, HDD   

캐시 메모리   
: CPU가 사용하고 저장한 데이터 중 가장 재사용 가능성이 높은 데이터를 미리 로드해서 대기시킴

쉽게   
데이터 = 현금   
주기억장치 = ATM기기   
캐시 메모리 = 지갑   

간단하게   
보조기억 => 주기억 => 캐시 => CPU 레지스터 순으로 빠르고 용량 작음

시스템 버스   
: 하드웨어를 전선으로 물리적 연결하는 통로
데이터 버스 : 하드웨어 간 데이터 전송 통로
주소 버스 : 데이터 도착지 주소 전달 통로
제어 버스 : CPU 제어 신호를 다른 장치로 전달하는 통로


프로그래밍이란?   
프로그램을 만드는 과정으로, 특정 목적을 달성하기 위해 설계된 알고리즘을 프로그래밍 언어를 사용하여 코드로 작성하는 과정

***

### **웹 애플리케이션 개발 구조**

클라이언트 == 홀 업무 // 눈에 보이는 그래픽적인 업무   
↑ ↓   
서버 == 주방 업무 // 보이지 않는 토대 구축 => 2티어 아키텍쳐 // 대부분 여기까지   
↑ ↓   
데이터 베이스 == 창고 // 리소스 저장, 출력 => 3티어 아키텍쳐   


#### **프론트엔드 기초**

HTML == 구조 && 내용   
CSS == 스타일 && 레이아웃   
JS (Java Script) == 상호작용   



1. HTML
   HyperText Markup Language
   프로그래밍 언어가 아닌 마크업 언어의 한 종류로 설계도 제작용 언어
   트리형으로 연결되어 있음 => 직접보면 느낌 암

```
<h1>...<h6> == Head
<div> == Division
<span> == Image
<a> == Link
<ul> & <ol> & <li> == Unordered List & Ordered List & List item
<input> == Input (text, radio(택1 체크), checkbox(다수 체크))
<textarea> == Multi-line Text Input
<p> == paragraph
<section> != <div> :
<section>은 독립적 개체, 시멘틱 요소의 일부, 따로 떨어져 나가도 그 하나만으로 의미가 클때 // 예를 들어 해당 부분만으로도 전달하고자 하는 의미가 충족될 때
<div>는 큰 개체 안에서 구역을 구분하고자 할 때 // 칸막이
```

2. CSS
   Cascadin Style Sheets : 말그대로 폭포처럼 흐르는 특성을 표현

```
   <UI> != <UX>
   <UI>는 유저 인터페이스로 일반 사용자가 쉽게 컴퓨터에게 명령할 수 있게 만드는 인터페이스
   <UX>는 유저 익스피리언스로 UI를 포함한 특정 서비스를 직/간접적으로 경험하면서 사용자가 느끼는 만족도
```

분류

```
<id> : #으로 선택 // 한 문서에 단 하나의 요소에만 적용 가능 // 특정 요소 이름 붙이기
<class> : . 으로 선택 // 동일한 값을 갖는 요소가 많을 때 // 스타일 분류에 사용
우선 순위 : ID (#) > Class (.) > Element (p
```

폰트   
- 다수 폰트 적용 // 지원 안되는 폰트는 fallback라고 함

```
<font-family>
```

- 글꼴 크기 단위   
- 절대 단위 : px, pt 등 // 환경 영향 받지 않는 절대적인 크기   
- 상대 단위 : %, em, rem, ch, vw, vh 등 // 대표적으로 rem 사용   

정렬   

```
<text-align>
```

가로 정렬 // 유효 명령어 : left / right / center / justify(양쪽)

간격   
행간 

```
<line-height>
```

자간

```
<letter-weight>
```

줄바꿈   
Block : 자동 줄바꿈 O 

```
<div>, <p>
```   

Inline : 자동 줄바꿈 X, 박스가 늘어남 

```
<span> => inline-block으로 자동 줄바꿈 O   
```


### **CSS 박스 모델**
- Margin (바깥 여백) > Border > Padding (안쪽 여백) > Content   
- Margin border : 값을 생략하면 마주보고 있는 값으로 자동 지정됨 // 하나만 넣으면 네 방향 통일 //   
- margin-right 등으로 특정 위치 여백 조절 가능   
- 여백 조절 명령어

```
<padding> & <margin> 
```

- 10px(상) 10px(우) 10px(하) 10px(좌) ==> 시계 방향 순서   
- 10px(상하) 10px(우좌)   
- 10px(전방향)   
- 박스 바깥으로 컨텐츠가 빠져나가지 않게 하고 너무 길면 스크롤이 자동으로 생성되는 명령어 == overflow: auto;   

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/222957714222)에서 옮긴 글
