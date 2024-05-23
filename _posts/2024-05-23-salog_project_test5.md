---
title: "프로젝트:샐로그 / 테스트 - 수입 2 : 레포지터리 슬라이스 테스트"
author: Jeremiah Lee
date: 2024-05-23
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

이번에 작성할 내용은 그리 많지 않다.

단순히 데이터 넣고, 쿼리 메서드 실행 시켜보고, 예상 결과가 도출되는 지에 대한 테스트이기 때문에 복잡한 내용은 없다.

테스트를 진행하기 전에 어떻게 했는 지와 하나의 테스트 케이스를 살펴보며 어떤 식으로 진행했는 지 서술할 것이다.

<br>
<br>
<br>

### 사전 작업

우선 이번 테스트의 경우, 목업 데이터가 필요하다.

기본적으로 쿼리 메서드에 대한 테스트를 진행하여 해당 쿼리 메서드가 정상적으로 동작하는 지를 확인 해야 하기 때문에 사전에 데이터를 미리 정의해 두어야 한다.

```java
public class IncomeRepositoryTest {
    @Autowired
    private IncomeRepository incomeRepository;
    @Autowired
    private MemberRepository memberRepository;
    @Autowired
    private LedgerTagRepository ledgerTagRepository;

    private Member member;
    private LedgerTag ledgerTag;
    private Income income;

    // given
    @BeforeEach
    void setUp() {
        member = new Member();
        member.setEmail("test@email.com");
        member.setEmailAlarm(true);
        member.setHomeAlarm(true);
        memberRepository.save(member);

        ledgerTag = new LedgerTag();
        ledgerTag.setTagName("testTag");
        ledgerTag.setCategory(LedgerTag.Group.INCOME);
        ledgerTag.setMember(member);
        ledgerTagRepository.save(ledgerTag);

        income = new Income();
        income.setMoney(10000);
        income.setMember(member);
        income.setDate(LocalDate.of(2024, 1, 1));
        income.setLedgerTag(ledgerTag);
        incomeRepository.save(income);
    }
```

위가 @BeforeEach를 통해 목업 데이터를 정의해 둔 코드인데, 회원과 태그도 마찬가지로 레포지터리를 DI 주입하여 의존하고 있다.

이는 수입과 각 도메인의 "관계" 때문이다.

슬라이스 테스트라고는 해도 여전히 엔티티 간 연관관계는 중요하다.

그래서 각각 회원, 태그의 필요한 정보를 담아주고 수입 엔티티와 관계를 맺은 것이다.

이를 통해 데이터 무결성과 쿼리 로직을 검증할 수 있게 된다.

<br>

이번 테스트를 진행하는 목적을 생각하면 된다.

이 수입 레포지터리의 쿼리 메서드는 "조회"에 해당한다.

조회에는 회원, 태그 간 관계가 올바르게 설정되어 있는 지를 포함한다.

즉, 올바르게 설정이 되어 있어야 쿼리가 정확한 결과를 반환할 수 있는 것이고, 이것이 "정상" 작동한다는 것이다.

그렇기에 실 서비스와 마찬가지로 엔티티간 관계를 연결하고 필요한 데이터를 목업하는 것이다.

<br>
<br>
<br>

### 수입 레포지터리 슬라이스 테스트

한 가지 테스트에 대해 살펴보자.

```java
    @Test
    @DisplayName("findByMonthAndTag")
    @Order(1)
    void findByMonthAndTagTest() {
        // when
        Page<Income> result = incomeRepository.findByMonthAndTag(member.getMemberId(), 2024, 1, "testTag", PageRequest.of(0, 5));

        // then
        assertThat(result.getContent()).isNotEmpty();
        assertThat(result.getContent().get(0).getMember()).isEqualTo(member);
        assertThat(result.getContent().get(0).getLedgerTag().getTagName()).isEqualTo("testTag");
    }
```

테스트 케이스 내부 로직에 대해서는 쉽게 파악 할 수 있다.

앞서 목업 데이터를 @BeforeEach로 미리 정의해 두었기 때문에 그 내용이 Given에 해당하고,
쿼리 메서드를 실행하는 부분이 When, 실행 후 어떤 결과가 도출될 것인지가 Then이다.

즉, 이 테스트는 수입 레포지터리의 findByMonthAndTag 쿼리 메서드를 실행하면
반환된 결과가 비어있지 않고, 동일한 member와 tag를 가지는 지에 대해 확인된다.

나머지 테스트도 이와 비슷한 방식으로, 실행하고, 결과 도출이 끝이다.

<br>
<br>
<br>

***

### 후기

이번 테스트는 내용이 적어서 별 볼일 없다고 생각할 수 있으나 사실 당장 정상적으로 동작하는 가를 중점으로 빠르게 작성하여 진행한 것이기 때문에
그런 것이고, 이 외에도 할 수 있는 테스트가 많다.

이 테스트 뿐만 아니라 다른 테스트도 마찬가지다.

month에 0이나 13이 들어가도록 경계값 분석을 해볼 수도 있고, 다량의 데이터가 입력되었을 때 해당 쿼리의 실행시간이 어떻게 되는지에 대해 성능 테스트를 해볼 수도 있고,
일부러 에러를 발생시켜 원하는 코드를 뱉어내는지도 테스트 해볼 수 있다.

다만 아직까지 테스트에 익숙해 지는 단계이고, 수입 관련 나머지나 고정 수입, 예산에 관한 테스트 케이스도 작성 해야 하기 때문에
빠르게 작성하여 "동작"하는 지에 대해서만 테스트 한 것이다.

정말 기본적인 테스트라 볼 수 있다.

<br>

아무튼 이어서 나머지 테스트를 진행한 다음 다시 돌아와 테스트 유지보수도 진행해보자.
