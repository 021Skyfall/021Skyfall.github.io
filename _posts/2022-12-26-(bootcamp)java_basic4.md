---
title: "자바 : 배열"
author: Jeremiah Lee
date: 2022-12-26
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

코드는 카독성이 좋아야한다   

! 변수 이름부터 누가봐도 알아볼 수 있도록

! 깔끔하게 한 눈에 싹 들어오게

<br>

### Array

동일한 데이터 타입들을 모아 간편하게 사용하기 위함 (안 그러면 변수 뒤지게 많이 부름)

<br>

기본타입 : 메모리 공간 확보 후에 이름 붙임 ex) int i = 0;

참조타입 : 주로 배열이 해당하는데 선언 할 때에는 요소가 몇 개 들어갈지를 모르니 정의하고 나서 크기가 정해짐, 즉 해당 주소 값을 저장할 수 있는 만큼 나중에 확보된다는 것. 이 말은 해당 요소를 부르면 그 크기 만큼의 전혀 다른 곳에 배열 값이 생성된다는 것임. 이후 선언된 배열 변수와 연결되어 출력됨
String과 비슷하게 length() 메소드가 있지만 배열은 메소드가 아니라 정의가 되어있어 length로만 불러옴
또 String과 다르게 charAt 안쓰고 배열 안에 인덱스만 넣어주면 순회 가능함   
~> 쉽게 말해서 일반적인 변수 선언은 선언과 동시에 이름이 붙여지고 주소, 즉 용량이 할당되는데에 반해
배열은 변수 선언은 그냥 이름만 붙이는 거고 요소를 할당해야 주소, 용량이 따라붙음
그래서 배열은 주소가 0x12345678 : 0x98765432 이런식으로 나옴

<br>

! 변수에 요소를 할당하는 것을 초기화라 함

<br>

주로 Array 클래스를 활용함

<br>

배열의 차원은 그 배열이 얼만큼 중첩되어있느냐를 말하는 것

ex)

1차원 배열 : int[] heights = new int[] { 1, 2, 3, 4 };

2차원 배열 : int[][] info = new int[][] { { 1, 2, 3, 4 }, { 5, 6, 7, 8 } };

<br>

가변 배열

2차원 이상의 다차원 배열에서 자유로운 형태로 배열 만들기가 가능하며 2차원 이상일 때 마지막 차수에 해당하는 배열의 길이를 고정하지 않아도 됨

ex) int[][] ages = new int[5][]; => 외부 배열 크기를 5로 지정, 마지막 지정안함 => 가변 배열 생성

<br>

★ new 연산자

= 객체를 Heap이라는 메모리 영역에 공간을 할당해주고 메모리 주소를 반환한 후 생성자를 실행시킴new연산자로 생성된 객체는 동일한 값을 가진 객체가 있어도 서로 다른 메모리를 할당하기 때문에 서로 다른 객체로 분류함

[참고 출처](https://yoo11052.tistory.com/52)

<br>

☆ 리터럴 방식

= 고정된 변수에 할당하는 것

리터럴은 변하지 않는 고유한 값을 말함

ex) int(타입) many(변수) = "str"(리터럴) < 리터럴 방식

! 실제로 new연산자를 많이 쓸거같음, 메모리 용량은 더 잡아먹겠지만 변수에 할당한 리터럴이 다른데서도 쓰일 수 있으니... 앞으로 new 연산자 많이 쓰도록!

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/222966368714)에서 옮겨짐
