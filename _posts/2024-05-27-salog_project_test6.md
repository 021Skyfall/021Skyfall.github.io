---
title: "프로젝트:샐로그 / 테스트 - 수입 3 : 서비스 유닛 테스트 (private 메서드에 대한 테스트)"
author: Jeremiah Lee
date: 2024-05-27
categories: [ 프로젝트, 샐로그, 테스트 ]
tags: [프로젝트, 샐로그, 테스트]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Project_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

### 개요

처음에는 회원 서비스 단위 테스트와 비슷한 맥락으로 진행되기 때문에 크게 기록할만한 내용이 없을 거라 생각했는데, 한 가지 문제가 있었다.

그래서 해당 문제에 대해 살펴보고자 이번 포스트를 작성한다.

<br>

우선 회원은 서비스 레이어에서만 사용하는 private 메서드가 없다.

모두 회원 컨트롤러 레이어나 다른 서비스 레이어에서 회원에 대한 검증을 하기 위해 의존하고 있기 때문에 private 메서드는 불필요하다.

물론 내부적으로 복잡한 로직에 의해 연산 과정을 거쳐야 하는 경우 생겨날 수 있겠지만 아직까지는 없다.

<br>

그러나 앞서 진행했던 회원 서비스 로직 단위 테스트와 달리 수입 파트에서는 서비스 로직 중 private 접근제어자를 가진 메서드가 있다.

이 메서드는 태그와 관련된 약간 복잡한 로직인데, 이 로직 자체가 다른 레이어에 간섭할 일이 없기 때문에 접근을 제한해 두었다.

그래서 이러한 캡슐화된 메서드에 대해서는 어떻게 테스트를 진행해야할까? 가 주요한 문제였다.

<br>
<br>
<br>

### private 메서드는 테스트 해야할까?

당연히 private 메서드도 테스트의 해야한다.

엄밀히 말하면 private 메서드의 로직이 주어진 임무를 잘 수행하는 지 테스트 되어야 한다.

하지만 테스트 클래스에서는 private 메서드에 접근할 수 없다.

그렇다면 어떻게 해야할까?

<br>

이를 해결하기 위한 방법이 두 가지가 있다.

<br>
<br>

#### 접근제어자를 protected 혹은 package-private 로 변경

이 방법이 가장 간단하다.

단순히 private 으로 지정된 메서드의 접근제어자를 protected 혹은 package-private 로 변경하고 테스트 클래스를 같은 패키지로 옮기는 것이다.

protected 는 동일 패키지 내의 모든 클래스에서 접근 가능하고 특히, 다른 패키지의 상속 받은 서브 클래스에서 접근 가능하게 하는 접근제어자이다.

마찬가지로 package-private은 동일 패키지 내의 모든 클래스에서 접근 가능하지만 protected 와 달리 상속 간 관계 없이 다른 패키지면 접근이 불가하다.

<br>

그런데 테스트 클래스가 있는 패키지 외에 다른 패키지에서 접근할 필요가 없고 캡슐화를 지키기 위해서 이 두 접근제어자는 사용할 수 없다.

<br>
<br>

#### 자바 리플렉션

이 경우 리플렉션을 사용할 수 있다.

리플렉션은 런타임에 클래스, 인터페이스, 메소드, 필드 등을 동적으로 검사하고 조작할 수 있는 기능이다.

즉, 컴파일 시점에는 알 수 없는 클래스의 정보에 접근하거나, 객체 생성, 호출, 수정 등이 가능하다.

<br>

이를 통해 private 메서드에 대한 캡슐화를 유지하고 private 메서드를 직접 테스트할 수 있다.

그러나 호출이 빈번할 경우 일반적인 호출보다 성능이 저하될 수 있으며 가독성이 떨어지고 결과적으로는 
private한 메서드에 직접 접근하게 되므로 보안 문제나 캡슐화 위배 문제가 생길 수 있다.

<br>

우선적으로 수입 테스트에서 이 리플렉션을 사용해 보았다.

```java
    @Test
    @DisplayName("tagHandler 1 : tag가 null이 아니고, 이미 존재하는 경우")
    @Order(18)
    void tagHandler1() throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {
        // given
        String incomePostDto = "existTag";
        String token = "testToken";

        LedgerTag existTag = new LedgerTag();
        existTag.setTagName("existTag");
        existTag.setCategory(LedgerTag.Group.INCOME);
        existTag.setMember(member);

        when(tagService.findLedgerTagByMemberIdAndTagName(token, incomePostDto, LedgerTag.Group.INCOME))
                .thenReturn(existTag);
        when(incomeRepository.save(income)).thenReturn(income);

        // 리플렉션을 사용하여 private 메서드 호출
        Method method = incomeService.getClass().getDeclaredMethod("tagHandler", String.class, String.class, Income.class);
        method.setAccessible(true); // private 메서드 접근 가능하게 설정

        // when
        Income result = (Income) method.invoke(incomeService, incomePostDto, token, income);

        // then
        assertNotNull(result.getLedgerTag());
        assertEquals("existTag", result.getLedgerTag().getTagName());
        assertEquals(LedgerTag.Group.INCOME, result.getLedgerTag().getCategory());
        assertEquals(member, result.getLedgerTag().getMember());

        verify(tagService, times(1)).findLedgerTagByMemberIdAndTagName(token, incomePostDto, LedgerTag.Group.INCOME);
        verify(incomeRepository, times(1)).save(income);
    }
```

이 코드에서 볼 수 있듯이 tagHandler에 대해 접근 가능하게 설정하여 테스트하고 있다.

그런데, 명칭을 고정해야하기 때문에 추후 메서드의 이름이 달라진다면 여기로와서 수정해야한다.

<br>

이러한 장점과 단점을 가진 두 가지 방법 외에 내가 정답이라고 생각하는 세 번째 방법이 있다.

<br>
<br>

#### private 메서드를 호출하는 public 메서드를 통해 테스트

세 번째는 private 메서드를 직접적으로 테스트 하지 않는 것이다.

테스트 해야만 한다와 모순되지만 "직접"이 포인트이다.

굳이 private 으로 접근 제한을 둔 메서드 그 자체를 테스트할 필요는 없다.

"메서드"를 테스트하는 것이 아니라 "로직"을 테스트하는 것이기 때문에 약간 관점을 바꿔 이 private 메서드를 활용하고 있는, 의존하고 있는 public 메서드에서
테스트를 진행하면 된다.

이렇게 하면 캡슐화와 설계 원칙을 저해할 수 있는 접근제어자 변경 혹은 유지보수가 어려운 리플렉션을 거치지 않고 private 메서드 로직을 테스트할 수 있게 된다.

```java
    @Test
    @DisplayName("createIncome + tagHandler 1 : 태그가 null이 아니고, 새로 생성하는 경우")
    @Order(1)
    void createIncomeTest1() {
        // given
        String testToken = "testToken";
        String tagName = "testTag";

        IncomeDto.Post postDto = new IncomeDto.Post();
        postDto.setIncomeTag(tagName);
        IncomeDto.Response responseDto = new IncomeDto.Response();

        ledgerTag.setTagName(tagName);

        when(incomeMapper.incomePostDtoToIncome(postDto)).thenReturn(income);
        when(memberService.findVerifiedMember(1L)).thenReturn(member);
        when(jwtTokenizer.getMemberId(testToken)).thenReturn(1L);
        
        // tagHandler
        when(tagService.findLedgerTagByMemberIdAndTagName(testToken, tagName, LedgerTag.Group.INCOME)).thenReturn(null);
        when(tagService.postLedgerTag(eq(testToken), any(LedgerTagDto.Post.class))).thenReturn(ledgerTag);
        
        when(incomeRepository.save(income)).thenReturn(income);
        when(incomeMapper.incomeToIncomeResponseDto(income)).thenReturn(responseDto);

        // when
        IncomeDto.Response result = incomeService.createIncome("testToken", postDto);

        // then
        assertNotNull(result);
        assertEquals(income.getMember(), member);
        assertEquals(ledgerTag, income.getLedgerTag());
        assertEquals(responseDto, result);

        verify(incomeMapper, times(1)).incomePostDtoToIncome(postDto);
        verify(jwtTokenizer, times(1)).getMemberId("testToken");
        verify(memberService, times(1)).findVerifiedMember(1L);
        verify(tagService, times(1)).postLedgerTag(eq(testToken), any(LedgerTagDto.Post.class));
        verify(incomeRepository, times(1)).save(income);
        verify(incomeMapper, times(1)).incomeToIncomeResponseDto(income);
    }
```

위 코드와 같이 tagHandler 가 실행되었을 경우, 리턴되어야하는 것을 지정해주고, 이에 대한 결과를 검증하면 결국은 tagHandler 가 실행되는 것과 다름 없다.

즉, 이 방법을 통해 원본 tagHandler를 변경하거나 별다른 성능 저하 없이 로직에 대한 테스트가 가능하다.

<br>

이렇게 private 메서드 자체를 테스트하려는 상황은 거의 나오지 않는다고 한다.

그도 그럴게, private 메서드는 해당 클래스 내에서만 호출이 되어야 하고 그러면 해당 클래스 내의 public 또는 protected 메서드가 호출하고 있다는 의미가 된다.

이에 대한 내용은 [망나니 개발자 블로그](https://mangkyu.tistory.com/235)에 간결히 나와있으니 이해 안 갈 때 한 번씩 체크하자.

<br>
<br>
<br>

### 후기

회원 서비스 파트와 거의 동일한 느낌으로 별 다를거 없이 진행될 줄 알았는데 의외의 복병이 숨어있었다.

이 문제를 해결하기 위해 꽤 시간이 흘렀고 생각을 많이 했다.

이후, 고정 수입과 예산 도메인에 대한 테스트 케이스 작성을 진행할 것인데, private 메서드를 테스트하고 싶다면
생각을 바꿔 해당 메서드를 호출하는 public 메서드에서 진행하자.
