---
title: "자바 : Enum과 Generic 타입, 예외와 Collection"
author: Jeremiah Lee
date: 2023-01-04
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

### Enum
- 열거형
- 상수의 카테고리 제공
- 서로 연관된 상수들의 집합 (상수 = 변하지 않는 값 / final 키워드)
- 몇 가지로 한정된 변하지 않는 데이터를 다루는데 사용

ex)   
열거형 사용전   

```java
public static final int SPRING = 1;
public static final int SUMMER = 2;
public static final int FALL = 3;
public static final int WINTER = 4;
// 열거형 사용 전 전역 상수 정의 방식
// 중복의 위험 ↓
public static final int DJANGO = 1;
public static final int SPRING = 2;
public static final int NEST = 3;
public static final int EXPRESS = 4;
// 전혀 다른 개체로 정의 하고 싶어도 계절의 SPRING 과 중복됨
// 인터페이스로 상수를 구분 한다면?
interface Seasons {
  int SPRING = 1, SUMMER = 2, FALL = 3, WINTER = 4;
}
interface  Frameworks {
  int DJANGO = 1, SPRING = 2, NEST = 3, EXPRESS = 4;
}
// 에러 없이 구분은 가능하지만 타입 안정성이라는 문제 발생
// 이는 상수에 주어진 숫자는 그냥 순서 나타내려고 입력한 것이고 의미가 있는 값이 아니지만
public static void main(String[] args) {
  if (Seasons.SPRING == Frameworks.SPRING) {
    System.out.println(true);
  } else System.out.println(false);
}
// 전혀 다른 개념으로 가져가고 싶은데 비교 연산자를 사용해도 에러가 안나고 비교가 가능해짐
// 이게 안정성 떨어진다는 것
// ~> 열거하기 위해 주어진 숫자로 비교가 가능해져 버린다는 뜻임

// 이를 해결하기 위해 객체로 생성하는 방법도 있음
class Seasons {
  public static final Seasons SPRING = new Seasons();
  public static final Seasons SUMMER = new Seasons();
  public static final Seasons FALL   = new Seasons();
  public static final Seasons WINTER = new Seasons();
}

public static void main(String[] args) {
  Seasons seasons = Seasons.SPRING;
  switch (seasons) { // << 타입 안맞는다고 예외 발생
    case Seasons.SPRING:
      System.out.println("봄");
      break;
    case Seasons.SUMMER:
      System.out.println("여름");
      break;
    case Seasons.FALL:
      System.out.println("가을");
      break;
    case Seasons.WINTER:
      System.out.println("겨울");
      break;
  }
}

class Frameworks {
  public static final Frameworks DJANGO  = new Frameworks();
  public static final Frameworks SPRING  = new Frameworks();
  public static final Frameworks NEST    = new Frameworks();
  public static final Frameworks EXPRESS = new Frameworks();
}

// 해결은 됐는데 또 이건 사용자가 정의한 타입이라 보는 바와 같이 switch 문에 활용 불가
// 그리고 코드도 엄청 김 귀찮잖아?
// 그래서 enum 쓰면 됨
```

ex)   
열거형 적용

```java
enum Season {SPRING, SUMMER, FALL, WINTER}
enum Frameworks {DJANGO, SPRING, NEST, EXPRESS}

class Check {
  public static void main(String[] args) {
    Season season = Season.SPRING;
    switch (season) {
      case SPRING:
        System.out.println("봄");
        break;
      case SUMMER:
        System.out.println("여름");
        break;
      case FALL:
        System.out.println("가을");
        break;
      case WINTER:
        System.out.println("겨울");
        break;
    }
  }

}
// 간편하고 보기도 좋음
// switch 문에도 활용 가능
```

! enum 은 관례적으로 이름을 전부 대문자로함   
용법은 위와 같이 enum Season {SPRING, SUMMER, FALL, WINTER}   
여기서 0 부터 인덱스가 정해짐   

용법 ex)

```java
public class EnumExample {
    public static void main(String[] args) {
        System.out.println(Seasons.SPRING); // 출력 SPRING
    }
}
```

또한 메인 메소드에 enum으로 열거한 상수를 불러 올때는 스태틱에서 변수 참조 하는거랑 마찬가지로 "<이넘명> <변수명> = <이넘명>.<상수명>" 으로 불러오면 됨

### Enum의 메소드 
```
String      name()                   열거 객체가 가지고 있는 문자열을 리턴하며, 리턴되는 문자열은 열거타입을 정의 할 때 사용한 상수 이름과 동일합니다.
int         ordinal()                열거 객체의 순번(0부터 시작)을 리턴합니다.
int         compareTo(비교값)         주어진 매개값과 비교해서 순번 차이를 리턴합니다.
열거 타입     valueOf(String name)    주어진 문자열의 열거 객체를 리턴합니다.
열거 배열     values()                모든 열거 객체들을 배열로 리턴합니다.
```


### Generic   
- 타입을 구체적으로 지정하는 것이 아니라, 추후에 지정할 수 있도록 일반화해두는 것    
- 작성한 클래스 또는 메서드의 코드가 특정 데이터 타입에 얽매이지 않게 해둔 것을 의미   

클래스 정의하고 객체 정의 다했는데   
같은 기능을 하면서 타입을 변경하고 싶다?   
원래 같으면   
같은 클래스 하나 더 만들고서 객체를 똑같이 정의하고 데이터 타입만 String, int, boolean, double 등등   
다시 재정의를 해줘야함   

이럴 때 쓰는게 제네릭이고   
이 제네릭을 사용하면 복붙 안하고 타입을 쉽게 변경할 수 있어짐   

용법ex)

```java
class Basket<T> { // << 제네릭 클래스 / <T> = 타입 매개변수 선언
  private T item;

  public Basket(T item) {
    this.item = item;
  }

  public T getItem() {
    return item;
  }

  public void setItem(T item) {
    this.item = item;
  }
}
```

이런 식으로 클래스명<T> 붙여주면   
"이 클래스는 T라는 타입 파라미터를 가질 것이다"라는 뜻이됨   

이후 인스턴스 화 할때   

```java
public class GenericWorking {
    public static void main(String[] args) {
        Basket<String> basket = new Basket<String>("쟈가쟝");
        System.out.println(basket.getItem());

        Basket<Integer> basket1 = new Basket<Integer>(1);
        System.out.println(basket1.getItem());
    }
}
```

이런식으로 내가 <> 안에 타입의 래퍼 클래스를 넣게되면 해당 데이터 타입으로 유동적으로 바뀌는 것   
또 바꾸고 싶다면 새로 인스턴스화 해서 마찬가지로 진행하면 됨   

타입 매개변수는 임의로 설정 가능함   
보통 Type, Key, Value, Element, Number, Result 의 맨 앞글자를 따서 자주 사용   
평범한 매개변수와 같이 여러개를 넣어야한다하면 <T, K, V ...> 로 할 수 있음   
단, 클래스 변수에는 사용 불가능함    
/ 다시 말해 클래스의 필드 멤버인 인스턴스 변수는 가능하지만 같은 필드 멤버인 static이 붙는 클래스 변수에는 불가능함    
이는 클래스 변수에 타입 매개변수를 사용하면 클래스 변수의 타입이 인스턴스 별로 달라지게 되기 때문임    
쉽게 말해 정적 변수인 클래스 변수는 변하지 말아야하는데 인스턴스 화 할때마다 데이터 타입이 바뀌어야하는     
제네릭 특성상 공생이 불가능하다는 것임     

!

```java
Basket<String>  basket1 = new Basket<>("Hello");
Basket<Integer> basket2 = new Basket<>(10);
Basket<Double>  basket2 = new Basket<>(3.14);
```

이렇게 참조 변수의 타입을 넣으면 참조할 변수의 타입은 생략해도 참조 변수 타입으로 알아서 유추함   
또, 인스턴스화 할때 타입 매개변수는 원시 타입인 int, double 등이 아닌 해당 원시 타입의 래퍼 클래스인 Integer, Double 등을 사용해야함

<br>

### 제네릭 다형성 적용

```java
Basket<Flower> flowerBasket = new Basket<>(); // << Flower를 타입 매개변수로 입력
        flowerBasket.setItem(new Rose()); // << 상속 받고 있기 때문에 flowerBasket의 item에 할당 가능
        flowerBasket.setItem(new RosePasta()); // 오류 / Flower의 상속받고있지 않음
  }
  }

class Flower {}
class Rose extends Flower{}
class RosePasta Flower{} // << 오류
```
기능은 Basket을 쓰고 타입은 Flower로 해서 Flower의 상속 받고 있는 Rose도 Basket의 매개변수가 될 수있다는 말임   

이렇듯 제네릭 클래스는 인스턴스화 할 때 타입을 지정하는데에 있어 제한이 없음   
근데 이걸 내가 지정할 수 있음 어떻게?   

ex)

```java
class Sun {}
class Earth extends Sun {}
class Pluto {}


//class Space<T> { 타입 매개변수에 제한이 없음
class Space<T extends Sun> { // Sun 클래스와 그 하위 클래스만을 타입 매개변수로 갖게끔 설정
private T star;

    public T getStar() {
        return star;
    }

    public void setStar(T star) {
        this.star = star;
    }
}

// 메인메소드 인스턴스화↓
Space<Earth> earthSpace = new Space<>(); // Sun의 하위 클래스인 Earth 가능
//        Space<Pluto> plutoSpace = new Space<>(); // 임의 지정해서 상위나 하위가 아닌 영 다른 클래스는 오류

// << 마찬가지로 인터페이스도 동일한 방식으로 임의 조정 가능함
// 단, implements를 이용하지않고 상속관계와 마찬가지로 extends 를 사용
```

만약   
특정 클래스를 상속받으면서 동시에 특정 인터페이스를 구현한 클래스만 타입으로 지정할 수 있도록 제한하려면?   
class Basket<T extends Flower & Plant>   
이런식으로 타입 매개변수에 & 을 넣고 둘 다 가능함   
단, 클래스가 인터페이스보다 앞에 와야함   

### 제네릭 메소드   
- 클래스 전체를 제네릭으로 선언할 수도 있지만, 클래스 내부의 특정 메서드만 제네릭으로 선언할 수 있음   
- 제네릭 메서드의 타입 매개변수 선언은 반환타입 앞에서 이루어지며, 해당 메서드 내에서만 선언한 타입 매개변수를 쓸 수 있음   

ex)

```java
class Basket {
		...
  public <T> void add(T element) {
				...
  }
}

// 제네릭 메서드의 타입 매개변수는 제네릭 클래스의 타입 매개변수와 별개의 것
class Basket<T> {                        // 1 : 여기에서 선언한 타입 매개변수 T와 
		...
  public <T> void add(T element) { // 2 : 여기에서 선언한 타입 매개변수 T는 서로 다른 것
				...
  }
}
```

~> 이는 타입 지정 시점이 다르기 때문   
클래스에 선언된 타입 매개변수는 클래스가 인스턴스화 할때 타입이 지정되고   
메소드의 타입 매개변수는 메소드가 호출될 때 지정됨   

```java
Basket<String> basket = new Bakset<>(); // 위 예제의 1의 T가 String으로 지정
basket.<Integer>add(10);                // 위 예제의 2의 T가 Integer로 지정
basket.add(10);                         // 타입 지정을 생략 가능
```

!! 메소드 타입 매개변수는 static 메소드에서도 선언 후 사용 가능

```java
class Basket {
		...
  static <T> int setPrice(T element) {
				...
  }
}
```

!주의!   
제네릭 메소드는 정의 하는 시점에 어떤 타입이 입력되는지 알 수 없기 때문에   
String, Integer, Double 등의 클래스 메소드는 사용 불가능   
단, 모든 클래스의 최상위 상속자인 Object 클래스의 메소드는 사용가능함   

```java
public <T> void getPrint(T item) {
//        System.out.println(item.length()); << 오류남 // T 정의 할 수 없데
        System.out.println(item.toString());
        System.out.println(item.equals(true)); // << 최상위 클래스인 Object 의 메소드는 가능
    }
```

### 제네릭 와일드카드
- 어떠한 용도로든 사용될 수 있는 일종의 비장의 카드를 의미
- 자바의 제네릭에서 와일드카드는 어떠한 타입으로든 대체될 수 있는 타입 파라미터를 의미
- 기호 ?로 와일드카드를 사용할 수 있음
- 일반적으로 와일드카드는 extends와 super 키워드를 조합하여 사용

ex)   
<? extends T>   
<? super T>   

<? extends T> = 와일드카드에 상한 제한을 두는 것 / T와 T를 상속받는 하위 클래스 타입만 타입 파라미터로 받을 수 있도록 지정

<? super T> = 와일드카드에 하한 제한을 두는 것 / T와 T의 상위 클래스만 타입 파라미터로 받도록 함

<?> = extends 및 super 키워드와 조합하지 않은 와일드카드 / (Object의 상속을 받는)모든 클래스 타입을 타입 파라미터로 받을 수 있음   
= 결국 <? extends Object> 와 같음

<br>

### 예외 처리(Exception Handling)
- 비정상적인 종료를 방지 & 정상적인 실행 상태를 유지
- 발생 원인   
내부 요인 : 개발자의 코드 작성 에러   
외부 요인 : 하드웨어의 문제, 네트워크의 연결 끊김, 사용자 조작 오류   

ex)

```java
public class ExceptionWorking {
  public static void main(String[] args) {
    BufferedReader Exist = new BufferedReader(new FileReader("없는 파일"));
    Exist.readLine();
    Exist.close();
  }
}
```

= 오류코드 :   
unreported exception java.io.FileNotFoundException; must be caught or declared to be thrown   
없는 파일 예외 상황 발생    

ex2)

```java
public class ExceptionWorking {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3};
        System.out.println(arr[3]);
    }
}
```

= 오류코드 :   
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 3 out of bounds for length 3   
at Frame.Exception.ExceptionWorking.main(ExceptionWorking.java:13)   
배열에서 벗어난 범위 예외 발생   

! 여기서 ex1은 IDE가 빨간 밑줄로 알려주지만   
ex2는 IDE가 노랑 밑줄로 알려줌 > 코드 실행된다고 착각할 수 있는 상황   
이는 예외 발생 시점에 따라 다른 것으로   
컴파일 에러, 런타임 에러라 불림   

### 컴파일 에러
- “컴파일 할 때" 발생하는 에러 == Javac 에서 걸러지는 예외
- 주로 세미콜론 생략, 오탈자, 잘못된 자료형, 잘못된 포맷 등 문법적인 문제   
== 신택스 에러(Systax Errors)라고도 불림
- 이 컴파일 에러가 IDE가 빨간 밑줄로 알려주는 에러   
쉽게 찾을 수 있어서 그리 어렵지 않게 핸들링 가능

### 런타임 에러
- 런타임 시(코드를 실행 시)에 발생하는 에러 == JVM에서 걸러지는 예외
- 이 런타임 에러가 IDE가 노란 밑줄로 알려주는 에러
- 주로 컴퓨터가 수행할 수 없는 특정한 작업 요청 시 발생

### 에러 VS 예외 ?
- 에러는 한번 발생하면 복구하기 어려운 수준의 심각한 오류
- 예외는 잘못된 사용 또는 코딩으로 인한 상대적으로 미약한 수준의 오류로서 코드 수정 등을 통해 수습이 가능한 오류

### 예외 클래스의 상속 계층도
- 일반 예외 클래스(Exception)   
= 런타임 시 발생하는 Runtime Exception 클래스와 그 하위 클래스를 제외한 모든 Exception 클래스와 그 하위 클래스들   
컴파일러가 코드 실행 전에 예외 처리 코드 여부를 검사한다고하여 checked 예외라 부르기도 함   
주로 잘못된 클래스명(ClassNotFoundException)이나 데이터 형식(DataFormatException) 등 사용자편의 실수로 발생   
- 실행 예외 클래스(Runtime Exception)   
= 런타임 시 발생하는 RuntimeException 클래스와 그 하위클래스를 지칭   
컴파일러가 예외 처리 코드 여부를 검사하지 않는다는 의미에서 unchecked 예외라 부르기도 함   
주로 개발자의 실수에 의해 발생하는 경우가 많고, 자바 문법 요소와 관련   

### try - catch문
- 예외 처리란 잠재적으로 발생할 수 있는 비정상 종료나 오류에 대비하여 정상 실행을 유지할 수 있도록 처리하는 코드 작성 과정을 의미
- 자바에서 예외 처리는 try - catch 블럭을 통해 구현이 가능

ex)

```java
try { // 예외 발생 가능성 있는 코드 삽입
            System.out.println(4 / 0);
        }
        catch (Exception e1) { // try에서 예외 발생시 실행
            System.out.printf("이게 되겠냐고");
        } // e1 에서도 예외 발생할 것 같다면 catch 로 계속 늘려주면 됨
        finally { // 예외 발생 여부와 관계 없이 항상 실행 / 옵셔널임
            System.out.printf(" 이게");
        } 
```

예외 전가
- try-catch 문 외에 예외를 호출한 곳으로 다시 예외를 떠넘기는 방법   
반환타입 메서드명(매개변수, ...) throws 예외클래스1, 예외클래스2, ... {   
...생략...   
}   

ex)

```java
void ExampleMethod() throws Exception {
}

ex)1
public class ExceptionWorking {
  public static void main(String[] args) {
    try {
      throwException();
    } catch (ClassNotFoundException e) {
      System.out.println(e.getMessage());
    }
  }
  static void throwException() throws NullPointerException, ClassNotFoundException {
    Class.forName("java.lang.StringX");
  }
// throwException 메소드에서 발생한 예외 상황을 throws 키워드를 통해
// main 메소드 안의 try catch 로 넘겨버림
// 즉 throws 키워드로 예외 처리의 책임을 main 메소드가 지게끔 떠넘긴 것
``` 

throw 키워드로 의도된 예외 작성

```java
try {
Exception intendedException = new Exception("의도된 예외");
            throw intendedException;
        } catch (Exception e) {
  System.out.println("\"의도된 예외\" 발생 성공");
        }
// Exception 인스턴스화로 예외 그 자체를 생성하고 다시 Exception을 참조하는 변수명에 예외 던지기
```

<br>
<br>

### 컬렉션 프레임워크(Collection Framework)
- Java에서는 데이터를 저장하기 위해 널리 알려져 있는 자료 구조를 바탕으로   
객체들을 효율적으로 추가, 삭제, 검색할 수 있도록 컬렉션을 만들고,   
관련된 인터페이스와 클래스를 포함시켜 두었음 >> 이거 전체가 컬렉션 프레임워크   
- 컬렉션이란 여러 데이터들을 그룹으로 묶어놓은 것
- 컬렉션을 다루는 데에 있어 편리한 메서드들을 미리 정의해놓은 것이 컬렉션 프레임워크
- 특정 자료 구조에 데이터를 추가하고, 삭제하고, 수정하고, 검색하는 등의 동작을 수행하는 편리한 메서드들을 제공

### 컬렉션 프레임워크의 구조

![](/assets/img/bootcamp/collectionframework.png)

### List
- 데이터의 순서가 유지되며, 중복 저장이 가능한 컬렉션을 구현하는 데에 사용
- 인덱스로 객체 조회 > 중복 허용
- ArrayList, Vector, Stack, LinkedList 등

### Set
- 데이터의 순서가 유지되지 않으며, 중복 저장이 불가능한 컬렉션을 구현하는 데에 사용
- 값 그 자체로 객체 조회 > 중복 불가
- HashSet, TreeSet 등

### Map
- 키(key)와 값(value)의 쌍으로 데이터를 저장하는 컬렉션을 구현하는 데에 사용
- 데이터의 순서가 유지되지 않으며, 키는 값을 식별하기 위해 사용되므로 중복 저장이 불가능하지만, 값은 중복 저장이 가능
- HashMap, HashTable, TreeMap, Properties 등
   
List와 Set은 서로 공통점이 많아 위 그림과 같이 Collection이라는 인터페이스로 묶임    
~> 즉, 이 둘의 공통점이 추출되어 추상화된 것이 바로 Collection이라는 인터페이스    

주로 ArrayList랑 HashMap 많이 씀   

 
### Collection 인터페이스 메소드    

![](/assets/img/bootcamp/collectioninterfacemethod.png)

<br>

### List<E>
- 배열과 같이 객체를 일렬로 늘어놓은 구조
- 객체를 인덱스로 관리하기 때문에 객체를 저장하면 자동으로 인덱스가 부여되고, 인덱스로 객체를 검색, 추가, 삭제할 수 있는 등의 여러 기능을 제공

### ArrayList
- List 인터페이스를 구현한 클래스
- 컬렉션 프레임워크에서 가장 많이 사용
- 객체가 인덱스로 관리된다는 점에서는 배열과 유사
- 배열은 생성될 때 크기가 고정되며, 크기를 변경할 수 없는 반면, ArrayList는 저장 용량을 초과하여 객체들이 추가되면, 자동으로 저장용량이 늘어나게 됨
- 리스트 계열 자료구조의 특성을 이어받아 데이터가 연속적으로 존재, 즉, 데이터의 순서를 유지

용법 ex)   
```java
ArrayList<타입 매개변수> 객체명 = new ArrayList<타입 매개변수>(초기 저장 용량);
ArrayList<String> container1 = new ArrayList<String>();
// String 타입의 객체를 저장하는 ArrayList 생성
// 초기 용량이 인자로 전달되지 않으면 기본적으로 10으로 지정됩니다.
```

```java
ArrayList<String> container2 = new ArrayList<String>(30);
// String 타입의 객체를 저장하는 ArrayList 생성
// 초기 용량을 30으로 지정하였습니다.
```

ArrayList에 객체를 추가하면 인덱스 0부터 차례대로 저장   
특정 인덱스의 객체를 제거하면, 바로 뒤 인덱스부터 마지막 인덱스까지 모두 앞으로 1씩 당겨짐   

```java
public static void main(String[] args) {
  ArrayList<String> list = new ArrayList<>();
  // ArrayList 생성 후 list 에 할당

  list.add("Java");
  list.add("C++");
  list.add("C#");
  // String 타입의 데이터를 리스트에 추가

  int size = list.size();
  // 저장된 총 객체 수 얻기

  String skill = list.get(0);
  // 0번 인덱스에 저장된 객체 얻기

  for (int i = 0; i < list.size(); i++) {
    String str = list.get(i);
    System.out.println(i +" : "+str);
  } // 저장된 객체 수 만큼 조회해서 저장된 문자열 타입 객체 출력

  for (String str: list) {
    System.out.println(str);
  } // for-each 문으로 순회

  list.remove(0); // 0번 인덱스 객체 삭제

  for (String str : list) {
    System.out.println(str);
  } // 다시 순회시 0번 인덱스 삭제 완료

  list.add("Hello"); // 리스트에 문자열 다시 추가

  for (String str:list) {
    System.out.println(str);
  } // 순회시 전에 있던 객체들의 순서가 1씩 당겨지고 마지막에 Hello 출력

  // 이런식으로 객체의 삭제, 삽입은 불편 ~> 편하게 하려면 LinkedList 사용
}
```

### LinkedList
- List 인터페이스를 구현한 클래스
- 데이터를 효율적으로 추가, 삭제, 변경하기 위해 사용
- 배열에는 모든 데이터가 연속적으로 존재하지만, LinkedList에는 불연속적으로 존재하며, 이 데이터는 서로 연결(link)되어 있음

![](/assets/img/bootcamp/LinkedList_bblb.png)
    
각 요소(node)들은 자신과 연결된 이전 요소 및 다음 요소의 주소값과 데이터로 구성되어 있음     
배열처럼 데이터를 이동하기 위해 복사할 필요가 없기 때문에 처리 속도가 훨씬 빠름     

삭제 삽입 ex)

```java
public static void main(String[] args) {
LinkedList<String> list = new LinkedList<>();
//Linked List 를 생성하여 list 에 할당

        list.add("Java");
        list.add("C++");
        list.add("C#");
        // Linked List 에 String 타입 데이터 추가

        int size = list.size();
        System.out.println(size);
        // 저장된 총 객체 수 얻기

        String skill = list.get(0);
        System.out.println(skill);
        // 0번 인덱스에 저장된 객체 얻기

        for (int i = 0; i < list.size(); i++) {
            String str = list.get(i);
            System.out.println(i+" : "+str);
        } // 순회하며 저장된 객체 순서대로 조회

        for (String str : list) {
            System.out.println(str);
        } // for-each 문으로 순회 가능

        list.remove(1);
        // 1번 인덱스 삭제
        list.add(0,"Hello"); // 0번 인덱스로 삽입
        for (String str : list) {
            System.out.println(str);
        } // 삭제후 추가후 바로 순회
    }
```


### ArrayList VS LinkedList ?
- ArrayList에 객체를 순차적으로 저장할 때는 데이터를 이동하지 않아도 되므로 작업 속도가 빠르지만,   
중간에 위치한 객체를 추가 및 삭제할 때에는 데이터 이동이 많이 일어나므로 속도가 저하   
반면 인덱스가 n인 요소의 주소값을 얻기 위해서는 배열의 주소 + n * 데이터 타입의 크기를 계산하여 데이터에 빠르게 접근이 가능하기 때문에 검색(읽기) 측면에서는 유리   
- ArrayList의 강점
  - 데이터를 순차적으로 추가하거나 삭제하는 경우
  - 데이터를 읽어들이는 경우
- 단점
  - 중간에 데이터를 추가하거나, 중간에 위치하는 데이터를 삭제하는 경우 비교적 느림

- LinkedList는 데이터를 중간에 추가하거나 삭제하는 경우 ArrayList보다 빠른 속도를 보여줌
- LinkedList의 강점
  - 중간에 위치하는 데이터를 추가하거나 삭제하는 경우
- 단점
  - 데이터 검색 느림


이유는   
ArrayList는 순차적으로 차곡차곡 쌓는 클래스라 순서대로 처리하는 경우에는 빠르고 좋지만   
중간에 데이터 삽입 삭제는 일일히 찾아서 하는 식이라 느리고 어려움   
ex) 컵 쌓기하다가 중간에 컵 집어넣는 느낌   

LinkedList는 마찬가지로 차곡차곡 넣지만 앞 뒤 객체의 주소값까지 저장됨   
그러니까 자기 위치를 특정하고 있는거임   
그래서 중간에 데이터 삽입 삭제가 빠르고 쉬움   
대신 주소값을 불러와서 참조하고 있는 요소를 출력해야하는 검색은 느림   
왜? 앞뒤 주소값까지 불러오기 때문임   


### Iterator
- 반복자
- 컬렉션에 저장된 요소들을 순차적으로 읽어오는 역할
- Iterator의 컬렉션 순회 기능은 Iterator 인터페이스에 정의되어져 있으며,   
Collection 인터페이스에는 Iterator 인터페이스를 구현한 클래스의 인스턴스를 반환하는 메서드인 iterator()가 정의되어져 있음   
즉, Collection 인터페이스에 정의된 iterator()를 호출하면, Iterator 타입의 인스턴스가 반환   
~> Collection 인터페이스를 상속받는 List와 Set 인터페이스를 구현한 클래스들은 iterator() 메서드를 사용할 수 있다는 뜻    
~> 이터레이터의 타입 매개변수는 배열의 타입과 같은 래퍼 클래스를 넣어줘야함    

![](/assets/img/bootcamp/Iterator_interface_method.png)

! 이거 많이 헷갈림   
쉽게 말하면 while로 컬렉션 순회하기 위해 최적화 하는 반복자라는 거     

용법 ex)   
Iterator를 활용하여 컬렉션의 객체를 읽어올 때에는 next() 메서드를 사용   
~> next() 메서드를 사용하기 전에는 먼저 가져올 객체가 있는지 hasNext()를 통해 확인하는 것이 좋음   
hasNext() 메서드는 읽어올 다음 객체가 있으면, true를 리턴하고, 더 이상 가져올 객체가 없으면 false를 리턴   
~> true가 리턴될 때에만 next()메서드가 동작하도록 코드를 작성   

```java
public static void main(String[] args) {
ArrayList<String> list = new ArrayList<>();

        list.add("Java");
        list.add("C#");
        list.add("C++");

        Iterator<String> iterator = list.iterator();
        // ArrayList 에서 Iterator 의 iterator() 메소드 호출


        while (iterator.hasNext()) { // 읽어올 다음 객체 있으면
            String str = iterator.next(); // next() 메소드를 통해 다음 객체를 읽어옴
            if (str.equals("Java")) {iterator.remove();} // if 문으로 해당하는 객체 삭제
        } // 이거랑 위에 쓴 for-each 랑 같은 거임 / 대신 얘는 Iterator 쓴 버전

        for (String str:list) {
            System.out.println(str);
        } // 삭제 됐는지 확인 용
    }
```

<br>

### Set<E>
- 수학에서 Set은 집합을 의미
- 중복된 값을 허용 X
- 요소의 중복을 허용하지 않고, 저장 순서를 유지하지 않는 컬렉션
- 대표적인 Set을 구현한 클래스에는 HashSet, TreeSet

![](/assets/img/bootcamp/Set_interface_method.png)

<br>

### HashSet
- 중복된 값을 허용하지 않으며, 저장 순서를 유지하지 않음
- 중복 값 판단?

1. add(Object o)를 통해 객체를 저장하고자 합니다.
2. 이 때, 저장하고자 하는 객체의 해시코드를 hashCode() 메서드를 통해 얻어냅니다.
3. Set이 저장하고 있는 모든 객체들의 해시코드를 hashCode() 메서드로 얻어냅니다.
4. 저장하고자 하는 객체의 해시코드와, Set에 이미 저장되어져 있던 객체들의 해시코드를 비교하여, 같은 해시코드가 있는지 검사합니다.
   - 이 때, 만약 같은 해시코드를 가진 객체가 존재한다면 아래의 5번으로 넘어갑니다.
   - 같은 해시코드를 가진 객체가 존재하지 않는다면, Set에 객체가 추가되며 add(Object o) 메서드가 true를 리턴합니다.
5. equals() 메서드를 통해 객체를 비교합니다.
   - true가 리턴된다면 중복 객체로 간주되어 Set에 추가되지 않으며, add(Object o)가 false를 리턴합니다.
   - false가 리턴된다면 Set에 객체가 추가되며, add(Object o) 메서드가 true를 리턴합니다.

ex)

```java
public static void main(String[] args) {
HashSet<String> languages = new HashSet<>();
// HashSet 생성

        languages.add("Java");
        languages.add("C#");
        languages.add("C++");
        languages.add("Kotlin");
        languages.add("Python");
        languages.add("Ruby");
        languages.add("Java"); // 중복 값 투입, 이미 IDE 에서 런타임 에러 뜨긴하네

        Iterator<String> it = languages.iterator();
        // 반복자 생성 후 it에 할당

        while (it.hasNext()) {
            System.out.println(it.next());
        } // 반복자를 통해 HashSet 을 순회하며 각 요소 출력
        // ~> Java가 두 번 출력 됐지만 HashSet이 이를 받아들이지 않고 한번만 저장됨
```

<br>

### TreeSet
- 이진 탐색 트리 형태
- 데이터의 중복 저장을 허용하지 않고 저장 순서를 유지하지 않는 Set 인터페이스의 특징은 그대로 유지
- 기본 오름차순 정렬

![](/assets/img/bootcamp/TreeSet_shape.png)

~> 이진 탐색 트리 형태로 좌측에 부모보다 작은 값 우측에 부모보다 큰 값으로 정렬됨


### TreeSet 자료 구조 구현 의사 코드

```java
class Node {
    Object element; // 객체의 주소값을 저장하는 참조변수 입니다.
    Node left;      // 왼쪽 자식 노드의 주소값을 저장하는 참조변수입니다.
    Node right;     // 오른쪽 자식 노드의 주소값을 저장하는 참조변수입니다.
}
```

TreeSet은 기본 정렬 방식이 오름차순

```java
public static void main(String[] args) {
TreeSet<String> workers = new TreeSet<>();
// TreeSet 생성

        workers.add("Lee");
        workers.add("Kim");
        workers.add("Park");
        workers.add("Joe");
        // 막 넣었는데
        System.out.println(workers); // 자동으로 사전 편찬 순으로 오름차순 정렬됨 ㄷㄷ
        System.out.println(workers.first()); // 오름차순으로 정렬된 객체 중 첫번째
        System.out.println(workers.last()); // 마찬가지로 마지막 객체
        System.out.println(workers.higher("Lee")); // 매개변수 인자 문자열 다음 값 출력
        System.out.println(workers.lower("Lee")); // 마찬가지 이전 값 출력
        System.out.println();
        System.out.println(workers.subSet("Kim","Park"));
        System.out.println(workers.subSet("Joe","Park")); // 앞 문자열부터 뒤 문자열 이전 객체 까지 출력
    }
```

<br>

### Map<K, V>
- 키(key)와 값(value)으로 구성된 객체를 저장하는 구조 << key랑 value 합쳐서 Entry객체
- 쉽게 Key = 주소 / Value = 해당 주소값에 들어있는 값
- Entry 객체는 키와 값을 각각 Key 객체와 Value 객체로 저장

![](/assets/img/bootcamp/Map_interface_shape.png)

~> 중요한 사실은 키는 중복 저장될 수 없지만, 값은 중복 저장이 가능하다는 것   
<< 키의 역할이 값을 식별하는 것이기 때문   
만약 기존에 저장된 키와 동일한 키로 값을 저장하면, 기존의 값이 새로운 값으로 대치   

<br>

### Map 인터페이스 메소드

![](/assets/img/bootcamp/Map_interface_method.png)
    
List가 인덱스를 기준으로 관리되는 반면에, Map은 키(key)로 객체들을 관리하기 때문에 키를 매개값으로 갖는 메서드가 많음

<br>

### HashMap
- Map 인터페이스를 구현한 대표적인 클래스
- 키와 값으로 구성된 객체를 저장 << Entry 객체
- 얘는 지 혼자 순회가 불가능함 > Iterator 써서 Set이랑 합쳐서 순회하는 것임   
중복 불가능 특성이 공통이라 Set 쓰는 것

![](/assets/img/bootcamp/HashMap_inner.png)

- 해시 함수를 통해 '키'와 '값'이 저장되는 위치를 결정   
~> 사용자는 그 위치를 알 수 없고, 삽입되는 순서와 위치 또한 관계가 없음

HashMap의 개별 요소가 되는 Entry 객체는 Map 인터페이스의 내부 인터페이스인 Entry 인터페이스를 구현   

![](/assets/img/bootcamp/Map.Entry_interface_method.png)

용법 ex)

```java
public static void main(String[] args) {
HashMap<String,Integer> hashMap = new HashMap<>();
// HashMap 은 <키, 값> 타입을 따로 지정해주어야 함

        hashMap.put("피카츄",88);
        hashMap.put("라이츄",97);
        hashMap.put("파이리",64);
        hashMap.put("꼬부기",25);
        hashMap.put("야도란",43);
        hashMap.put("버터풀",61);
        // Entry(= 키 + 값) 객체 저장

        System.out.println("총 entry 수 : "+hashMap.size());
        // 총 Entry 수

        System.out.println("파이리 : "+hashMap.get("파이리"));
        // 특정 객체의 값 찾기

        Set<String> keySet = hashMap.keySet();
        // key 를 요소로 가지는 set 을 map 순회를 위해 생성
        Iterator<String> keyIterator = keySet.iterator();
        // while 순회를 위한 반복자 할당

        while (keyIterator.hasNext()) {
            String key = keyIterator.next(); // 키 순회
            Integer value = hashMap.get(key); // 값 순회 (반복자 생성해서 하면 와일 안에도 래퍼 클래스 넣네)
            System.out.println(key + " : " + value);
        }
        System.out.println();
        hashMap.remove("버터풀");
        // 객체 삭제

        Set<java.util.Map.Entry<String,Integer>> entrySet = hashMap.entrySet();
        //Entry 객체를 요소로 가지는 Set 생성 ~> 순회하기 위함

        Iterator<java.util.Map.Entry<String ,Integer>> entryIterator = entrySet.iterator();
        // 마찬가지로 while 순회를 위해 반복자 생성
        while (entryIterator.hasNext())  {
            java.util.Map.Entry<String, Integer> entry = entryIterator.next();
            String key = entry.getKey(); // Map.Entry 인터페이스의 메소드
            Integer value = entry.getValue(); // 마찬가지
            System.out.println(key + " : " + value);
        }
        // 객체 삭제 확인

        hashMap.clear();
        // 객체 전체 삭제
        System.out.println(hashMap.size());
        // 객체 삭제 확인


    }
```

<br>

### HashTable
- HashTable은 HashMap과 내부 구조가 동일하며, 사용 방법 또한 매우 유사
- 간단하게 HashMap이 HashTable의 새로운 버전
- 많이 안 씀

ex)

```java
public static void main(String[] args) {
Hashtable<String, Integer> map = new Hashtable<>();

        map.put("qwe",123);
        map.put("asd",456);
        map.put("zxc",789);

        System.out.println(map);

        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("아이디와 비밀번호를 입력해 주세요.");
            System.out.println("아이디");
            String id = scanner.nextLine();

            System.out.println("비밀번호");
            int password = Integer.parseInt(scanner.nextLine());

            if (map.containsKey(id)) {
                if (map.containsValue(password)) {
                    System.out.println("로그인 완료");
                    break;
                } else System.out.println("비밀번호가 일치하지 않습니다.");
            } else System.out.println("아이디가 일치하지 않습니다.");

        }
    }
```

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/222974918617)에서 옮겨짐
