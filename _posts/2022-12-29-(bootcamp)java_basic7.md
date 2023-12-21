---
title: "자바 : OOP : 상속과 캡슐화"
author: Jeremiah Lee
date: 2022-12-29
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

### 상속
- 기존의 클래스를 재활용하여 새로운 클래스를 작성하는 자바의 문법 요소
- 서로 상속 관계 있다고 하며, 하위 클래스는 상위 클래스가 가진 모든 멤버를 상속
- 즉, 하위 클래스의 멤버 개수는 언제나 상위 클래스의 그것과 비교했을 때 같거나 많음
- +"~클래스로부터 상속받았다"라는 표현보다는
- "~클래스로부터 확장되었다"는 표현이 그 역할과 기능을 생각했을 때 더 적절한 표현

사용 이유 : 코드를 재사용하여 보다 적은 양의 코드로 새로운 클래스를 작성할 수 있어 코드의 중복을 제거   
== 다형적 표현이 가능하다 == 하나의 객체가 여러 모양으로 표현될 수 있다는 것이 다형성   

! 자바는 다중 상속은 안되고 단일 상속(single inheritance)만을 허용   
~> 상위는 하나 하위는 다중 가능   

ex)

```java
public class Example {
  public static void main(String[] args) {
    Programmer p = new Programmer();
    p.name = "제임스";
    p.age = 32;
    p.companyName = "나이버";
    p.programmer();
    p.eat();
    p.sleep();
    p.shit();

    Banker b = new Banker();
    b.name = "란란루";
    b.age = 28;
    b.bankName = "우리은행";
    b.banker();
    b.eat();
    b.sleep();
    b.shit();
  }
}
class Person { // 상위 클래스
  String name;
  int age; // 필드, 인스턴스 변수

  public void eat () { // 메소드 1
    System.out.printf("%s 먹고\n",name);
  };
  public void sleep() { // 메소드 2
    System.out.printf("%s 자고\n",name);
  };
  public void shit() { // 메소드 3
    System.out.printf("%s 싼다\n",name);
  };
}
class Banker extends Person{ // 하위 클래스
  String bankName;

  public void banker() {
    System.out.printf("나는 %s이고, 나이는 %d다. 다니는 은행은 %s이다.\n",name,age,bankName);
  };
}
class Programmer extends Person{
  String companyName;

  public void programmer() {
    System.out.printf("나는 %s고, 나이는 %d다. 다니는 직장은 %s다.\n",name,age,companyName);
  }
}
```

↑ 생성자 포함 한거

<br>

### 포함 관계
- 포함(composite) : 클래스의 멤버로 다른 클래스 타입의 참조변수를 선언하는 것
- 클래스 간의 관계가 ‘~은 ~이다(IS-A)’ 관계인지 ~은 ~을 가지고 있다(HAS-A) 관계인지   
=> is 면 상속 / has 면 포함

ex)

```java
class Address {
String city;
String country; // 필드

    public Address(String city, String country) {
        this.city = city;
        this.country = country;
    } // 생성자
}
class Info {
int id;
String name;
Address address; // 필드

    public Info(int id, String name, Address address) {
        this.id = id;
        this.name = name;
        this.address = address;
    } // 생성자

    void showInfo() {
        System.out.println(id + " " + name);
        System.out.println(address.city+ " " + address.country);
//        System.out.println(Address.city+ " " + Address.country);
} // 메소드 ~> 마지막에 출력할 것
}
public class inherit_Composite {
public static void main(String[] args) {
Address address1 = new Address("서울", "한국");
Address address2 = new Address("도쿄", "일본");

        Info e = new Info(1, "김코딩", address1);
        Info e2 = new Info(2, "박해커", address2);
//        Info e = new Info();
//        e.id = 1;
//        e.name = "김코딩";
//        Info e2 = new Info();
//        e2.id = 2;
//        e2.name = "박해커";

        e.showInfo();
        e2.showInfo();
    }
}
// 아 상속 안쓰고 포함관계로 나타냈다는 뜻 같은데
// 포함관계는 상속 이용 안하고 ~> 왜냐면 Address 클래스가 Info 클래스를 상속하거나 하는게 아니라
// Info 클래스에 정의된 객체가 Address 를 가지고 있다의 형태라 포함되는 관계라고 하는가보네
// 그래서 Info 필드에 Address 라는 클래스 변수를 이용해서 Address 의 클래스 멤버를 불러왔고
// 그 클래스 멤버를 포함해서 Info 객체의 메소드에 정의했네
// 그러고서 메인 메소드에 생성할 때는 Address 클래스도 생성해주고나서 매개 변수 값 입력해주고
// Info 클래스도 마찬가지로 메소드 호출해야되니까 생성했고
// 근데 이거 생성자로 없으면 Address 그대로 주소 찍어주면 되나? 한번 해봄
```

<br>

### 메서드 오버라이딩(Method Overriding)
- 상위 클래스로부터 상속받은 메서드와 동일한 이름의 메서드를 재정의하는 것을 의미
- 세 가지 조건
1. 메서드의 선언부(메서드 이름, 매개변수, 반환타입)이 상위클래스의 그것과 완전히 일치해야한다.
2. 접근 제어자의 범위가 상위 클래스의 메서드보다 같거나 넓어야 한다.
3. 예외는 상위 클래스의 메서드보다 많이 선언할 수 없다.

<br>

### super 키워드와 super()
- super 키워드는 상위 클래스의 객체, super()는 상위 클래스의 생성자를 호출하는 것
- 공통적으로 모두 상위 클래스의 존재를 상정하며 상속 관계를 전제
- super 키워드를 사용하면 부모의 객체의 멤버 값을 참고
- 상위 클래스의 멤버와 자신의 멤버를 구별하는 데 사용된다는 점을 제외한다면 this와 super는 기본적으로 같은 것
- super()는 this()와 마찬가지로 생성자 안에서만 사용가능하고, 반드시 첫 줄에 와야 함

! 모든 생성자의 첫 줄에는 반드시 this() 또는 super()가 선언되어야 한다   
>> 만약 super()가 없는 경우에는 컴파일러가 생성자의 첫 줄에 자동으로 super()를 삽입   
>> 이때 상위클래스에 기본생성자가 없으면 에러가 발생   
>> 클래스를 만들 때는 자동으로 기본 생성자를 생성하는 것을 습관화할 것   

<br>

### Object 클래스
- 자바의 클래스 상속계층도에서 최상위에 위치한 상위클래스
- 자바의 모든 클래스는 Object 클래스로부터 확장된다
- 자바 컴파일러는 컴파일 과정에서 다른 클래스로부터 아무런 상속을 받지 않는 클래스에 자동적으로 extends Object를 추가하여 Object 클래스를 상속받도록 함

ex)

```
class ParentEx {  //  컴파일러가 "extends Object" 자동 추가

}

class ChildEx extends ParentEx {

}
// ~> Object 클래스는 자바 클래스의 상속계층도에 가장 위에 위치하기 때문에
// Object 클래스의 멤버들을 자동으로 상속받아 사용
```

<br>

### 캡슐화(Encapsulation)
- 특정 객체 안에 관련된 속성과 기능을 하나의 캡슐(capsule)로 만들어 데이터를 외부로부터 보호하는 것
- 목적
1. 데이터 보호의 목적
2. 내부적으로만 사용되는 데이터에 대한 불필요한 외부 노출을 방지
3. 정보 은닉(data hiding), 독립성을 확보, 오류의 범위를 최소화

<br>

### 패키지(package)
- 특정한 목적을 공유하는 클래스와 인터페이스의 묶음
- 클래스들을 그룹 단위로 묶어 효과적으로 관리하기 위한 목적   
ex) 폴더를 만들어 그 폴더와 관련된 파일들을 관리하는 것
- 자바에서 패키지 = 하나의 디렉토리(directory)이고, 하나의 패키지에 속한 클래스나 인터페이스 파일은 모두 해당 패키지에 속해있음
- 계층 구조 간 구분은 점(.) ~> 이거 주소 찍어줄때 쓰던 것 처럼   
~> String 클래스의 실제 이름은 java.lang.String => java.lang은 패키지명 => 여기서 String 클래스 불러온다는 뜻   
클래스의 충돌을 방지해주는 기능 = 같은 이름이더라도 다른 패키지 소속으로 명시해주면 충돌 X   

<br>

### Import문
- 다른 패키지 내의 클래스를 사용하기 위해 사용
- 일반적으로 패키지 구문과 클래스문 사이에 작성
- import문 없이 다른 패키지의 클래스를 사용하기 위해서는 매번 패키지명을 붙여 주어야 함 =>   
import문을 사용하면 사전에 컴파일러에게 소스파일에 사용된 클래스에 대한 정보를 제공 =>   
번거롭지 않음   

기본 문법   
mport 패키지명.클래스명; ~> 해당 패키지의 특정 클래스만 필요할때   
import 패키지명.*; ~> 해당 패키지의 클래스를 전부 쓰고싶을 때    

<br>

### 제어자(Modifier)
- 클래스, 필드, 메서드, 생성자 등에 부가적인 의미를 부여하는 키워드
- 크게 접근 제어자와 기타 제어자로 구분

접근 제어자 public, protected, (default), private   
기타 제어자 static, final, abstract, native, transient, synchronized 등   

- 제어자 자체는 하나의 대상에 여러 개를 쓸 수 있지만 접근 제어자는 단 한번만 사용할 수 있음

### 접근 제어자(Access Modifier)
1. 클래스 외부로의 불필요한 데이터 노출을 방지(data hiding)
2. 외부로부터 데이터가 임의로 변경되지 않도록 방어

```
접근 제어자     접근 제한 범위
private       동일 클래스에서만 접근 가능
default       동일 패키지 내에서만 접근 가능 // 변수명 앞에 제어자가 없을때의 기본 설정을 뜻함
protected     동일 패키지 + 다른 패키지의 하위 클래스에서 접근 가능
public        접근 제한 없음
```

~> 아래로 갈 수록 보안 느슨해짐

! protected 이거 자신 패키지에서 다른 패키지로 상속 관계로 연결 되었을때 해당 연결이 자신이 상위고 다른 패키지의 클래스가 하위일 때 참조 가능함

![](/assets/img/bootcamp/oneseeaccessmodifier.png)

<br>

### getter와 setter 메서드
- 목적 : 객체지향의 캡슐화의 목적을 달성하면서도 데이터의 변경이 필요한 경우   
ex) private 접근제어자가 포함되어 있는 객체의 변수의 데이터 값을 추가하거나 수정하고 싶을 때

setter 메서드 : 데이터 값을 변경 가능하게 해줌 / 메서드명에 set-을 붙여서 정의   
getter 메서드 : 설정한 변수 값을 읽어오는 데 사용 / 메서드명에 get-을 붙여서 정의   

! 정리하자면 private 접근 제어자를 가진 클래스 변수에 다른 클래스에서 접근 가능하게 하려고 set 이 달린 변수를 생성자 만들때 처럼 만들어주고 다른 클래스에서 생성한 다음 접근해서 값 넣어주고 get 이 달린 변수를 이용해서 다른 클래스로 그 값을 불러오는 것임   
즉, private 걸어논 변수 수정 가능케 해주는 친구임   
다른 데다가 막 못쓰게 private 걸어논거 특정한 파일에서는 써야할 때 쓰는 방법인듯   

이유 : getter 는 값을 읽기 전용으로 하는거 ~> 특정 데이터만 보여주고 싶을 때    
이유 : setter 는 값을 쓰기 전용으로 하는거 ~> 특정 데이터만 변경할 수 있게 만들 때

// 이거 캡슐화 호락호락한게 아니었음 걍 특성과 기능 별로 묶어서 따로 관리 하는 거 뜻하는건 줄 알았는데   
어? 맞나? ~> 1번 객체의 정의는 1번 객체에서만 쓰고, 2번 객체의 정의는 2번 객체에서만 쓰고   
만약에) 2번에 1번 내용이 있으면 1번 내용이 수정되어야 할 때 2번에서도 찾아서 수정해야되는 귀찮은 상황이 발생 -> 2번 내용은 2번에만 1번 내용은 1번에만 하고 호출만 하면 1번을 수정해야할 때 2번 수정은 안해도됨 -> 이게 캡슐화인거같음   

<br>

+ 생성자 추가 깨달음
얘 기본적으로 클래스에 포함 되어있고 생략되는거잖음? 그건 뭐 생략 ㅇㅋ 하고 넘어가고   
얘 용도가 해당 클래스의 인스턴스던 스태틱이던 필드 변수를 매개 변수로 다중 값 입력하려고 쓰는거 같음   

ex)

```java
public class inherit_Example_with_Constructor {
    public static void main(String[] args) {
//        Programmer1 p = new Programmer1("제임스",32);
        // 3. 근데 실제로 넣어보니까 오류 나네? 이거 상속 하위 클래스까지 적용 안되나봄
        // 4. 그럼 생성자를 하위 클래스에 넣어봄
//        p.name = "제임스";
//        p.age = 32;
//        p.companyName = "나이버";
        // 2. 위 부분을 생성자로 간단히
        Programmer1 p = new Programmer1("제임스",32,"나이버");
        // 5. 이제 잘되네 생성자는 하위 클래스에 넣어야하는구만
        // 6. 다음 부터는 alt + insert로 해보자
        p.programmer();
        p.eat();
        p.sleep();
        p.shit();

        Banker1 b = new Banker1("란란루",28,"우리은행");
//        b.name = "란란루";
//        b.age = 28;
//        b.bankName = "우리은행";
        // 2. 위 부분을 생성자로 간단히
        b.banker();
        b.eat();
        b.sleep();
        b.shit();
    }
}
class Person1 { // 상위 클래스
    String name;
    int age; // 필드, 인스턴스 변수
    
//    public Person1(String name, int age) {
//        this.name = name;
//        this.age = age;
//    } 1. 생성자 생성했음 그럼? 이제 위 코드 3줄 빼고 생성과 동시에 매개 변수로 입력해주면?
//          코드가 짧아짐 <~ 이거다 느낌 딱옴 와 ㅋㅋ

    public void eat () { // 메소드 1
        System.out.printf("%s 먹고\n",name);
    };
    public void sleep() { // 메소드 2
        System.out.printf("%s 자고\n",name);
    };
    public void shit() { // 메소드 3
        System.out.printf("%s 싼다\n",name);
    };
}
class Banker1 extends Person{ // 하위 클래스
    String bankName;

    public Banker1(String name, int age, String bankName) {
        this.name = name;
        this.age = age;
        this.bankName = bankName;
    }

    public void banker() {
        System.out.printf("나는 %s이고, 나이는 %d다. 다니는 은행은 %s이다.\n",name,age,bankName);
    };
}
class Programmer1 extends Person{
    String companyName;

    public Programmer1(String name, int age, String companyName) {
        this.name = name;
        this.age = age;
        this.companyName = companyName;
    }

    public void programmer() {
        System.out.printf("나는 %s고, 나이는 %d다. 다니는 직장은 %s다.\n",name,age,companyName);
    }
}
```

원래는메인 메소드에 클래스 생성하고서 해당 클래스의 필드 멤버를 호출해서 따로 값 입력해야되는데   
생성자로 생성 해놓고 클래스 생성하면 매개 변수로 입력해서 코드 더 짧아짐 

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/222969413295)에서 옮겨짐
