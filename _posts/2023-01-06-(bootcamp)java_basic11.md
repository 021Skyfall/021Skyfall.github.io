---
title: "자바 : 애너테이션과 람다, 그리고 스트림"
author: Jeremiah Lee
date: 2023-01-06
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

### 애너테이션(annotation)   
- 정보 전달을 위한 목적
- 애너테이션은 다른 프로그램에게 정보를 전달
- 소스 코드가 컴파일되거나 실행될 때에 컴파일러 및 다른 프로그램에게 필요한 정보를 전달해주는 문법 요소

ex) @Override    
애너테이션은 @로 시작하며, 클래스, 인터페이스, 필드, 메서드 등에 붙여서 사용    
~> 컴파일러에게 알려주는 역할     

### JDK에서 기본적으로 제공하는 애너테이션   
표준 애너테이션 : JDK에 내장되어 있는 일반적인 애너테이션   
~> @Override와 같이 다른 문법 요소에 붙여서 사용하는 일반적인 애너테이션을 의미   
메타 애너테이션 : 다른 애너테이션을 정의할 때 사용하는 애너테이션   
~> 애너테이션을 직접 정의해서 사용할 때에 사용하는 애너테이션   

! 붙이는 이유   
#### @Override를 예로   
상속 관계 일때 개발자의 실수로 오버라이드 하려는 메소드 명이 잘못 정의되면   
컴파일 할때 새로운 메소드를 정의한 것으로 간주하고 컴파일 에러가 발생하지 않음   
~> 이런 실수가 있을 경우 @Override가 붙어 있으면 이 메소드는 상위 객체의 오버라이드니까 문제 있으면 알려달라는 것   
== 추상 메소드 구현도 마찬가지임   

### 표준 애너테이션   

ex)

```java
public class Practice_Override {
  public static void main(String[] args) {
    Example ex = new power1();
    ex.exam();
  }
}
interface Example{
  void exam();
}
class power1 implements Example{
  @Override // << 이게 애너테이션 / @로 시작하며 클래스, 인터페이스, 필드, 메서드 등에 붙여서 사용
  // 이거 추상 메소드 구현 오버라이드 입니다~하고 컴파일러에게 알려주는 역할
  public void exam() {
    System.out.println("애너테이션 연습");
  }
```

#### @Deprecated
기존에 사용하던 기술이 다른 기술로 대체되어 기존 기술을 적용한 코드를 더 이상 사용하지 않도록 유도하는 경우에 사용    
기존의 코드를 다른 코드와의 호환성 문제로 삭제하기 곤란해 남겨두어야만 하지만 더 이상 사용하는 것을 권장하지 않을 때에 @Deprecated를 사용    

ex)

```java
public class Practice_Deprecated {
  public static void main(String[] args) {
    OldClass oldClass = new OldClass();
    System.out.println(oldClass.getOldField()); // << 밑줄로 경고해줌
  }
}
class OldClass {
  @Deprecated // 삭제하기 곤란해 남겨야하지만 쓰는 거 권장하지 않음의 뜻
  private int oldField;

  @Deprecated
  public int getOldField() {
    return oldField;
  }
}
```

#### @SuppressWarnings   
컴파일 경고 메시지가 나타나지 않도록 함   
경고가 발생할 것이 충분히 예상됨에도 묵인해야 할 때 주로 사용   

![](/assets/img/bootcamp/SuppressWarning_annotation.png)

<br>

@SuppressWarnings 뒤에 괄호를 붙이고 그 안에 억제하고자 하는 경고메세지를 지정해줄 수 있음   
ex) @SuppressWarnings({"deprecation", "unused", "null"})   

ex)

```java
class Anno {
  @SuppressWarnings("all")
  public void aa1() {
    if(0==0) { // << always true 라는 경고가 출력 안됨
      System.out.println("무조건 true임");
    } else System.out.println("이건 무조건 안나옴");
  }
}
```

### @FunctionalInterface
함수형 인터페이스를 선언할 때, 컴파일러가 함수형 인터페이스의 선언이 바르게 선언되었는지 확인하도록   
함수형 인터페이스는 단 하나의 추상 메서드만을 가져야하는 제약   


### 메타 애너테이션   
애너테이션을 정의하는 데에 사용되는 애너테이션   
애너테이션의 적용 대상 및 유지 기간을 지정하는 데에 사용   

### @Target   
애너테이션을 적용할 “대상"을 지정하는 데 사용   

![](/assets/img/bootcamp/Taget_target.png)

<br>

ex)

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

public class Practice_Target {
}
@Target({ElementType.FIELD,ElementType.TYPE,ElementType.TYPE_USE}) // 얘네를 담고 있는
@interface Custom{} // 커스텀 애노테이션 생성
@Custom // Type 대상에 적용
class main {
  @Custom // FIELD 대상에 적용
  int i;
}
```

### @Documented   
javadoc으로 작성한 문서에 포함되도록 하는 애너테이션 설정   

### @Inherited   
하위 클래스가 애너테이션을 상속받도록   
상위 클래스에 붙이면, 하위 클래스도 상위 클래스에 붙은 애너테이션들이 동일하게 적용   

### @Retention   
특정 애너테이션의 지속 시간을 결정하는 데 사용   

![](/assets/img/bootcamp/kind_of_retention_policy.png)

<br>

### @Repeatable   
애너테이션을 여러 번 붙일 수 있도록 허용한다는 의미


### 참고 : 사용자 정의 애너테이션

ex)

```java
@interface 애너테이션명 { // 인터페이스 앞에 @기호만 붙이면 애너테이션을 정의할 수 있습니다.
타입 요소명(); // 애너테이션 요소를 선언
}
```

~> java.lang.annotation 인터페이스를 상속받기 때문에 다른 클래스나 인터페이스를 상속 받을 수 없다

<br>
<br>

### 람다식(Lambda Expression)
- 함수형 프로그래밍 기법을 지원하는 자바의 문법요소
- 메서드를 하나의 ‘식(expression)’으로 표현한 것
- 코드를 매우 간결하면서 명확하게 표현할 수 있다는 큰 장점

- 익명 자식 객체 (= 상속)   
=> 상위 클래스를 상속 받아서 생성   
=> 상위 타입의 참조변수에 할당 가능   
=> 익명 하위 객체를 만들 때 , 중괄호 블록 내에는 보통 상위 클래스의 메소드를 재정의하는 코드 작성   
익명 구현 객체 (= 인터페이스)   
=> 인터페이스를 구현해서 생성   
=> 인터페이스 타입의 참조 변수에 할당 가능   
=> 익명 구현 객체를 만들 때, 중괄호 블록 내에는 보통 인터페이스의 추상 메소드를 구현하는 코드 작성   

! 람다식은 인터페이스에서만 사용 가능

<br>

### 람다식의 기본 문법

ex)

```java
//기존 메서드 표현 방식
void sayhello() {
  System.out.println("HELLO!")
}

//위의 코드를 람다식으로 표현한 식
() -> System.out.println("HELLO!")

~> 반환타입과 이름을 생략
따라서 람다함수를 종종 이름이 없는 함수, 즉 익명 함수(anonymous function)라 부르기도 함


변환 ex)1
int sum(int num1, int num2) {
  return num1 + num2;
}

// ↓

(int num1, int num2) -> { // 반환타입과 메서드명 제거 + 화살표 추가
  return num1 + num2;
}
```

ex)2

```java
// 기존 방식
void example1() {
  System.out.println(5);
}

// ↓

() -> {System.out.println(5);}

ex)3
// 기존 방식
int example2() {
  return 10;
}

// ↓

() -> {return 10;}

ex)4
// 기존 방식
void example3(String str) {
  System.out.println(str);
}

// ↓

(String str) -> {System.out.println(str)};
```

! 특정 조건이 충족되면 람다식을 더욱 축약하여 표현   
실행문이 하나만 존재할 경우 중괄호, ;, return문 생략 가능   
매개변수의 타입을 함수형 인터페이스를 통해 유추할 수 있는 경우에 매개변수의 타입까지 생략 가능   

ex)

```java
// 기존 방식
int sum(int num1, int num2) {
  return num1 + num2;
}

// ↓

(num1, num2) -> num1 + num2
```

<br>

### 함수형 인터페이스
- 람다식 = 객체 ~> 이름이 없기 때문에 일회용 익명 객체
- 익명 객체는 익명 클래스를 통해 만들 수 있음
- 익명 클래스 = 객체의 선언과 생성을 동시에 하여 오직 하나의 객체를 생성하고, 단 한번만 사용되는 일회용 클래스   
~> 여기서 람다식이 객체면 이 객체에 접근하고 사용하기 위한 참조변수가 필요함

ex)

```java
new Object() {
  int sum(int num1, int num2) {
    return num1 + num2;
  }
};
// 여기서는 Object 클래스에 sum 이라는 메소드가 없어서 
// Object 타입의 참조변수에 할당해도 sum 메소드를 쓸 수 없음
```

↓   
이걸 메인 메소드에 인스턴스화 해 출력해도   
java: cannot find symbol   
symbol: method sum(int,int)   
location: variable o of type java.lang.Object   
이런 결과가 나옴   
↓   
해결하기 위한 방법이 함수형 인터페이스(Functional Interface)임   
즉, 함수형 인터페이스는 기존의 인터페이스 문법을 활용하여 람다식을 다루는 것   

단, 함수형 인터페이스에는 단 하나의 추상 메소드만 선언할 수 있음   
람다식과 1:1로 매칭되어야 하기 때문   

<br>

### 메서드 레퍼런스
- 불필요한 매개변수를 제거할 때 주로 사용
- 즉, 객체를 더욱더 간단하게 사용
- 입력값과 출력값의 반환타입을 쉽게 유추할 수 있기 때문에 입력값과 출력값을 일일이 적어주는 게 크게 중요하지 않음

ex)

```java
(left, right) -> Math.max(left, right)
// 이걸 더 간편하게 만들면 ↓
Math :: max
// Math 클래스의 max() 메소드를 참조한다는 뜻임
```

내 경험 ex)

```java
Functional functional = (num1, num2) -> num1 + num2;
// 참고로 이거 num 에 int 타입 들어갈거고 래퍼 클래스인 Integer 에
// 더하기 메소드가 있음 그래서 변형 가능
// 그래서 Functional functional = Integer::sum; 로 변형 가능
// Intger 클래스의 sum() 메소드를 참조한다는 것
```

! 메소드 참조의 원리도 람다식과 똑같이 인터페이스의 익명 구현 객체로 생성되는 것으로   
인터페이스의 추상 메서드가 어떤 매개 변수를 가지고, 리턴 타입이 무엇인가에 따라 달라짐   


### 정적 메소드와 인스턴스 메소드 참조   
정적 메소드 ~> 클래스 :: 메소드   
인스턴스 메소드 ~> 참조변수 :: 메소드   

ex)
```java
public class Method_reference {
  public static void main(String[] args) {
    A a;
    a = Integer :: sum;
    System.out.println(a.ac(1,2));
    a = Integer :: max;
    System.out.println(a.ac(5,1));
    a = Integer :: min;
    System.out.println(a.ac(5,1));
    // 기존 함수형 인터페이스 참조 메소드

    a = Plus :: s; // 클래스명 :: 메소드명
    System.out.println(a.ac(7,1));
    // 정적 메소드 참조

    Plus plus = new Plus();
    a = plus :: i; // 인스턴스명 :: 메소드명
    System.out.println(a.ac(7,1));
    // 인스턴스 메소드 참조
  }
}
interface A { // 함수형 인터페이스
  int ac(int x, int y);
}
class Plus {
  public static int s(int x, int y){ // 정적 메소드
    return x + y;
  }
  public int i(int x, int y) { // 인스턴스 메소드
    return x * y;
  }
}
```

### 생성자 참조
- 생성자를 참조한다는 것은 객체 생성을 의미
- 객체를 생성하고 리턴하도록 구성된 람다식은 생성자 참조로 대치 가능
- 클래스 이름 뒤에 :: 기호를 붙이고 new 연산자를 기술

ex)

```java
//기존 람다식
(a,b) -> new 클래스(a,b)

//생성자 메소드 참조
클래스 :: new
```

생성자가 오버로딩 되어 여러 개가 있을 경우   
컴파일러는 함수형 인터페이스의 추상 메서드와 동일한 매개 변수 타입과 개수를 가지고 있는 생성자를 찾아 실행   

ex)

```java
public class Constructor_reference {
  public static void main(String[] args) {
    Function<String,Person> function = Person::new;
    Person person = function.apply("");
    BiFunction<String,String,Person> biFunction = Person::new;
    Person person1 = biFunction.apply("","");

  }
}
class Person {
  private String name;
  private String id;

  public Person(String name) {
    this.name = name;
    System.out.println("이름만");
  }

  public Person(String name, String id) {
    this.name = name;
    this.id = id;
    System.out.println("이름 아이디");
  }

  public String getName() {
    return name;
  }

  public String getId() {
    return id;
  }
}
```

!   
이거 근데 잘 이해 안감   
Function 이랑 BiFunction 들어가는 이유랑   
왜 써야하는지?   
람다식이 맞는지? 맞다면 인터페이스는 어따 팔아먹었는지?   
이건 그냥 메소드인거고   

! 람다식은 인터페이스에서만 사용 가능   

<br>
<br>

### 스트림(Stream)
- 배열, 컬렉션의 저장 요소를 하나씩 참조해서 람다식으로 처리할 수 있도록 해주는 반복자
- List, Set, Map, 배열 등 다양한 데이터 소스로부터 스트림을 만들 수 있고, 이를 표준화된 방법으로 다룰 수 있음
- 다량의 데이터에 복잡한 연산을 수행하면서도, 가독성과 재사용성이 높은 코드를 작성할 수 있음


### 스트림의 핵심 개념과 특징
- 자바8부터 도입된 문법
- 배열 및 컬렉션의 저장 요소를 하나씩 참조해서 람다식으로 처리할 수 있도록 하는 반복자
- 자바에서의 스트림은 “데이터의 흐름”을 의미   
~> 각 데이터를 흐름에 따라 우리가 원하는 결과로 가공하고 처리하는 일련의 과정


### 스트림(Stream)의 도입 배경
기존에는 >   
배열과 컬렉션   
~> 저장된 데이터들에 반복적으로 접근 & 가공   
~> for문 과 Iterator 을 활용   
근데 이게 문제가 코드가 길고 복잡해질 수 있음   

ex)

```java
public class Basic {
public static void main(String[] args) {

        List<Integer> list = Arrays.asList(1,2,3,4,5);

        // 기존의 Iterator 방식 순회
        Iterator<Integer> it = list.iterator();
        while (it.hasNext()) {
            System.out.print(it.next());
        }

        System.out.println();

        // 스트림 사용 순회
        Stream<Integer> stream = list.stream();
        stream.forEach(System.out::print);
    }
}
```

<br>

### 명령형 프로그래밍(Imperative Programming)
- “어떻게” 코드를 작성할 지
- 기존에 하던 방식

### 선언형 프로그래밍(Declarative Programming)
- “무엇”에 집중하여 코드를 작성할 지 // 어떻게는 "추상화"됨
- 스트림 도입 방식
- 데이터 소스가 무엇이냐에 관계없이 같은 방식으로 데이터를 가공/처리
- 열이냐 컬렉션이냐에 관계없이 하나의 통합된 방식으로 데이터를 다룰 수 있게 되었음

ex)

```java
public class Declarative_Imperative {
  public static void main(String[] args) {
    List<Integer> list = List.of(1,3,5,7,9,11);
    int sum = 0;

    // 명령형 프로그래밍
    for (int x : list) {
      if (x > 4){
        sum+=x;
      }
    }
    System.out.println("명령형 프로그래밍 : "+sum);
    // 기존에 하던대로 for-each 문으로 순회 후 합계 계산

    // 선언형 프로그래밍
    int sum1 =
      list.stream()
        .filter(x -> x > 4)
        .mapToInt(x -> x)
        .sum();
    System.out.println("선언형 프로그래밍 : "+sum1);
    // stream 을 이용한 합계 계산
    // 전부 메소드 + 람다식 이라 이해하기 훨씬 직관적이긴 함
    // 아직 코드를 짧게 넣어서 간결한지는 몰?루겠음

    List<String> fruitList = List.of("바나나 ", "사과 ", "오렌지");
    String[] fruits = {"오렌지 ", "사과 ", "바나나"};
    // 리스트랑 배열 생성
    Stream<String> str = fruitList.stream();
    // 리스트 스트림 적용
    Stream<String> str2 = Arrays.stream(fruits);
    // 배열 스트림 적용
    str.forEach(System.out::print); // 리스트 순회
    System.out.println();
    str2.forEach(System.out::print); // 배열 순회
  }
}
```

<br>

### 스트림의 특징
1. 스트림 처리 과정은 생성, 중간 연산, 최종 연산 세 단계의 파이프라인으로 구성
2. 스트림은 원본 데이터 소스를 변경하지 않는다(read-only)
3. 스트림은 일회용이다(onetime-only)
4. 스트림은 내부 반복자

##### 1.

![](/assets/img/bootcamp/stream_pipeline.png)

<br>

### 스트림 파이프라인 3단계

1) 스트림의 생성   
   배열, 컬렉션, 임의의 수 등 다양한 데이터 소스를 일원화하여 스트림으로 작업하기 위해서 스트림을 생성    

2) 중간 연산   
   필터링, 매핑, 정렬 등의 작업이 포함    
   중간 연산의 결과는 또 다른 스트림이기 때문에 계속 연결해서 연산을 수행할 수 있음    

3) 최종 연산   
   중간 연산이 완료된 스트림을 최종적으로 처리하는 최종 연산(총합, 평균, 카운팅 등)을 끝으로 스트림은 닫히고 모든 데이터 처리가 완료    
   최종적으로 단 한번의 연산만 가능 ~> 다시 데이터를 처리하고 싶다면, 다시 스트림을 생성해야함    


##### 2.    
원본이 되는 데이터 소스의 데이터들을 변경하지 않음    
오직 데이터를 읽어올 수 있고, 데이터에 대한 변경과 처리는 생성된 스트림 안에서면 수행    
원본 데이터가 스트림에 의해 임의로 변경되거나 데이터가 손상되는 일을 방지하기 위함    


##### 3.    
스트림이 생성되고 여러 중간 연산을 거쳐 마지막 최종 연산이 수행되고 난 후에는 스트림은 닫히고 다시 사용 불가능    


##### 4.    
외부 반복자(External Iterator) = 개발자가 코드로 직접 컬렉션의 요소를 반복해서 가져오는 코드 패턴을 의미    
ex) 인덱스를 사용하는 for문 , Iterator 를 사용하는 while문    

내부 반복자(Internal Iterator) = 외부 반복자 반대 개념    
컬렉션 내부에 데이터 요소 처리 방법(람다식)을 주입시켜서 요소를 반복처리하는 방식    

<br>

### 스트림의 생성
- 스트림으로 데이터를 처리하기 위해서는 가장 먼저 스트림을 생성
- 가장 많이 쓰이는 배열, 컬렉션, 그리고 임의의 수로 스트림을 생성하는 방법

### 배열 스트림 생성

ex)

```java
public class Create_ArrayStream {
public static void main(String[] args) {
//배열 스트림 생성

        int[] arr = new int[]{1, 2, 3, 4, 5};
        IntStream intStream = Arrays.stream(arr);
        // int 배열을 int 형 스트림으로 생성 후 intStream 에 할당
        intStream.forEach(System.out::print);

        System.out.println();

        double[] arr2 = new double[]{1.1, 1.2, 1.3, 1.4, 1.5};
        // double 배열을 double 형 스트림으로 생성
        DoubleStream doubleStream = Arrays.stream(arr2);
        // double 배열을 double 형 스트림으로 생성
        doubleStream.forEach(System.out::print);

        System.out.println();

        String[] arr1 = new String[]{"Hello", " Java", " World", " !"};
        // Stream<String> stream = Arrays.stream(arr1);
        // String 배열을 String 형 스트림으로 생성
        Stream<String> stream = Stream.of(arr1);
        // 문자열은 스트림 클래스에서 불러온 메소드인 of() 도 가능
        stream.forEach(System.out::print);

        System.out.println();

        char[] arr3 = new char[]{'a','b','c','d','e'};
        Stream<Character> stream1 = new String(arr3)
                .chars().mapToObj(x -> (char) x);
        // Character 는 특이하게 배열을 문자열로 생성한 다음에
        // 다시 String 클래스의 메소드 들로 나누고
        // 해당 클래스의 stream 객체의 메소드인 mapToObj 를 사용해서
        // 다시 다운캐스팅 해 줘야함
        stream1.forEach(System.out::print);

    }
}
```

<br>

### 컬렉션 스트림 생성
- 최상위 클래스인 Collection 에 정의된 stream() 메서드를 사용하여 스트림을 생성할 수 있음
- 따라서 Collection 으로부터 확장된 하위클래스 List 와 Set 을 구현한 컬렉션 클래스들은 모두 stream() 메서드를 사용하여 스트림을 생성할 수 있음

ex)

```java
public class Create_CollectionStream {
  public static void main(String[] args) {
    List<Integer> list = Arrays.asList(1,2,3,4,5);
    Stream<Integer> stream = list.stream();
    // List 클래스를 참조하고 있는 변수 list 에서 곧바로 stream() 메소드를 호출 가능함
    // 이미 Collection 클래스에 stream() 메소드가 정의되어있기 때문
    // 그래서 생성이 간편함
    stream.forEach(System.out::print);

    System.out.println();
    // 무한 스트림
//        IntStream ints = new Random().ints();
//        ints.forEach(System.out::println);
    // 난수 무한 생성
    // 횟수 제한을 두고 싶다면
//        IntStream in = new Random().ints(3);
    IntStream in = new Random().ints().limit(3);
    in.forEach(System.out::println);
    // 2가지의 제한 두는 방법

    // 범위는?
    IntStream out = IntStream.rangeClosed(1,10);
    out.forEach(System.out::println);
  }
}
```

<br>

### 스트림의 중간 연산
##### 중간 연산자(Intermediate Operation)
- 스트림의 중간 연산자의 결과는 스트림을 반환하기 때문에 여러 개의 연산자를 연결하여 우리가 원하는 데이터 처리를 수행할 수 있다
- 가장 빈번하게 사용되는 필터링(filtering), 매핑(maping), 정렬(sorting)

![](/assets/img/bootcamp/stream_sourcecode_b.png)

<br>

### 필터링(filter() , distinct() )    
- 조건에 맞는 데이터들만을 정제하는 역할을 하는 중간 연산자    

##### distinct() : 중복을 제거하기 위해 사용
##### filter() :     
조건에 맞는 데이터만을 정제하여 더 작은 컬렉션을 만들어냄    
매개값으로 조건(Predicate)이 주어지고, 조건이 참이 되는 요소만 필터링    
조건은 람다식을 사용하여 정의    

ex)

```java
public static void main(String[] args) {
  //필터링
  List<String> ns = Arrays.asList("뭐","요","인마","어쩌","라구요","라구요");
  // List 니까 stream 바로 사용 가능
  ns.stream()
    .distinct() // 중복 제거
    .forEach(System.out::print);
  System.out.println();
  ns.stream()
    .distinct()
    .filter(e -> e.startsWith("a")) // a 로 시작하는 문자열 거르기
    .forEach(System.out::print);
}
```


### 매핑(map())
- 원하는 필드만 추출하거나 특정 형태로 변환할 때 사용하는 중간연산자
- 조건을 람다식으로 정의

ex)

```java
ns.stream()
.map(e -> e.toUpperCase()) // 대문자 전환
.forEach(System.out::print); // 순회하면서 대문자로 전환 가능한거 전환해줌
System.out.println();
ns.stream()
.map(e -> e + "끼얏호우 ") // List 요소에 문자 추가
.forEach(System.out::print);
System.out.println();

        // 이중 배열인 경우
        String[][] arr = new String[][] { {"a ","b ","c "},{"1 ","2 ","3"} };
        // 기존에 쓰던
        Arrays.stream(arr) // 배열로 스트림 바로 쓰기!
                .map(e -> Arrays.stream(e))
                .forEach(System.out::print);
        // 주소값이 나와벌임
        System.out.println();
        //변형해서
        Arrays.stream(arr)
                .map(e -> Arrays.stream(e))
                .forEach(r -> r.forEach(System.out::print));
        //forEach 메소드에 람다형 조건식으로 넣어줌으로 해결
        System.out.println();
        // 근데 불편하잖음 그럴때 쓰는게 flatMap 메소드
        Arrays.stream(arr)
                .flatMap(Arrays::stream) // 중첩을 제거하고 Stream<String> 으로 캐스팅 해줌 ~> flattening 이라함
                .forEach(System.out::print);
    }
```

<br>

### 정렬(sorted())
- 정렬을 할 때 사용하는 중간 연산자
- 괄호 안에 아무 값도 넣지 않은 상태로 호출하면 기본 정렬(오름차순)로 정렬

ex)

```java
List<String> alphabet = Arrays.asList("v ","f ","s ","d ","b ","a ");
alphabet.stream().sorted().forEach(System.out::print);
// 정렬됨
System.out.println();
// 역순으로?
alphabet.stream().sorted(Comparator.reverseOrder()).forEach(System.out::print);
// Comparator 클래스의 reverseOrder 메소드를 사용하여 내림차순으로 할 수 있음
System.out.println();
```

<br>

### skip()
- 스트림의 일부 요소들을 건너뜀

ex)

```java
// skip
IntStream intStream = IntStream.rangeClosed(1,5);
intStream.skip(3).forEach(System.out::print);
// skip 메소드로 앞의 3글자 건너뛰기
```

<br>

### limit()
- 스트림의 일부를 자름

ex)

```java
// limit
IntStream intStream1 = IntStream.rangeClosed(1,5);
intStream1.limit(2).forEach(System.out::print);
// 앞의 2개만 출력
```


### peek()
- forEach() 와 마찬가지로, 요소들을 순회하며 특정 작업을 수행
- forEach() 와의 차이는 peek는 중간 연산자라 여러번 사용할 수 있지만
- forEach()는 최종 연산자라 쓰면 스트림 닫힘

ex)

```java
// peek
IntStream intStream2 = IntStream.of(1, 2,2,3,4,4,5,5,6,7,8,8,9,10);
int sum = intStream2.filter(e -> e % 2 == 0)
.peek(System.out::println)
.sum();
System.out.println("짝수의 합계 : " +sum);
// peek로 순회하면서 filter에 걸린 값을 출력, 이후에 sum 메소드로 해당 숫자 합침
```

<br>
<br>

### 스트림의 최종 연산
- 스트림 파이프라인에서 최종 연산자가 최종적으로 사용되고 나면, 해당 스트림은 닫히고 모든 연산이 종료됨
- 지연된 연산(lazy evaluation) : 중간 연산은 최종 연산자가 수행될 때야 비로소 스트림의 요소들이 중간 연산을 거쳐 가공된 후에 최종 연산에서 소모됨. 즉, 런타임 시 최종 연산자가 수행되면 그제서야 스트림이 생성되고 중간 연산을 거쳐 가공되고 출력된다는 뜻


#### 기본 집계(sum() , count() , average(), max() , min())
- 숫자와 관련된 기본적인 집계의 경우에는 대부분 최종 연산자

ex)

```java
public class Terminal_Operation {
public static void main(String[] args) {
// 기본 집계 ~> 숫자 관련 메소드는 대부분 최종 연산자로 작동
int[] num = new int[]{1,2,3,4,5};

        // 개수
        long count = Arrays.stream(num).count(); // count 메소드가 long 타입만 받아들이나봄
        System.out.println("개수 : "+count);

        //합계
        int sum = Arrays.stream(num).sum();
        System.out.println("개수 : "+sum);

        // 평균
        double average = Arrays.stream(num).average().getAsDouble(); // getAsDouble 로 값 더블로 받음
        System.out.println("평균 : "+average);
        
        // 최대값
        int max = Arrays.stream(num).max().getAsInt(); // 인트로 값 받기
        System.out.println("최대값 " + max);

        // 최소값
        int min = Arrays.stream(num).min().getAsInt();
        System.out.println("최소값 " + min);

        // 배열의 첫 번째 요소 
        int first = Arrays.stream(num).findFirst().getAsInt();
        System.out.println("배열의 첫번째 요소 " + first);

        // 근데 최종 연산자로 끝냈는데 get? 어떻게 들어가지
        // ~>
        // // 평균값을 구해 Optional 객체로 반환
        //        OptionalDouble average = Arrays.stream(intArr).average();
        //        System.out.println(average);
        //
        //        // 기본형으로 변환
        //        double result = average.getAsDouble();
        //        System.out.println("전체 요소의 평균값 " + result);
        // 이게 풀어 쓴거
        // 즉 OptionalDouble 클래스는 래퍼 클래스로 null 값이 나와서 에러가 발생하는 현상을
        // 방지하기 위함
        // 그래서 캐스팅이 한번 더 필요한 것
        // 즉, 객체로 반환된 값을 다시 기본형으로 변환하기 위한 것이라 스트림 파이프라인과는 관계 없음
```

<br>

### 매칭(allMatch(), anyMatch(), noneMatch() )
- 조건식 람다 Predicate 를 매개변수로 넘겨 스트림의 각 데이터 요소들이 특정한 조건을 충족하는 지 만족시키지 않는 지 검사하여, 그 결과를 boolean 값으로 반환
- allMatch() : 모든 요소들이 조건을 만족하는 지 여부를 판단
- noneMatch() : 모든 요소들이 조건을 만족하지 않는 지 여부를 판단
- anyMatch() : 하나라도 조건을 만족하는 요소가 있는 지 여부를 판단

ex)

```java
boolean result = Arrays.stream(num).allMatch(e -> e % 2 == 0);
System.out.println(result);
result = Arrays.stream(num).anyMatch(e -> e % 3 == 0);
System.out.println(result);
result = Arrays.stream(num).noneMatch(e -> e % 3 == 0);
System.out.println(result);
// boolean 값으로 조건 결과 출력
```

<br>

### 요소 소모(reduce())
- 스트림의 요소를 줄여나가면서 연산을 수행하고 최종적인 결과를 반환
- 첫 번째와 두 번째 요소를 가지고 연산을 수행하고, 그 결과와 다음 세 번째 요소를 가지고 또다시 연산을 수행하는 식으로 연산이 끝날 때까지 반복
- BinaryOperator<T> 로 정의
- reduce() 메서드는 최대 3개까지 매개변수를 받을 수 있음

ex) T reduce(T identity, BinaryOperator<T> accumulator)   
첫 번째 매개변수 identity는 특정 연산을 시작할 때 설정되는 초기값   
두 번째 accumulator 는 각 요소들을 연산하여 나온 누적된 결과값을 생성하는 데 사용하는 조건식   

ex)

```java
// reduce
long sum1 = Arrays.stream(num).sum();
System.out.println("전체 요소 합 : "+sum1);

        int sum2 = Arrays.stream(num)
                .reduce((a,b) -> a + b)
                .getAsInt();
        System.out.println("초기 값이 없는 reduce : "+sum2);
        int sum3 = Arrays.stream(num)
                .reduce(5,(a,b) -> a + b);
        System.out.println("초기 값 5가 있는 reduce : "+sum3);
        // a = 누적 값 , b = 새롭게 더해질 값
        // 최초 연산 시에 1+2 -> a:3, b:3
        // 마지막 6+4 -> a: 10, b: 5
        // 그리고 나서 초기값이 더 해짐
```

<br>

### 요소 수집(collect())
- 중간 연산을 통한 요소들의 데이터 가공 후 요소들을 수집하는 최종 처리 메서드
- Collector 인터페이스 타입의 인자를 받아서 처리

ex)

```java
public class collectMethod {
public static void main(String[] args) {
List<Person> list = Arrays.asList(
new Person("김씨",32,Person.Gender.Male,"aa"),
new Person("이씨",28,Person.Gender.Female,"bb"),
new Person("박씨",33,Person.Gender.Male,"cc"),
new Person("최씨",38,Person.Gender.Male,"dd")
);
Map<String,Integer> male = list.stream()
.filter(e -> e.getGender() == Person.Gender.Male)
.collect(Collectors.toMap(
p -> p.getName(),
p -> p.getAge()
));
System.out.println(male);
// Person 클래스에 객체 생성 후 List 에 때려박고
// 이름과 나이 두 가지를 받아오기 위해
// 키와 값으로 나뉘는 Map 으로 반환
// 이후 Map 에 filer 로 객체의 enum 이 Male 인 객체만 거름
// collect 로 list 의 배열을 Map 형태로 변환 후 Name 과 Age 만 getter 로 읽어옴
// 그럼 collect 로 주서 담은 객체는 male 로 들어감
// 그래서 male 을 출력하면 걸러진 결과가 나옴
// 한 박스에 객체가 두 개 라 가능한듯? 성별은 enum 으로 넣었고
// Person 에 멤버 몇 개를 만들던 상관은 없는데
// collector 로 담는건 Map 타입이라 두 개 밖에 안되네
// 흠.. 근데 어떻게 쓰는지 정확히 모르는듯 Collection 으로 한 객체만 뽑아와보려고했는데 흠...
}
}
class Person {
public enum Gender {Male,Female}
private String name;
private int age;
private Gender gender;
private String middleName;

    public Person(String name, int age, Gender gender, String middleName) {
        this.name = name;
        this.age = age;
        this.gender = gender;
        this.middleName = middleName;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public Gender getGender() {
        return gender;
    }

    public String getMiddleName() {
        return middleName;
    }
}
```

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/222976908616)에서 옮겨짐
