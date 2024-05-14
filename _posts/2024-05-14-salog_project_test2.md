---
title: "프로젝트:샐로그 / 테스트 - 회원 2 : 레포지터리 슬라이스 테스트"
author: Jeremiah Lee
date: 2024-05-14
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

저번 컨트롤러 슬라이스 테스트에 이어서 이번에는 레포지터리 슬라이스 테스트를 진행했다.

레포지터리는 기본 기능인 CRUD에 대한 테스트는 진행하지 않았고, 구현된 쿼리 메서드에 대한 테스트를 진행했다.

아무래도 CRUD는 JPA의 "기본" 기능이기 때문에 테스트를 진행해야할 정도로 문제가 있을 것이라 생각이 들지 않았기 때문이다.

<br>

회원 레포지터리에는 세 개의 쿼리 메서드가 있는데, 전부 조회에 해당하는 메서드이다.

<br>
<br>
<br>

### 레포지터리 슬라이스 테스트 수행 이유

무엇을 하든 해당 작업을 수행하는 이유와 목적은 중요하다.

그래서 가장 먼저 이 테스트를 진행하는 이유를 먼저 간단하게 알아보고 가자.

순서대로 레포지터리 슬라이스 테스트를 수행하는 이유는 다음과 같다.

1. 데이터 액세스 레이어 검증
   - 데이터베이스 연산이 예상대로 동작하는지 확인하기 위함이다.
2. 격리된 테스트
   - 컨트롤러, 서비스 등으로 부터 격리되어 독립적으로 실행, 문제의 원인을 빠르고 쉽게 파악할 수 있다.
3. 빠른 피드백
   - 다른 슬라이스 테스트도 마찬가지이지만 특정 레이어만 로드하여 실행하기 때문에 테스트가 빠르고, 개발자가 피드백을 빨리 얻을 수 있다.

<br>
<br>
<br>

### 사전 작업

회원 레포지터리 테스트 케이스 작성과 관련된 내용에 진입하기 전에 한 가지 체크해둬야할 부분이 있다.

앞선 컨트롤러 테스트에서도 마찬가지로 살펴봤지만 이번에도 애너테이션 부터 체크하고 들어가야 한다.

<br>

기본적으로 스프링부트에서는 레포지터리 슬라이스 테스트를 진행하기 쉽게하는 애너테이션이 존재한다.

바로 "@DataJpaTest" 이다.

이 애너테이션은 JPA 관련 컴포넌트만 로드하고, 내장된 H2 DB를 사용하여 테스트를 수행할 수 있게 환경을 조성한다.

이를 통해 레포지터리 슬라이스 테스트를 효율적으로 수행할 수 있게 된다.

<br>

여기서 만약 내장된 H2 DB가 아닌 실제 사용 중인 DB 설정을 테스트 환경에 입히고 싶다면

"@AutoConfigureTestDatabase"

이 애너테이션을 사용할 수 있다.

<br>

해당 애너테이션에는 두 가지 옵션이 있다.

```java
@AutoConfigureTestDatabase(replace = Replace.ANY)

@AutoConfigureTestDatabase(replace = Replace.NONE)
```

첫 번째 ANY 옵션은 생략 가능한 default 옵션이다.

즉, 스프링부트가 테스트를 위해 임베디드 DB(H2 등)를 자동으로 구성하게 된다.

두 번째의 NONE 옵션은 자동으로 DB를 구성하지 않고 실제 애플리케이션이 사용하는 데이터베이스 설정을 그대로 사용한다.

다시 말해, Salog와 같은 경우 실제 애플리케이션에 설정된 DB인 MySQL을 사용하게 된다.

이를 통해 실 서비스 환경에서도 레포지터리 슬라이스 테스트를 진행해 볼 수 있다.

다만 테스트 환경에서 실제 DB와 연결이 되게 설정 해야 한다.

<br>

현재는 테스트 할 항목이 적고, 비교적 복잡하지 않은 로직을 테스트 해보는 상황에 맞게 H2를 자동 구성하여 테스트를 진행했다.

임베디드 DB가 아닌 실제 DB를 목적에 맞게 테스트 해보기 위해서는 로컬 MySQL 보다는 RDS와 연결 해주는 것이 맞다고 보는데,
빠르게 진행하여 피드백을 얻어야 하는 이 테스트와 맞지 않다고 생각하여 내장 DB를 사용했다.

<br>
<br>
<br>

### 회원 레포지터리 테스트

테스트 케이스의 작성 방식은 세 개가 전부 동일하다.

그 중 두 번째 테스트 케이스의 경우 boolean 값을 리턴해야하는 것 빼고는 결국 특정 회원을 찾는 것이 메서드의 주목적이기 때문에 
예상 응답 값도 동일하다.

그렇기 때문에 전부를 보지 않고 email로 회원 조회와 존재하는 email인지 체크하는 로직을 테스트한 케이스만 살펴볼 것이다.

<br>

첫 번째로 Eamil을 기준으로 회원을 조회하는 쿼리 메서드에 대한 테스트 케이스이다.

```java
    @DisplayName("findByEmail")
    public void findByEmailTest() {
        // given
        Member member = new Member();
        member.setEmail("test@email.com");
        memberRepository.save(member);

        // when
        Optional<Member> findMember = memberRepository.findByEmail("test@email.com");

        // then
        assertThat(findMember).isPresent();
        assertThat(findMember.get().getEmail()).isEqualTo("test@email.com");
    }
```

H2 DB에 테스트용 회원을 입력하고서 쿼리메서드를 호출한다.

이후 해당 회원이 존재하는지, 입력된 Email과 일치하는 지를 확인했다.

<br>

다음으로는 boolean 값이 리턴되어야 하는 쿼리 메서드이다.

```java
    @Test
    @DisplayName("existsByEmail")
    public void existsByEmailTest() {
        // given
        Member member = new Member();
        member.setEmail("test@email.com");
        memberRepository.save(member);

        // when
        Boolean exists = memberRepository.existsByEmail("test@email.com");

        // then
        assertThat(exists).isTrue();
    }
```

마찬가지로 테스트용 회원을 먼저 입력하고 저장, 쿼리메서드를 실행 시켜보고
원하는 값을 도출하는지를 확인한다.

<br>

이 두 쿼리메서드 보다 복잡한 로직이 없어서 이 정도로 끝났다.

사실 이렇게만 해도 예상대로 값이 출력되는지 확인할 수 있기 때문에 굳이 복잡하게 작성할 필요가 없다.

<br>
<br>
<br>

***

### 후기

이로써 회원 파트의 두 레이어에 대한 슬라이스 테스트가 끝났다.

다음으로는 회원 서비스 레이어의 유닛 테스트를 진행해 볼 생각이다.

서비스 로직은 각각의 로직에 대한 테스트가 중요하기 때문에 별도로 슬라이스 테스트는 진행하지 않는다.
