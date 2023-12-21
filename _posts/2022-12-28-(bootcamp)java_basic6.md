---
title: "자바 : OOP : 생성자와 이너 클래스"
author: Jeremiah Lee
date: 2022-12-28
categories: [ 자바, 기초, OOP ]
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

### 생성자
- 객체를 생성하는 역할
- 클래스의 구성 요소
- 인스턴스가 생성될 때 호출되는 인스턴스 초기화 메서드
- 생성자의 이름은 반드시 클래스의 이름과 같아야 함
- 생성자는 리턴 타입이 없음 > void 키워드를 사용하지 않음 > 아예 리턴 타입 자체가 존재하지 않기 때문
- 매개변수는 있을 수도 있고 없을 수도 있음   
=> 왜 쓰냐고? 매번 객체 생성할 때마다 값을 일일히 넣어줘야하는데 그럴 필요없이 여러개의 생성자로 편하게 만들 수 있기 때문

ex)

```java
static class Person {
private String name;
private int age;
private String job;

        Person(String name, int age, String job) {
            this.name = name;
            this.age = age;
            this.job = job;
        } 
    } // 이 클래스에 필드 인스턴스 변수 넣고 생성자 하나 생성함 => 객체 정의가 간편
===========결과물===========
Person A = new Person("James",26,"Constructor");
Person B = new Person("Lee",32,"Builder");
Person C = new Person("Wong",29,"gunner");
Person D = new Person("Queen",53,"Queen");
Person E = new Person("Loko",36,"officer");
// 메인 메소드로 불러와서 인스턴스 생성할때 이렇~게 간편해짐 ㅇㅋ? ㅇㅋ
// 객체 정의도 같이 되버린다는 말씀 << 사실 정확한지 모름 ㄷㄷ 추후 수정 요망
```

<br>

↔ new는 인스턴스 생성을 담당   
생성자는 인스턴스 변수 초기화에 사용되는 특수한 메서드   

```
클래스명(매개변수) { // 생성자 기본 구조
}
```

<br>

### 기본 생성자(Default Constructor)
- 모든 클래스에는 반드시 하나 이상의 생성자가 존재
- 인스턴스 만들 때 클래스안에 생성자가 없으면 자바 컴파일러가 기본 생성자를 자동으로 추가해서 생략됨

```
클래스명(){} //기본 생성자
```

### 매개변수가 있는 생성자
- 고유한 특성을 가진 인스턴스를 계속 만들어야하는 경우 인스턴스마다 각기 다른 값을 가지고 초기화할 수 있음   
~> 그러니까 main 메소드에서 출력해야할 때마다 일일히 변수 선언 안해주고 인스턴스 생성과 동시에 초기화할 수 있게 하는 것임   
ex)  Car c = new Car("Model X", "빨간색", 250); << 메인 메소드에서 호출 시

```
public class 클래스명 {
int x;
String y;

    클래스명(int a, String b) { // 매개변수 있는 생성자
        x = a;
        y = b;
    }
}
```

<br>

### this()
- 생성자의 상호 호출을 위함
- 자신이 속한 클래스에서 다른 생성자를 호출하는 경우에 사용

<br>

### this() 메소드 문법요소
1. this() 메소드는 반드시 생성자의 내부에서만 사용할 수 있음
2. this() 메소드는 반드시 생성자의 첫 줄에 위치해야 함

<br>

### this 키워드
- 인스턴스 변수와 매개변수를 이름만으로는 구분하기가 어려워지는 문제가 발생 -> 이를 구분해주기 위한 용도로 주로 사용되는 방법
- 즉, 인스턴스의 필드명과 지역변수를 구분하기 위한 용도
- 결국 this는 인스턴스 자신이라는 뜻, 우리가 참조변수를 통해 인스턴스의 멤버에 접근할 수 있는 것처럼 this를 통해서 인스턴스 자신의 변수에 접근할 수 있는 것   
=> 원래 모든 메서드에는 자신이 포함된 클래스의 객체를 가리키는 this라는 참조변수가 있음   
일반적으로는 컴파일러가 this.를 추가해주기 때문에 자주 생략함   
~> this 키워드는 객체 자신을 의미하는 참조변수이며, 이를 통해 객체 자신의 변수에 접근 가능함   
==> 필드 변수와 이어줄 특정 생성자의 로컬 변수를 같은 변수 이름으로 할때 쓰는듯    

```
static class Person { // 클래스
private String name; // 클래스 변수
private int age;
private String job;

        Person(String name, int age, String job) { // 생성자
            this.name = name; // 생성자 로컬 변수
            this.age = age;
            this.job = job;
        } // 생성자(constructor) 단축키 / alt + insert
    }
```

★쉽게 풀어서 말하면★   
this() 메소드는 같은 클래스 안에 메소드 1, 메소드 2가 있으면 메소드 끼리 다른 메소드를 호출할 때 사용하는 것이고   
this 키워드는 특정 클래스에서 선언된 클래스 변수를 해당 클래스의 메소드 안에 불러올 때 사용하는 키워드   

ex)

```
class Con {
private int x;
private int y;
private Con() {
System.out.println(1);
};
private Con(int x) {
this(); // 기본 생성자를 불러옴
System.out.println(2);
}
private Con(int a, int b) {
this.x = a; // 클래스 변수를 불러옴
this.y = b;
}
```

<br>

### 내부 클래스(Inner Class)
- 클래스 내에 선언된 클래스
- 외부 클래스와 내부 클래스가 서로 연관되어 있을 때 사용
- 코드의 복잡성을 줄임   
기본적으로 내부 클래스는 외부 클래스 내에 선언된다는 점을 제외하면 일반 클래스와 차이점이 없음   
단지 외부 클래스와 내부 클래스가 서로 연관되어 있을 때 사용의 편의성을 고려하여 만들어진 문법 요소   

```
public class outerClass {
    class inner {
        // 인스턴스 내부 클래스
    }
    static class staticClass {
        // 정적 내부 클래스
    }
    void run() {
        class localClass {
            // 지역 내부 클래스
        }
    }
} // 인스턴스 내부 클래스 + 정적 내부 클래스 = 멤버 내부 클래스
```

### 인스턴스 내부 클래스
- 객체 내부에 멤버의 형태로 존재
- 외부 클래스의 모든 접근 지정자의 멤버에 접근할 수 있음
- 메인 클래스는 외부 클래스의 메소드를 사용하고 외부 클래스는 내부 클래스의 메소드를 불러옴
- 이때 외부 클래스에서 선언된 변수를 내부 클래스 메소드의 값으로 입력하고 불러옴 >> 연관성   
★ 메인 클래스에 외부 클래스를 생성하고 호출함   
=> private을 이용해도 외내부가 서로를 참조함   

<br>

### 정적 내부 클래스
- 내부 클래스가 외부 클래스의 존재와 무관하게 정적 변수를 사용할 수 있게하는 방법
- 정적 내부 클래스는 인스턴스 내부 클래스와 동일하게 클래스의 멤버 변수 위치에 정의하지만, static 키워드를 사용한다는 점에서 차이가 있다   
★ 얘는 내부 클래스를 직접 생성해야함   

ex) <외부 클래스명>.<내부클래스명> = new <외부 클래스명>.<내부 클래스명>();   
=> 얘도 외내부가 참조하는 건 맞지만 외부클래스의 정적 변수만 참조할 수 있으며 메인 클래스에서 메소드를 불러올때도 위치를 정확히 특정 해줘야함

<br>

### 지역 내부 클래스
- 클래스의 멤버가 아닌 메소드 내에서 정의되는 클래스
- 지역 변수와 유사하게 메서드 내부에서만 사용가능
- 일반적으로 메서드 안에서 선언 후에 바로 객체를 생성해서 사용

![](/assets/img/bootcamp/innerClass_1.png)

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/222968240547)에서 옮겨짐
