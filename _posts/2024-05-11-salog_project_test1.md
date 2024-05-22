---
title: "프로젝트:샐로그 / 테스트 - 회원 1 : 컨트롤러 슬라이스 테스트"
author: Jeremiah Lee
date: 2024-05-11
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

이전 [테스트 개념과 종류 포스트](https://021skyfall.github.io/posts/SW_test_1/) 에서 한달이 지났다.

본래 계획으로는 정보처리기사 실기를 맛보는 정도로만 생각하고 치루려 했는데, 이왕 시험 보는거 그냥 합격하자 생각하고 임했다.

그렇게 한주는 시험에 대한 고민과 컨트롤러 슬라이스 테스트 케이스 작성으로, 나머지 3주 정도는 정보처리기사 시험에 시간을 투자했고
아직 결과는 한달 넘게 남았지만 긍정적으로 생각 중이다.

그래서 다시 원점으로 돌아와 새로운 프로젝트를 하기보다는 아쉬웠던 점이 많았던 샐로그 프로젝트를 유지보수하기로 마음을 잡았고,
그 첫 번째 발걸음이 테스트이다.

내가 담당했던 파트를 하나 씩 차근차근 컨트롤러 슬라이스부터 시작해서 서비스 단위, 전체 통합, 성능 테스트까지 진행해볼 예정이며, 더 나아가 성능 개선, 코드 품질 개선,
리팩토링, 기능 추가 등 프로젝트 진행 당시에 진행하지 못했던 작업들을 처리하며 샐로그 프로젝트를 좀 더 풍성하게 만들어볼 생각이다.

<br>

모든 작업에 대한 코드는 깃헙 샐로그 레포에 있으며,    
금전적인 목적이 있는 프로젝트가 아닌 학습용 프로젝트이기 때문에 소스는 항상 공개 중이다.

<br>

이번 포스트에서는 가장 간단한 회원 컨트롤러의 CRUD에 대한 테스트 케이스에 대해 작성한다.

<br>
<br>
<br>

### 회원 컨트롤러

회원 컨트롤러에는 10개의 메서드가 있다.
 
슬라이스테스트가 목적이기 때문에 CRUD를 포함해 이 10개의 함수에 대한 테스트를 진행했다.

<br>
<br>
<br>

### 회원 컨트롤러 슬라이스 테스트

회원 컨트롤러에 대한 슬라이스 테스트는 요청을 보냈을 때 이 URL로 통신이 잘 되는 지와 예상하고 있는 HTTP 상태값이 응답되는지
를 주요 목적으로 두고 작성했다.

그렇기 때문에 Mock으로 필요한 의존 객체들을 주입 해주었고, 가짜 사용자를 만들어 각 기능을 테스트 해보았다.

<br>

이번 포스트에서는 CRUD만 살펴보자.

<br>
<br>
<br>

### 사전 작업

테스트 시작 전, 두 가지 핸들링해야할 문제가 있다.

첫 번째는 원본 회원 컨트롤러가 의존하고 있는 객체에 대한 의존성 주입이고, 하나는 스프링 시큐리티 필터를 끄는 것이다.

전자는 테스트를 위해 내부적으로는 스프링이 정상 실행되어야 하기 때문에 필요하고,
후자는 보안 관련 테스트가 아닌 기능에 대한 테스트이기 때문에 테스트를 간단히 하기 위해 필요했다.

```java
@WebMvcTest(value = MemberController.class)
@AutoConfigureMockMvc(addFilters = false) // 시큐리티 필터 끔
@DisplayName("회원 컨트롤러 유닛 테스트")
public class MemberControllerTest {
    @Autowired
    private MockMvc mockMvc;
    @Autowired
    Gson gson = new Gson();
    @MockBean
    private MemberService memberService;
    @MockBean
    private TokenBlackListService tokenBlackListService;
    @MockBean
    private SecurityConfiguration securityConfiguration;
```

이와 같이 애너테이션으로 이 테스트 내에서 필터를 끄고, 사용하지 않아도 원본이 의존하고 있거나 스프링 자체 실행에 필요한 객체를 mock 
객체로 주입했다.

<br>
<br>
<br>

### 테스트 케이스 작성

일부 테스트 케이스는 통신을 위해 데이터가 필요했기 때문에 DTO 객체를 마찬가지로 Mock 객체로 생성하고, 임의의 값을 입력했다.

```java
// given 
MemberDto.Post post = new MemberDto.Post("test@gmail.com", "1234qwer!@#$", false, false);

String content = gson.toJson(post);
```

이후 gson 객체에 해당 dto를 전달해 json 형태로 변환했다.

이러한 DTO 전달이 필요한 기능은 회원 가입과 회원 수정 기능 두 가지가 있었다.

<br>

#### 1. 회원 가입

회원 가입의 경우 앞선 DTO 객체로 데이터와 함께 url로 요청을 보낸다.

```java
mockMvc.perform(post("/members/signup")
    .contentType(MediaType.APPLICATION_JSON)
    .content(content))
  .andExpect(status().isCreated())
  .andDo(print());
```

이후 바로 then 에 해당 하는 작업인 예상 상태값과 응답 출력을 호출했다.

또한,

```java
verify(memberService, times(1)).createMember(any(MemberDto.Post.class));
```

이와 같이 연계된 memberService의 회원 생성 메서드가 한 번만 호출 되는지 확인도 할 수 있다.

<br>

#### 2. 회원 수정

마찬가지로 앞선 DTO 객체 생성과 gson을 통한 json 타입으로의 변환을 활용하여 구성하였다.

코드 자체는 회원 가입과 동일하나 이 기능부터는 헤더로 jwt 액세스 토큰을 포함하여 요청해야하기 때문에
mockMvc로 가요청 시 헤더를 생성했다.

```java
mockMvc.perform(
  patch("/members/update")
    .header("Authorization", "Bearer fakeToken")
    .accept(MediaType.APPLICATION_JSON)
    .contentType(MediaType.APPLICATION_JSON)
    .content(content))
  .andExpect(status().isOk()).andDo(print());
```

<br>

#### 3. 회원조회와 삭제

이 두 가지 기능에서는 요청 시에 데이터가 필요하지 않고 헤더로 전달 받은 토큰으로 회원을 구분하기 때문에
content를 포함할 필요가 없다.

이외에는 위의 두 기능의 테스트 케이스와 코드가 동일하다.

```java
mockMvc.perform(
  delete("/members/leaveid")
    .header("Authorization", "Bearer fakeToken")
    .accept(MediaType.APPLICATION_JSON)
    .contentType(MediaType.APPLICATION_JSON))
  .andExpect(status().isNoContent()).andDo(print());
```

<br>
<br>
<br>

### 어려웠던 테스트 보안 핸들링

사전 작업 중 스프링 보안 필터를 끄는 애너테이션이 있었는데, 첫 작성 시에는 해당 애너테이션을 몰랐기 때문에 이 부분이 가장 어려웠다.

모든 요청에는 헤더에 JWT 액세스 토큰이 필요했기 때문에 어떤 방법을 사용해야할지 몰라 많이 헤맸다.

그래서 생각나는 대로 여러 방법을 적용해 보았다.

<br>

##### 1. 미리 생성한 토큰을 헤더에 삽입하기

처음 했던 것이 mockMVC로 요청을 보낼 때 헤더에 미리 생성해둔 액세스 토큰을 포함하는 것이었다.

가장 먼저 @BeforeEach 애너테이션(All로 하면 회원 상태가 유지되기 때문에 일부 테스트 케이스가 실패한다.)을 활용하여 회원을 생성한 다음,

```java
.header("Authorization", "Bearer fakeToken")
```

이와 같은 헤더 포함 메서드에 fakeToken이 아닌 앞서 생성한 사용자를 로컬에서 별도로 생성하여 해당 회원의 토큰을 발급 받아 넣었었는데,
이렇게 처리하게 되면 애초에 토큰 유효시간이 있기 때문에 해당 시간 내에만 테스트가 가능했다.

이상하다고 생각하여 다른 방법으로 접근했다.

<br>

##### 2. 테스트 시 회원 생성과 토큰 발급

1번에서 말한 방법대로 @BeforeEach 을 활용해 회원을 생성하고, 동시에 토큰 생성에 필요한 객체들을 mock으로 주입받아 테스트 실행 시 마다 
토큰을 생성하게 끔 만들었다.

그런데 이 방법은 실제 데이터베이스에 회원이 들어있지 않아, 요청 후 응답 시 실체가 없어 번번히 실패했다.

그래서 또 다른 방법을 시도했다.

<br>

##### 3. 가짜 사용자 생성

```java
// 가짜 사용자 생성
UsernamePasswordAuthenticationToken principal = new UsernamePasswordAuthenticationToken(
  "username", "password", AuthorityUtils.createAuthorityList("ROLE_USER"));

// SecurityContextHolder에 가짜 사용자 설정
SecurityContextHolder.getContext().setAuthentication(principal);
```

스프링 시큐리티를 직접적으로 활용하여 임시 사용자를 생성하는 방법이었는데, 실상 2번과 별 다를 바 없었고
이를 통해 토큰을 발급 받아도 2번과 같은 에러가 발생하여 삭제했다.

<br>

##### 4. 스프링 시큐리티 필터 끄기

세 가지 방법과 씨름하다가 지속적인 테스트 케이스 부분 실패, 순환 참조 문제 등이 발생하여 진행 방식이 이상하다는 것을 느꼈다.

애초에 통신이 되는지만 체크하면 되는데, 점점 복잡해지는게 이상했기 때문이다.

이 전에 봤던 블로그에서 스프링 테스트 애너테이션을 활용해 몇몇 문제를 해결했던 글이 생각나 스프링 공식 문서에
WebMvcTest 키워드를 기준으로 살펴 보았고, 이 [문서](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/autoconfigure/web/servlet/WebMvcTest.html)
에 나와있는 애너테이션들이 무슨 역할을 하는 지 구글링 해보았다.

그 중 @AutoConfigureMockMvc 를 알게 되어 좀 더 살펴보았다.

이 애너테이션은 MockMvc 인스턴스를 자동으로 구성해주는 애너테이션인데, 옵션으로 스프링 시큐리티, CORS와 같은 필터를 적용하지 않도록 설정할 수 있다.

그렇기 때문에 단순히 이 옵션을 적용하면 보안 관련 필터를 무시하고 동작만을 테스트할 수 있게 해준다.

내가 생각하던 컨트롤러 테스트의 목적에도 부합하기 때문에 이 애너테이션과 옵션을 적용하여 문제를 해결했다.

<br>
<br>
<br>

### 인식 정정과 이후

지금까지 내가 한 테스트가 단위 테스트인 줄 알았다.

컨트롤러의 핸들러 메서드들을 각각 테스트 했기 때문에 그런 줄 알았는데, 그게 아니라 컨트롤러 계층의 통신에 대해 전반적으로 테스트하기 때문에
계층 테스트인 슬라이스 테스트가 맞았다.

또한 이번 테스트 초반에는 @SpringBootTest 어노테이션을 적용하여 진행 했었는데, 이 어노테이션은 전체 애플리케이션을 로드하고, 실제와 같은 조건에서
애플리케이션을 실행, 테스트하기 때문에 통합 테스트에 이용되는 어노테이션이다.

본격적으로 테스트 환경을 구성해본 것이 처음이라 지식이 부족했던 것 같다.

이를 바탕으로 이후에는 좀 더 정확히 표현할 수 있을 것 같다.

<br>

앞으로, 컨트롤러 계층 슬라이스 테스트를 마무리하고 나면
서비스 계층에서 단위 테스트로 각 기능에 집중하고,
리포지터리 계층(@DataJpaTest)에서 슬라이스 테스트를 이용하여 DB와의 상호작용을 검증할 것이다.

마지막으로는 애플리케이션의 전체 흐름과 정상 동작하는 지를 통합 테스트해 볼 것이다.
 
<br>
<br>
<br>

***

### 후기

이번 테스트 블로깅을 진행하면서 잘 몰랐던 것과 헷갈렸던 것에 대해 정정하고 가게 되어 기쁘다.

또한, 테스트 케이스 작성을 처음에는 어렵게만 생각했는데 목적을 뚜렷히 두고 진행하니 훨씬 수월하고 할 만하다.

물론, 처음부터 테스트 케이스를 작성하면서 프로젝트를 진행했으면 더 좋았겠지만 그렇게 못 했으니 지금이라도 착실히 하자는 생각이다.

남은 테스트도 잘 진행하고, 이후 기능 추가나 성능 개선도 확실히 해보자.
