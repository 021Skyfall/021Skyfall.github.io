---
title: "자바 : 반복문"
author: Jeremiah Lee
date: 2022-12-23
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

### 반복문

for문 : for(변수선언+초기화; 조건식; 증감식)

변수 선언 : 사용할 변수의 초깃값

조건식 : 해당 조건이 true일 동안 반복

증감식 : 반복 횟수

<br>

enhanced for문 : for(변수 선언 : 배열)

배열에 저장된 항목만큼 반복됨

<br>

### while문

조건식이 false가 되도록 만들거나 break를 이용해 탈출해야함

<br>

do-while문

do {

실행문 // 처음 한번 무조건 실행됨

} while (조건식);

실행문 실행 후 조건식 체크 후 true면 반복 false면 종료

<br>

### break문

break는 가장 가까운 반복문만 종료 > 바깥에는 영향 없음

for문 안 쪽에서 바깥까지 break 걸고싶다면 가장 바깥 for문에 <Label> :  식으로 레이블 붙여주고 break <Label>;

<br>

### continue문

for문 while문 do-while 즉, 반복문에서만 사용

break와 달리 반복문이 끝나면 다음 차례로 넘어감

예제>

```java
for(int i = 0; i < 10; i++) {

            if(i%2==0) {

                continue; // 다음 반복으로

            }

            System.out.println(i);

        }
```

이해를 위해 이 예제의 경우를 보면,

i는 1씩 더하며 0~9까지 실행 > 0일 경우 if문 안에서 true값 > continue > for문 다시 실행 > 1일 경우 if문 안에서 false 값

sout 출력 > 9까지 반복이니 반복 ㄱ

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/222963799519)에서 옮겨짐
