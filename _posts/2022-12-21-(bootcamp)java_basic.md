---
title: "자바 : 데이터 타입과 변수"
author: Jeremiah Lee
date: 2022-12-21
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

### 언어 동작 순서   
Java  
소스코드 -> 컴파일러(javac) -> 바이트코드(class) -> JVM -> OS 

C 언어   
소스코드 -> 컴파일러 -> 바이트코드 -> OS


### Java 기초   
함수 (function) : 특정 기능을 수행하는 코드들을 묶은 것   
메소드 (method) : 클래스 내에 포함되어 있는 함수   
/ 특정 기능을 수행하기 위한 코드들을 묶어 놓은 것으로 데이터를 입력 받아 그 데이터에 일련의 처리를 한 후, 결과값을 반환하는 것을 의미   
매개변수 (parameter) : 메소드 외부와 메소드 내부를 매개해주는 변수   
변수 (variable) : 특정 데이터를 임시 저장   
! 자바에서 일반적으로 변수명은 camelCase사용 >> 앞 소문자 뒤 대문자   
상수 (invariable) : 말 그대로 변하지말아야 할 데이터를 임시 저장   

기본 타입 (primitive type) : 데이터의 실제 값 저장   
정수 타입 : byte, short, int, long(L)   
실수 타입 : float (f), double   
문자 타입 : char   
문자열 타입 : String   
논리 타입 : boolean   

참조 타입 (reference type) : 데이터의 주소값 저장 // 기본 타입 제외 전부   

앞으로 값 = 리터럴 이라 불러야함

### 정수 타입 중   
long은 리터럴 뒤에 L을 붙여야함   
오버플로우 : 자료형이 표현할 수 있는 범위 중 최대값 이상의 값을 표현한 경우 발생, 최대값을 넘어가면 해당 데이터 타입의 최소값으로 값이 순환   
언더플로우 : 자료형이 표현할 수 있는 범위 중 최소값 이하의 값을 표현한 경우 발생, 최소값을 넘어가면 해당 데이터 타입의 최대값으로 값이 순환   

### 실수 타입 중   
float은 리터럴 뒤에 f를 붙여야함

### 자동 타입 변환   
byte(1) -> short(2)/char(2) -> int(4) -> long(8) -> float(4) -> double(8)   
== 메모리 용량이 더 큰 타입에서 작은 타입으로는 자동으로 타입 변환 X

### 문자열 타입   
클래스는 거푸집, 그걸 통해서 찍어낸 것이 인스턴스 이때 new 연산자 사용   
chaAt() : 문자열의 특정 인덱스에 해당하는 문자 반환 / 더 크거나 음수를 전달하면 오류   
compareTo() : 대소문자를 구분하여 문자열을 인수로 전달된 문자열과 사전 편찬 순으로 비교 / 작으면 음수, 크면 양수 반환   
compareToIgnoreCase() : 위랑 같은데 대소문자 구분 X   
concat() : 문자열의 뒤로 인수로 전달된 문자열을 추가한 새로운 문자열을 반환 / 쉽게 말해 이 메소드 문자열을 기존 문자열 뒤로 이어 붙여줌   
indexOf() : 해당 문자열에서 특정 문자나 문자열이 처음으로 등장하는 위치 인덱스 반환, 없으면 -1 // 특정 문자 찾을 때 유용할 듯   
trim() : 문자열의 맨 앞과 맨 뒤에 포함된 모든 공백 제거   
toLowerCase() & toUpperCase() : 소문자 & 대문자 변경   

StringBuilder : 문자열 이어붙이기 // 문자가 너무 많을 때 사용하기 좋을 듯

StringBuffer : 원래 String 클래스의 인스턴스는 한 번 생성되면 그 값 읽기만 되는데 해당 함수로 수정, 삽입, 삭제 등 가능


+ String.format() : 문자열 생성   
  == System.out.printf() : 같은 형식이지만 출력

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/222961996113)에서 옮겨짐
