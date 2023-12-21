---
title: "자바 : 연산자와 입출력 그리고 조건문"
author: Jeremiah Lee
date: 2022-12-22
categories: [ 자바, 기초 ]
tags: [자바, 부트캠프]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Academic_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

### 연산자

연산자 우선 순위   
```
우선순위       연산자                 내용

1             (),[]                 괄호 / 대괄호

2             !, ~, ++, --          부정/ 증감 연산자

3             *, /, %               곱셈 / 나눗셈 연산자

4             <, <=, >, >=          대소 비교 연산자

5             &&                    AND 연산자

6             ||                    OR 연산자

7             ? :                   조건 연산자

(용법) 조건식 ? 조건식이 true일 때 적용될 값 : 조건식이 false일 때 적용될 값)

8             =, +=, -=, /=, %=     대입/할당 연산자
```

### 특이사항

% : 좌항의 값을 우항의 값으로 나눈 나머지 반환

/ : 좌우 모두 int타입이면 몫만 // 하나라도 실수면 실수로

++ / -- (증감연산자) : 앞에 붙으면 증감부터 실행 / 뒤에 붙으면 기존 값 출력 후 증감 실행

+= 등 대입/할당 연산자 : 자기자신에 계산하는 것 > 계산하기 위해 변수 새로 할당 안해도뎀


### I/O (input/output)

System.out.print() : 콘솔창에서 줄바꿈 없이 쭉 이어 붙여서 출력함

System.out.println() : 줄바꿈 들어가서 출력 >> println과 같이 쓰니까 이전 print와 합쳐짐

System.out.printf() : String.format과 비슷한 문법으로 형식대로 출력 됨

ex) System.out.printf("지금은 %s입니다", 2022 + "year"); // 이때 %로 자동 타입 변환

### printf 형식   
```
지시자       출력 포맷

%b          Boolean

%d          10진수

%o          8진수

%x, %X      16진수

%c          Character

%s          String

%n          줄바꿈 (일반 줄바꿈은 "" 안에 \n)

%f          double // 정수 자리수도 정할 수 있는데 정수 자리수는 그냥 출력하면 다 나오고

                   // 소수 자리수는 안정해주면 기본 6자리까지 출력됨

                   // ex) %.1f  ~> 소수 1자리까지 출력
```

Scanner

문법과 시행착오는 깃헙에

☆ 콘솔창 스캔해서 값 읽고 출력함(값을 콘솔창에 입력하면 된다는 뜻) > 이거 몰라서 한참 헤맷다...

알고리즘이란 어떤 문제를 논리적으로 해결하기 위해 정해진 일련의 절차

IF와 switch의 문법과 시행착오는 깃헙에

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/222962828430)에서 옮겨짐
