---
title: "프로젝트:샐로그 / 테스트 - 회원 3 : 서비스 유닛 테스트"
author: Jeremiah Lee
date: 2024-05-17
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

이번에는 회원 서비스 레이어의 유닛 테스트이다.

단위 테스트인 만큼 서비스 레이어에 있는 모든 메서드의 로직을 테스트했으며, 분기가 있는 로직은 상황에 맞게 모든 분기에 대해 테스트 케이스를 정의했다.

아무래도 서비스 로직인 만큼 분기도 많고 상황에 따른 메서드가 많기 때문에 시간이 좀 오래걸렸다.

<br>

모든 테스트 케이스를 정의했지만 블로그에는 몇 가지를 추려야 할 것 같다.

전부 다 쓰기에는 테스트 케이스가 수 십개가 있기 때문에 읽는 사람도, 작성하는 나도 지칠 것이다.

<br>

기본적으로 어떻게 진행했는 지, 어떤 분기가 있었는 지, 어떤 어려움이 있었는 지에 대해 작성할 생각이다.

또한, 아직 리펙터링을 거치지 않았기 때문에 중복되는 내용이 어느 정도 있을 것이다.

처음에는 작성 중에 중복되는 내용 추출해서 접근하려고 했는데,
아무래도 본격적으로 테스트 케이스를 정의하는 것이 처음이다 보니 오히려 헷갈려 그냥 하드 코딩으로 작성했다.

추후 리펙터링 후 깔끔하게 정리할 생각이지만 해당 내용에 대해 블로깅할 예정은 없다.

<br>
<br>
<br>

### 사전 작업

우선, 서비스 레이어의 유닛 테스트를 위해 적절한 테스트 애너테이션을 적용해야 한다.

```java
@ExtendWith(MockitoExtension.class)
@TestMethodOrder(MethodOrderer.OrderAnnotation.class) // 순서보장 (cf. https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-execution-order)
@DisplayName("회원 서비스 유닛 테스트")
public class MemberServiceTest {
  ...
}
```

유닛 테스트를 진행하기 위한 애너테이션은 @ExtendWith 애너테이션이다.

이 @ExtendWith 애너테이션은 JUnit5 에서 Mockito 프레임워크를 사용하기 위해 필요한 설정이다.

Mockito는 자바 단위 테스트 작성을 위해 사용되는 모의 객체 프레임워크이다.

즉, 실제 객체를 사용하지 않고 모의 객체로 대체하여 테스트 환경에서 단위 테스트를 해볼 수 있게 해주는 기술이다.

더 쉽게 말하면, 의존성을 관리할 필요가 없어지고, 실제 애플리케이션 환경으로 부터 격리시키기 때문에 서비스 자체가 꼬일 문제가 없다.

```java
public class MemberServiceTest {
  @InjectMocks
  private MemberService memberService;
  @Mock
  private JwtTokenizer jwtTokenizer;
  @Mock
  private MemberRepository memberRepository;
  @Mock
  private PasswordEncoder passwordEncoder;
  @Mock
  private CustomAuthorityUtils authorityUtils;
  @Mock
  private MemberMapper memberMapper;
  @Mock
  private EmailSender emailSender;
  @Mock
  private RandomGenerator randomGenerator;
    
  ...
}
```

이런 식으로 @Mock 애너테이션을 활용해 원본 회원 서비스 클래스가 의존하는 객체들을 모의 객체로 격리 후 활용할 수 있다.

테스트 대상인 MemberService에 적용된 @InjectMocks 애너테이션은
테스트 대상 객체를 생성하고 앞서 @Mock으로 설정한 모의 객체들을 주입하는 역할을 한다.

<br>

다음으로 @TestMethodOrder 애너테이션은, 말 그대로 테스트 메서드들의 순서 보장을 위한 애너테이션이다.

처음에는 단순히 테스트 메서드들에 @Order 애너테이션을 적용하면 순서를 내가 지정할 수 있을 줄 알았는데 junit 공식 문서를 살펴보니
테스트 클래스에 해당 애너테이션을 적용해야 @Order 애너테이션 동작 한다고 했다.

해당 내용을 참조하여 테스트 실행 순서를 내가 원하는 대로 설정하기 위해 적용했다.

[해당 내용 junit 공식 문서](https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-execution-order)을 참조했다.

<br>

@DisplayName 애너테이션은 실행 결과 콘솔 창에 테스트 명칭을 표시해준다.

<br>
<br>
<br>

### 회원 서비스 단위 테스트

사전 작업을 거쳐 회원 서비스 레이어에 대한 단위 테스트를 진행했다.

진행 방식은 사실상 전부 비슷하다.

Given -> 테스트 전 준비

When -> 테스트 대상 실행

Then -> 이후 어떻게 되어야 하는가

이 세 가지를 염두에 두고 진행하면 그 복잡한 테스트도 의외로 쉽게 핸들링 가능하다.

<br>

첫 번째로 가장 기본적인 회원 가입 로직에 대한 테스트 케이스를 살펴보자.

```java
@Test
@DisplayName("createMember")
@Order(1)
void createMemberTest() {
  // Given
  MemberDto.Post postDto = new MemberDto.Post();
  postDto.setEmail("test@email.com");
  postDto.setPassword("password");

  member.setEmail("test@email.com");
  member.setPassword("password");

  when(memberMapper.memberPostDtoToMember(postDto)).thenReturn(member);
  when(passwordEncoder.encode("password")).thenReturn("encodedPassword");
  when(authorityUtils.createRoles("test@email.com")).thenReturn(List.of("ROLE_USER"));
  when(memberRepository.save(any(Member.class))).thenReturn(member);

  // When
  memberService.createMember(postDto);

  // Then
  verify(memberRepository, times(1)).save(member);
  assertEquals("test@email.com", member.getEmail());
  assertEquals("encodedPassword", member.getPassword());
  assertEquals(List.of("ROLE_USER"), member.getRoles());

  // logs
  logger.info("Member Email: {}", member.getEmail());
  logger.info("Member Password: {}", member.getPassword());
  logger.info("Member Roles: {}", member.getRoles());
}
```

전체 코드는 위와 같다.

순서대로 Given / When / Then으로 진행된다.

Given 파트에서는 테스트에 필요한 준비물을 챙긴다.

회원 가입 시 post dto를 통해 회원에 대한 정보를 받아오고, 이 정보를 바탕으로 데이터베이스에 저장이 이루어진다.

그렇기 때문에 MemberDto 클래스의 Post 이너클래스에 필요한 내용을 먼저 생성한다.

또한 이 DTO를 java 객체인 Member로 변환해주어야 하기 때문에 Member 객체에 별도로 동일한 내용을 설정한다.

<br>

다음으로 when 함수의 경우 어떠한 로직이 실행되면 어떤 것이 리턴되어야 하는 것에 대한 정의다.

순서대로 memberMapper가 실행되어 DTO 객체가 전달되면 자바 객체인 member 가 리턴되어야 하고,
회원의 비밀번호는 인코딩되어 인코딩된 비밀번호가 리턴되어야 하고,
보안을 위한 회원의 인가 역할 생성 시 특정 역할을 리턴해야하고, 
해당 내용을 데이터베이스에 저장 시 해당 회원이 리턴되어야 한다는 내용이다.

즉, 미리 이 테스트 대상 메서드가 실행되었을 경우 로직이 실행되면 어떤 것이 리턴되어야 하는지 가상으로 전부 설정해준 셈이다.

<br>

이후, when은 테스트 대상이 실행되는 시점이다.

위의 케이스에서 when에 해당하는 테스트 대상이 실행되면 given에 설정된 내용들이 리턴된다.

<br>

마지막으로 then은 "결과"에 대한 것이다.

그래서 결과가 어떻게 나와야 하는가?에 대한 내용으로, 개발자가 예상한 결과가 도출되는지 확인하는 단계이다.

이 테스트 대상의 목표를 생각하면 된다.

현재는 회원 가입 메서드가 요청 시 한번 만 실행되는 지, 저장된 내용이 원하는 내용과 일치하는 지 체크한다.

<br>

이제 이 테스트 케이스를 실행했을 경우 통과가 되면 테스트가 완료된 것이다.

given / when / then 뿐만 아니라 대상의 목적과 실행 흐름을 항상 생각해야 한다.

<br>
<br>

다음으로는 분기마다 검증해야 하는 테스트 케이스의 경우를 살펴보자.

우선 원본 로직은 다음과 같다.

```java
    // 존재하는 이메일인지 체크
    public void isExistEmail(String email) {
        if (memberRepository.existsByEmail(email))
            throw new BusinessLogicException(ExceptionCode.EMAIL_EXIST);
    }
```

아주 간단하고, 노출되도 괜찮은 로직을 갖고 왔다.

이 메서드는 존재하는 이메일이 없는 경우, 있는 경우 두 가지 상황을 생각해야한다.

단순히 따져봐도 if 로 조건을 분기하고 있다. 

다시 말하면, 이러한 조건이 있는 로직은 각 상황에 맞는 테스트 케이스를 작성하고 검증 해야한다는 뜻이다.

<br>

그렇다면 두 가지 상황에 대한 검증은 어떻게 이루어져야 할까?

```java
    @Test
    @DisplayName("isExistEmail 1 : 이메일 중복 X")
    @Order(12)
    void isExistEmailTest1() {
        // Given
        String email = "test@email.com";

        when(memberRepository.existsByEmail(email)).thenReturn(false);

        // When & Then
        assertDoesNotThrow(() -> {
            memberService.isExistEmail(email);
        });
    }
```

간단하다.

위와 같이 이메일이 존재하지 않는 경우 요청 시 원하는 대로 예외가 발생되지 않으면 검증 성공이다.

```java
    @Test
    @DisplayName("isExistEmail 2 : 이메일 중복 O")
    @Order(13)
    void isExistEmailTest2() {
        // Given
        String email = "test@email.com";

        when(memberRepository.existsByEmail(email)).thenReturn(true);

        // When & Then
        assertThrows(BusinessLogicException.class, () ->{
            memberService.isExistEmail(email);
        });
    }
```

반대의 경우도 마찬가지로 이메일이 존재하는 경우에는 단순히 예외를 발생시키는 지 검증하면 된다.

<br>
<br>

이런 식으로 또 다른 조건 분기가 존재하는 메서드를 핸들링하면 된다.

하나의 메서드에 여러 상황에 따른 조건이 있는 경우, 상황 하나 당 검증을 시도하면 된다.

그렇게 정의된 테스트 케이스의 대부분, Given은 동일하되 When, Then에서 차이가 있을 수 있지만 기본적으로 가장 처음 염두에 둔 상황에 대한 테스트 케이스를
정의하고 나면 나머지는 크게 어렵지 않다.

<br>
<br>
<br>

### 어려웠던 점

단위 테스트에서 가장 어려웠던 것은 이 테스트 대상이 어떤 흐름을 가지고 어떤 리턴 값을 출력해야하는 지 항상 생각해야 하는 것이 었다.

Given, When, Then으로 나누어서 그나마 낫긴 했지만, 조금이라도 분기가 많거나 검증되어야 하는 내용의 양이 많아지면 엄청 헷갈렸다.

테스트 케이스 작성에 대해서는 모든 메서드를 외우고 있는 것이 아니기 때문에 어떤 검증 메서드가 적절할 지도 많이 찾아보았다.

결국, 대부분의 테스트 케이스가 비슷한 흐름과 비슷한 메서드를 사용하는 것을 알게 된 후 부터는 안정적으로 작성할 수 있었지만

처음에는 흐름도 잘 몰라, 메서드도 잘 몰라, 대상이 어떤 검증을 거쳐야하는 지 몰라... 등등 복잡하게 생각한 경우가 많았다.

<br>

특히 테스트 대상이 어떤 데이터를 가지고 어떤 검증을 거쳐야 하는 지에 대한 어려움은 TDD 방식이나 메서드 하나를 완성하고 테스트를 작성했으면
좀 덜 했을 것 같다.

이미 다 짜여진 로직에 대해 뒤늦게 테스트 케이스를 작성하려 하니 이런 고충이 있는 느낌이다.

다음부터는 로직 하나 당 테스트 케이스 한 개라는 규칙을 세우고 최대한 맞춰가 보자는 생각이 들었다.

<br>
<br>
<br>

***

### 후기

내가 배분을 잘 못한 탓에 실질적인 후기가 어려웠던 점 파트에 포함되어 버렸다.

한 번 더 상기하고 가자면, 앞으로는 로직 하나가 완성되면 해당 로직 대상으로 테스트 케이스를 작성하는 습관을 들이자.

그렇게 테스트 케이스 작성에 익숙해지면 TDD 방식으로도 진행해볼 수 있으리라 믿는다.

<br>

또한, 테스트 케이스 작성 시에는 이 대상이 어떤 데이터를 가지고 어떤 검증을 거쳐야하는 지 항상 생각하자.

지금 껏 했던 Given, When, Then을 항상 생각하자.

<br>

이제 회원 서비스 단위 테스트도 마무리가 되었고, 다음으로는 수입 파트의 테스트 케이스를 작성할 예정이다.
