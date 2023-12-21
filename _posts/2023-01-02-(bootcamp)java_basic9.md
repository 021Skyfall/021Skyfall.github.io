---
title: "자바 : OOP : 복사 생성자와 싱글톤 패턴 / 얕은복사, 깊은복사"
author: Jeremiah Lee
date: 2023-01-02
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

cart 클래스 chooseOption

```java
if (input.equals("2")){

                ((Burgers) product).setBurgerSet(true);

            }


if (input.equals("2")) {

                ((Beverage) product).setHasStraw(false);

            }
```

이 두 부분 스캐너로 받아온 사용자 입력 값이 String 타입이라 .equals("") 로 했어야됐는데

걍 비교연산자 써서 == 이거 들어가 있어서 자꾸

입력 값이 변경이 안되고

product repository에 넣은 값으로 자꾸 나와서 헤맸음


### 얕은 복사 & 깊은 복사
- 얕은 복사는 객체의 참조값을 복사하는 것을 의미

ex)

```java
public void shallowCopyExample(Product product) {
    Product newProduct = product;
}
```

- 깊은 복사는 내용이 동일하지만 참조값이 다른 새로운 객체를 생성하는 것을 의미

ex)

```java
public void DeepCopyExample(Product product) {
    Product newProduct = new Product(product.getId, product.getName, ...);
}
```

깊은 복사 넣어서 출력 해봤는데 사이드 메뉴가   
감자튀김 11 넣고 감자튀김 22 넣으면   

감자튀김 22 감자튀김 22 로 나왔음;   
문제가 뭔가 했더니   

```java
Product[] newItems = new Product[items.length + 1];
System.arraycopy(items, 0, newItems, 0, items.length);
newItems[newItems.length - 1] = Product; // << 이 부분 이었음
items = newItems;
```

Product를 새로운 newProduct라는 변수에 할당해서   
바로 아래에 생성자를 통해 복사를 해줬는데   
저기서 아이템들을 담을 Product를 복사된 newProduct로 변경하는걸 까먹음   

```java
public class Order_Function {
private Cart_Function cartFunction;

public Order_Function(Cart_Function cartFunction) {
    this.cartFunction = cartFunction;
}
```

이와 같이 되어 있을때   
사용할 클래스를 변수 초기화해주고   
생성자를 통해 해당 클래스를 인자로 받아와서 내가 Order 클래스에서 Cart 클래스의 객체를 사용할 거다 하는 뜻인데   

이게 결국 Cart_Function cartFunction = new Cart_Function();   
이랑 같은거고 뭘 쓰든 상관없다고 한다   

>> 내가 방금 실제로 해봤는데   
메소드 안에서는 생성자로 불러오는 방식은 안됨   
클래스 안에서만 필드 멤버로 가능한듯   
메소드 정의 할때 불러오는건 Cart_Function cartFunction = new Cart_Function(); 이런 형태밖에 사용 못함   
그리고 클래스에서도 특정 클래스를 인자로 받아와서 사용하는건 생성자로 하는게 편함   
안그러면 사용하려는 클래스에 인자가 필요하면 그것도 고쳐줘야함 걍 ㅈㄴ 복잡함   

간단하게   
클래스 안에서 특정 클래스의 객체를 사용하고 싶다 = 생성자 생성 후 해당 클래스 매개 변수로 넣어주기   
메소드 안에서 특정 클래스의 객체를 사용하고 싶다 = new 키워드로 인스턴스 생성해주기   

<br>

### DI

Policy 인터페이스 클래스 만듬 -> Rate와 Amount를 인터페이스의 하위 클래스로 만듦   
-> Rate와 Amount의 공통 메소드를 인터페이스 클래스인 Policy로 추출함   
(추출한다는 것이 똑같은 메소드를 시그니처만 만들어 각 클래스에서 각자 지들 필요한 걸로 정의하게 하는 것 < 추상화)   
(이게 강제성을 띈다고는 하는데 결국 지들이 무조건 갖고 있어야되는 공통점을 추출한거긴함 << 엄살임)   
-> Rate와 Amount의 기능을 직접적으로 참조해서 각각 필요한 매개변수 인자를 넣고   
쓰던 Coz와 Kid 클래스는 이제 Rate와 Amount 클래스를 직접 참조할 필요가 없어짐   
-> 왜? Rate와 Amount에서 불러와야하는 기능은 인터페이스인 Policy 클래스에 들어 있기 때문임   
-> 이는 곧 Coz와 Kid 클래스에 있는 Rate와 Amount 변수와 생성자를 제거하고 둘다 Policy의 변수와 생성자만 있음 됨   
-> Policy에 다 들어잇으니까   
-> 그럼 이제 Coz와 Kid 로직은 Rate와 Amount 클래스의 로직을 공통으로 가진 Policy를 참조하게 됨   
-> 그럼? Coz와 Kid의 인스턴스를 생성하여 사용하던 Order 클래스의 makeOrder 메소드는   
걍 간단하게 Coz 인스턴스 생성과 동시에 매개 변수로 각각 필요한 Rate나 Amount를 생성과 동시에 매개변수로 필요한 값   
입력하면됨   
->> 인터페이스를 통한 의존성 주입 DI

결국 인터페이스는 하위 클래스들이 가진 공통점을 추출해 저장하고 그 공통점을 이용하고자하는 또 다른 클래스에 전달을 해주는   
매개체, 통로임

ex)

```java
public class Coz {

private Policy policy;

    public Coz(Policy policy) {
        this.policy = policy;
    }
}
```

Coz 클래스에서 Rate와 Amount 클래스의 인터페이스인 Policy를 참조하여 생성자 생성   

↓   

```java
public void makeOrder() {
    Coz coz = new Coz(new FixedRate(10));
```

makeOrder()에서 참조할 Coz (Policy policy) 에서 매개변수로 new FixedRate가 들어간거   


그럼 Policy 인터페이스에 new FixedRate 가 들어가게되고   
이 뜻은 FixedRate를 Policy에 생성하고 FixedRate 하위 클래스에 정의된 << 다형성   
int calDiscountPrice(int price); 메소드를 불러온다는 것임   

그럼 결국

```java
public int calDiscountPrice (int price) {
    return price - discountAmount;
}
```

이게 돌아가게됨   

이 전체 매커니즘이 DI, 의존성 주입임   


이거 이외에도 Coz와 Kid 클래스에 담겨있던   

```java
checkDiscountCondition()
applyDiscount(int price)
isSatisfied()
```

이걸 인터페이스를 통해 DI 하고싶다면

또 다른 인터페이스를 정의해

이 메소드들을 구체적으로 불러오던 Order에서

private DS_Condition[] dsConditions; 로 메소드가 여러개이기 때문에 담을 배열로 참조하고

생성자에 매개변수로 입력될 DS_Condition을 넣는 작업을 해주면됨

이후 Order을 인스턴스화 해서 쓰던 Kiosk에 매개 변수를 필요한 만큼 추가하면 되는것

ex)

```java
Order_Function orderFunction = new Order_Function(cartFunction,new DS_Condition[]{
new Coz(new FixedRate(10)),
new Kid(new FixedAmount(500))
});
```

이런식으로 간단하게 매개변수 안의 매개변수 안의 매개변수 안의 ...   
왜 상속보다 인터페이스를 많이 쓰는지 알게된 대목 ㄷㄷ


Bug   
새우버거 단품을 선택했는데 세트로 나온다;

버거에서 세트 선택 후 로직 끝나고 다시 단품을 선택하면 세트 선택 구간이 나옴

->> 근데 이거 내가 잘못한게 아닌게 리퍼런스 풀해서 그대로 실행해봤는데 똑같은 문제가 발생함

->> 아마 예외 처리 안하고 객체지향 원리 위주의 실습이라 그런거같은데

다 끝나면 새로 플젝 열어서 풀 한담에 이 부분이랑 사용자 입력 실수 같은 예외 상황 핸들링 좀 해봐야겠음

=
내가 보기에는 세트 선택 끝나고 단품 선택했을 때 다시 세트 선택 구간 나오는 문제가

깊은 복사가 해결법인지   
아님 chooseOption 의 코드를 바꿔줘야하는지   
아님 addtoCart가 문젠지   

이게 감이 안옴

근데 높은 확률로 chooseOption의 문제일 것 같음



객체지향 설계 원칙   
책임 : 객체가 수행해야 할 일 또는 객체가 맡은 임무를 의미   
단일 책임 원칙 : 단일 책임 원칙이라 함은 객체는 오직 하나의 책임만 맡아야 한다는 것   
SRP > 싱글 리스판스빌리티 프린시펄 > 단일 체계 원칙   
->> 즉 한 객체에는 단일 기능에 대한 책임만 지게해야함


### 싱글톤 패턴(Singleton pattern)   
단 하나의 객체만 생성되도록 코드를 작성하는 패턴

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/222973895168)에서 옮겨짐
