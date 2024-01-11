---
title: "자바 : OOP : 다형성과 추상화"
author: Jeremiah Lee
date: 2022-12-30
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

+ 상속은 캡슐화를 위반할 수 있으며 이는 하위 클래스가 상위 클래스의 정보를 너무 많이 담고 있어 수정하기 어렵다는 뜻이다 ~> 결합도가 높다
  그래서 보통 상속은 단일 상속 밖에 안되고 캡슐화를 위반할 수 있기 때문에 자주 쓰진 않는다

### +캡슐화
~> 객체의 내부 동작과 성격을 외부로 부터 보호하는 것   
~> 그니까 A 클래스에서 해야할 일을 A에만 담고 B 클래스가 할은 B에만   
~> 그러고서 접근 제어자를 통해 보호함   
~> B에서 A의 기능이 필요하면 getter로 읽기만 가능하게 해서 특정 기능을 불러옴 (수정할게 있다면 setter)   
~> 이러면 A에서 변경 사항이 있어도 A만 바꿔주면 B는 기존 로직 그대로 이용가능   

의존성 주입 -> Dependency Injection (DI)   
- 객체 지향 핵심
- 추상화랑 다형성이 필수

<br>

### 다형성
- 하나의 객체가 여러 가지 형태를 가질 수 있는 성질
- 한 타입의 참조변수를 통해 여러 타입의 객체를 참조할 수 있도록 만든 것
- 상위 클래스 타입의 참조변수를 통해서 하위 클래스의 객체를 참조할 수 있도록 허용한 것
간단하게 하위 클래스(타입) 자신으로 인스턴스 생성? O ~> 원래하던 방식   
상위클래스(타입)를 참조해서 하위클래스 인스턴스 생성? O ~> 상속관계라 어차피 참조할 멤버랑 자기랑 가지고있는 멤버가 무조건 같기 때문   
하위클래스(타입)를 참조해서 상위클래스 인스턴스 생성? X ~> 위와 같은 명제가 다를 수 있기 때문   

### 참조변수의 타입 변환
- 사용할 수 있는 멤버의 개수를 조절하는 것을 의미
- 조건
  1. 서로 상속관계에 있는 상위 클래스 - 하위 클래스 사이에만 타입 변환이 가능
  2. 하위 클래스 타입에서 상위 클래스 타입으로의 타입 변환(업캐스팅)은 형변환 연산자(괄호)를 생략할 수 있어야함
  3. 반대로 상위 클래스에서 하위 클래스 타입으로 변환(다운캐스팅)은 형변환 연산자(괄호)를 반드시 명시

참조변수의 타입변환은 서로 상속 관계에 있는 관계에서는 양방향으로 자유롭게 수행될 수 있으나,   
상위 클래스로의 타입 변환이냐(괄호 생략 가능) 아니면 하위 클래스로의 타입 변환이냐(괄호 생략 불가)에 따라서 약간의 차이

! Casting : 타입변환

### instanceof 연산자
- 참조변수의 캐스팅이 가능한 지 여부를 boolean 타입으로 확인
- 이유 : 프로젝트의 규모가 커지면 객체를 어떤 생성자로 만들었는가’와 ‘클래스 사이에 상속관계가 존재하는가’를 판단하기 어려워짐, 즉, 캐스팅 되냐 안되냐를 코드 규모가 커지면 확인하기 힘들다는 것

! null 인 경우도 false로 나옴   
문법 : 참조_변수 instanceof 타입

<br>

### 추상화
- “사물이나 표상을 어떤 성질, 공통성, 본질에 착안하여 그것을 추출하여 파악하는 것"
- 핵심적인 개념은 공통성과 본질을 모아 추출하는 것
- 기존 클래스들의 공통적인 요소들을 뽑아서 상위 클래스를 만들어 내는 것
- 상위 타입의 인스턴스에 하위 타입의 인스턴스를 할당 할 수 있는 것   
ex) Person person = new Man1(); == Person 상위 / Man1 하위 일때 이거 오류 안난다는 뜻
- 같은 타입의 참조변수에서 여러개 결과가 나올 수 있다.
- 같은 타입인데도 다른 출력 결과가 나온다는것 -> 다형성과 관계 잇음
- 각 객체의 공통 정보를 한 객체에 모으기 위해 추출 -> 추상화 했다고함   
  == 공통점을 추출했다

### 추상 메소드
추상 클래스에는 추상 메소드를 정의할 수 있다. (없을 수도 있다)=> 메소드 바디가 없고 시그니처만 정의함   
ex) public abstract void A();   
~> 상속 받는 하위 클래스들은 A라는 메소드를 반드시 구현해야할 의무가 생김   
-> 즉 하위 클래스들은 A라는 메소드를 끌고와 메소드 오버라이딩하는 것이 강제된다 // 이때 구현 & 오버라이딩 했다 둘다 맞음   
! alt + insert 쓰면 쉽고 간편하게 오버라이딩 가능   

! 어떤 클래스가 상속을 해주는 역할만 하면 추상 클래스   
추상 클래스에는 추상 메소드를 정의할 수 있는데     
그걸 정의하면 모든 하위 클래스에서 해당 메소드를 반드시 강제로 구현(오버라이드)을 해야한다     

### abstract 제어자
- 자바의 맥락에서 abstract라는 단어가 내포하는 의미는 ‘미완성'
- abstract는 주로 클래스와 메서드를 형용하는 키워드로 사용
- 메서드 앞에 붙은 경우를 ‘추상 메서드(abstract method)’, 클래스 앞에 붙은 경우를 ‘추상 클래스(abstract class)’
- 어떤 클래스에 추상 메서드가 포함되어있는 경우 해당 클래스는 자동으로 추상 클래스가 됨
- 추상 메서드 = 시그니처만 있고 바디가 없는 메서드를 의미
- 객체 생성 불가

ex)
```java
abstract class AbstractExample { // 추상 메서드가 최소 하나 이상 포함돼있는 추상 클래스   
    abstract void start(); // 메서드 바디가 없는 추상메서드   
}   
```

이유
1. 상속 관계에 있어 새로운 클래스를 작성하는데 매우 유용
2. 메서드의 내용이 상속을 받는 클래스에 따라서 종종 달라지기 때문에
3. 상위 클래스에서는 선언부만을 작성하고 실제 구체적인 내용은 상속을 받는 하위 클래스에서 구현하도록하면 상황이 변하더라도 보다 유연하게 대응 가능

### 추상 클래스 & 추상 메소드
상위 클래스 멤버는 하위 클래스들이 공통적으로 갖고있음( = 하위 클래스에 무조건적으로 들어가야하는 멤버이기 때문에 강제됨)   
~> 근데 공통적으로 갖고는 있는데 출력되는 값을 전부 다르게 하고 싶음   
~> 어떻게 해야? 상위 클래스를 추상 클래스로 만들고 추상 메소드 작성해서 해당 메소드의 정의는 하위 클래스들에게 맡김   
~> 그럼? 하위 클래스들은 오버라이딩을 통해 추상 메소드를 정의, 즉 구현해내야함   

ex) 여러 사람이 함께 개발하는 경우, 공통된 속성과 기능임에도 불구하고   
각각 다른 변수와 메서드로 정의되는 경우 발생할 수 있는 오류를 미연에 방지     

고로, 추상화는 상속계층도의 상층부에 위치할 수록 추상화의 정도가 높고 그 아래로 내려갈수록 구체화된다

### final 키워드
더이상 변경이 불가하거나 확장되지 않는 성질 붙임   

```
위치            의미
클래스          변경 또는 확장 불가능한 클래스, 상속 불가
메서드          오버라이딩 불가
변수            값 변경이 불가한 상수
```


### 인터페이스
- “-간/사이"를 뜻하는 inter와 “얼굴/면"을 의미하는 face의 결합으로 구성된 단어, 두 개의 다른 대상 사이를 연결한다는 의미
- 기본적으로 인터페이스도 추상 클래스처럼 자바에서 추상화를 구현하는 데 활용된다는 점에서 동일
- 추상클래스에 비해 더 높은 추상성을 가진다는 점에서 큰 차이
- 추상 클래스 = “미완성 설계도"
- 인터페이스 = “밑그림"
- 기본적으로 추상 메서드와 상수만을 멤버로 가질 수 있다 = 추상 클래스에 비해 추상화 정도가 더 높다
- “추상 메서드의 집합"

### 인터페이스의 기본 구조
- 추상 클래스에서 정의할 수 있는 추상 메소드를 똑같이 인터페이스에서도 정의 할 수 있다.
- 추상 클래스와의 차이점 = 추상 클래스는 일반 메소드를 정의할 수는 있지만 인터페이스는 추상 메소드만 정의할 수 있다. / 인터페이스는 클래스 단일 상속과 다르게 다중 구현이 가능함
- 관례로 -able을 많이 붙임
- 기본적으로 클래스를 작성하는 것과 유사 but class 키워드 대신 interface 키워드를 사용
- 내부의 모든 필드가 public static final로 정의
- static과 default 메서드 이외의 모든 메서드가 public abstract로 정의   
  단, 모든 인터페이스의 필드와 메서드에는 위의 요소가 내포되어있기 때문에 명시하지 않아도 생략이 가능   
> 컴파일러가 자동으로 추가해줌

### 인터페이스의 구현
인스턴스를 생성할 수 없고, 메서드 바디를 정의하는 클래스를 따로 작성해야함   
> 이 과정은 앞서 배운 extends 키워드를 사용하는 클래스의 상속과 기본적으로 동일 but “구현하다"라는 의미를 가진 implements 키워드를 사용함   
> 이 과정으로 인터페이스를 구현한 클래스는 해당 인터페이스에 정의된 모든 추상메서드를 구현해야함   
> 즉, 어떤 클래스가 특정 인터페이스를 구현한다는 것 = 그 클래스에게 인터페이스의 추상 메서드를 반드시 구현하도록 강제하는 것   
== 인터페이스가 가진 모든 추상 메서드들을 해당 클래스 내에서 오버라이딩하여 바디를 완성한다   

### 인터페이스의 다중 구현
상속은 다중 상속 허용 안되는데, 인터페잇는 다중적 구현 가능   
다시 말해, 하나의 클래스가 여러 개의 인터페이스를 구현할 수 있음   
>> 이게 상속 많이 안쓰고 인터페이스 많이 쓰는 이유인듯   
단, 인터페이스는 인터페이스로부터만 상속이 가능하고, 클래스와 달리 Object 클래스와 같은 최고 조상이 존재하지 않음   

ex)

```java
class ExampleClass implements ExampleInterface1, ExampleInterface2, ExampleInterface3 {
... 생략 ...
}
```

! 클래스 다중 상속 불가 이유   
= 만약 상위 클래스에 동일한 이름의 필드 또는 메서드가 존재하는 경우 충돌이 발생하기 때문   
! 인터페이스는 왜 가능?   
= 애초에 미완성된 멤버를 가지고 있기 때문에 충돌이 발생할 여지가 없고, 따라서 안전하게 다중 구현이 가능   

! “의존한다" = A 클래스에서 B 클래스에 정의된 특정 속성 또는 기능을 가져와 사용하고 있다 = A가 B에 의존한다.   

### 인터페이스의 장점
- 역할과 구현을 분리시켜 사용자 입장에서는 복잡한 구현의 내용 또는 변경과 상관없이 해당 기능을 사용할 수 있다
- 코드 변경의 번거로움을 최소화하고 손쉽게 해당 기능을 사용할 수 있도록 함

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/222970349371)에서 옮겨짐