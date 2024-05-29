---
title: "프로젝트:샐로그 / 테스트 - 회원 : 통합 테스트"
author: Jeremiah Lee
date: 2024-05-29
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

슬라이스 테스트와 유닛 테스트를 마무리하고 다시 통합 테스트를 진행했다.

회원과 수입 외에도 고정수입, 예산에 대한 테스트를 진행했었지만 해당 내용은 크게 다룰 내용이 없기 때문에 별도로 작성하지 않았다.

기본적으로 수입에 있는 기능과 비슷하거나 간단한 CRUD 정도 밖에 없기 때문에 문제될 내용이 없었다.

또한 통합 테스트에서도 다룰 내용은 회원과 수입 두 가지이다.

<br>

이 통합 테스트를 진행하면서 동시에 문서화 작업도 처리했는데, 관련 내용은 별도의 포스트에 작성할 것이다.

너무 길게 쓰면 가독성도 떨어지고 포인트를 짚기 어려울 것 같기 때문에 분리하려고 생각했다.

<br>
<br>
<br>

### 테스트 진행 방식

우선 최대한 많은 기능에 대한 테스트를 진행하고자 하였으며, 비지니스 예외 처리나 의도된 에러 응답 등에 대해서 테스트 케이스를 작성했다.

이외에 400, 500, 403 등의 흔히 볼 수 있는 에러에 대한 테스트 케이스를 작성하지는 않았지만, 추후 자주 발생하는 상황이 있다면 테스트 케이스를 추가할 예정이다.

<br>

하나의 기능을 완성한 다음 테스트 케이스를 작성한 것이 아니라, 이미 배포 중인 프로젝트에 대해 유지보수 차원으로 테스트 케이스를 작성했기 때문에
놓친 부분이 있을 수 있다.

<br>
<br>
<br>

### 통합 테스트 관련 애너테이션

문서화에 대한 것은 별도의 포스트에 작성할 예정이니 해당 내용은 제외하고 통합 테스트 관련된 설정만 살펴보자.

<br>

우선 테스트 클래스에 적용된 애너테이션을 살펴보면,

```java
@SpringBootTest // 테스트 환경 애플리케이션 컨텍스트 로드
@AutoConfigureMockMvc // MockMvc 자동 구성, 웹 계층 테스트
@AutoConfigureRestDocs(outputDir = "build/generated-snippets") // Rest Docs 자동 구성, 문서화
@TestMethodOrder(MethodOrderer.OrderAnnotation.class) // 테스트 케이스 순서 보장
@Transactional
public class MemberIntegrationTest {
  ...
}
```

상당히 많은 것을 볼 수 있다.

최대한 이 코드를 보는 이가 이해할 수 있도록 주석을 달아놓긴 했다.

<br>

첫 번째로 @SpringBootTest 애너테이션은 스프링 부트로 제작된 프로젝트의 통합 테스트에서 자주 사용되는 애너테이션이다.

이 애너테이션은 실제 애플리케이션과 동일한 환경에서 테스트를 실행할 수 있게 컨텍스트를 로드하여 통합 테스트를 수행한다.

이렇게 함으로써 애플리케이션의 컨트롤러, 서비스, 리포지토리 등을 포함한 통합 테스트를 해볼 수 있게 된다.

<br>

다음으로 @AutoConfigureMockMvc 애너테이션은 MockMvc를 자동으로 구성한다.

이는 웹 계층 테스트를 위해 사용되고, 실제 서버를 시작하지 않고 컨트롤러를 테스트할 수 있게 된다.

즉, 통합 테스트에서 MockMvc를 통해 HTTP 요청을 모의하고 응답을 검증할 수 있게 된다.

<br>

세 번째로 @AutoConfigureRestDocs는 말 그대로 RestDocs를 자동 구성하는 역할을 하는데, 문서화에 필요한 애너테이션이다.

테스트 결과를 API 문서화하는 데 필요한 설정을 자동으로 제공한다.

또한, outputDir 속성을 통해 생성된 스니펫이 저장될 디렉토리를 지정했다.

<br>

마지막으로 @TestMethodOrder 애너테이션인데, 이 애너테이션은 테스트 메서드 실행 순서를 지정하는 애너테이션이다.

처음에는 각 메서드에 @Order() 만 붙여서 사용하면 되는 줄 알았는데, 테스트 클래스에 해당 애너테이션을 적용해야
그 설정들이 정상 작동한다.

<br>
<br>

다음은 의존성 주입에 관한 애너테이션인데,

```java
    @Autowired
    private MockMvc mockMvc;
    @SpyBean // 실제 객체의 메서드를 호출하면서도 메서드 호출을 검증 ref.https://jojoldu.tistory.com/226
    private MemberService memberService;
    @SpyBean
    private TokenBlackListService tokenBlackListService;
    @SpyBean
    private MemberRepository memberRepository;
    @Autowired
    private PasswordEncoder passwordEncoder;
    @Autowired
    Gson gson = new Gson();
    @Autowired
    private JdbcTemplate jdbcTemplate;

    // 이메일 전송 로직 모킹
    @MockBean
    EmailSender emailSender;
```

MockMvc, Gson 과 같이 웹 계층 테스트를 위한 클래스도 있고,
회원 컨트롤러에 주입된 클래스인 서비스나 토큰 블랙리스트 등도 있다.

또, 회원 객체를 모킹하기 위해 리포지토리와 패스워드 인코더도 주입해주었다.

여기서 특이한 점은 실제 기능을 사용하기 위해 의존성 주입된 클래스인 서비스, 리포지토리, 토큰 블랙리스트 클래스에
@Autowired가 아닌 @SpyBean 애너테이션이 적용되어 있는데, 이 애너테이션은 스프링부트 1.4에서 @MockBean과 함께 추가된
테스트용 애너테이션이다.

이 애너테이션은 Mockito를 통해 Mock 테스트를 진행하는 데 있어서 편의성을 제공하기 위해 추가되었는데, 
기존 빈의 메서드 호출을 감시하고, 일부 메서드를 모의하거나 원래 메서드를 호출할 수 있다.

즉, 컨텍스트에 존재하는 빈의 스파이 인스턴스를 별도로 생성하여 부분적으로 모의할 수 있게 된다.

이를 통해 간편하게 의존성을 주입할 수 있게되고, 실제 동작 뿐만아니라 부분적으로 모의하여 테스트를 좀 더 쉽게 다룰 수 있다.

좀 더 자세한 내용은 [기억보단 기록을](https://jojoldu.tistory.com/226) 블로그를 참고하자.

<br>
<br>

이 외에는 메서드에 적용된 애너테이션이 있는데,

```java
    @BeforeEach
    void setup() throws Exception {
  
    }

    @Test
    @DisplayName("회원가입 성공")
    @Order(1)
    void postMemberTest_Success() throws Exception {
  
    }
```

위와 같이 테스트 데이터를 모킹하기 위해 각 테스트 케이스 시작 전 실행되는 setup 메서드의 @BeforeEach 애너테이션이 있고,
나머지는 테스트를 명시하는 @Test 애너테이션과 테스트 결과의 제목을 설정하는 @DisplayName, 테스트 전체 실행 시 메서드의 순서를 지정하는 @Order
정도가 있다.

<br>
<br>
<br>

### 회원 객체 모킹과 토큰 생성

통합 테스트는 애플리케이션의 각 컴포넌트들이 상호 유기적으로 정상 동작하는 지 테스트 해보기 위한 것이다.

그렇기 때문에 이 전에 진행했던 슬라이스, 혹은 유닛 테스트와 달리 목업된 데이터가 필요하다.

특히 이번 회원 통합 테스트에서는 "회원가입" 기능을 제외한 모든 기능은 가입된 회원이 필요한 기능들이다.

그래서 가장 먼저 각 테스트 케이스 시작 전 회원을 미리 저장해 놓아야 한다.

```java
    // given : 회원 모킹
    @BeforeEach
    void setup() throws Exception {
        jdbcTemplate.execute("ALTER TABLE member ALTER COLUMN member_id RESTART WITH 1");

        memberRepository.deleteAll();

        member = new Member();
        member.setMemberId(1L);
        member.setEmail("test@example.com");
        member.setPassword(passwordEncoder.encode("1234qwer!@#$"));
        member.setEmailAlarm(false);
        member.setHomeAlarm(false);
        member.setRoles(List.of("USER"));
        member.setLedgerTags(Collections.emptyList());
        memberRepository.save(member);

        // JWT 토큰 생성
        token = generateAccessToken(member.getEmail());
    }
```

목업 데이터는 샘플 데이터이다.

즉, 이 회원을 테스트 용으로 생성한 것이 위 내용이다.

jdbcTemplate 에 관한 내용은 뒤로 미루고, 일단 이 회원 데이터를 생성하여 "회원가입"을 제외한 나머지 기능에서 사용할 것이다.

그런데 우리 애플리케이션은 회원을 구분할 때 JWT 액세스 토큰의 페이로드에 포함된 식별자를 활용하여 회원을 구분한다.

그렇다면 기능을 정상적으로 테스트 해보기 위해서는 이 액세스 토큰이 필요하다는 뜻이다.

이 액세스 토큰을 어떻게 발급 받아 사용할 것인지, 방법에 대한 생각을 좀 해봤었다.

토큰 발급 로직을 의존해 위에 저장된 회원을 바탕으로 토큰을 발급 받는 방법이 있었는데,
통합 테스트는 실제 애플리케이션의 동작을 테스트 해보는 것이기 때문에 이보다는 실제로 로그인하여 토큰을 발급 받는 것이 맞다고 생각하여
아래의 로직을 활용해 실제 로그인처럼 애플리케이션에 요청을 보냈다.

```java
    // 실제 동작을 모의하기 위한 login 요청 (액세스토큰 추출)
    private String generateAccessToken(String username) throws Exception {
        MvcResult result = mockMvc.perform(post("/members/login")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content("{\"email\":\"" + username + "\", \"password\":\"" + "1234qwer!@#$" + "\"}"))
                .andExpect(status().isOk())
                .andReturn();

        String responseContent = result.getResponse().getContentAsString();
        return JsonPath.read(responseContent, "$.accessToken");
    }
```

엄밀히 말하면 mockMvc 를 활용해 요청을 모의한 것인데,
이를 통해 로그인 로직이 정상 작동하는 지 테스트할 수 있었을 뿐만 아니라 토큰도 발급받아 사용할 수 있게 된 것이다.

<br>
<br>
<br>

### DB 식별자 증가 문제와 테스트 속도 향상

다시 돌아와서 jdbcTemplate를 왜 적용했는 지 살펴보자.

작성된 테스트 케이스들은 개별로 실행할 때 별 문제 없이 전부 통과한다.

그러나 테스트 클래스 자체를 실행하면 몇몇 실패하는 케이스가 있었는데, 그 케이스들의 공통점은 목업한 회원 정보가 일정하지 않아서 실패한다는 것이었다.

좀 더 간단히 말하면 회원의 식별자가 DB의 데이터를 모두 삭제해도 계속해서 증가하기 때문에 식별자를 특정해야하는 케이스에 문제가 생기는 것이다.

회원의 식별자를 지정해서 목업했지만, 그보다 DB 식별자 저장 설정이 우선시 된다는 것이다.

<br>

이를 해결하기 위해 처음에는 아래와 같은 애너테이션을 사용했다.

```java
@DirtiesContext(methodMode = DirtiesContext.MethodMode.BEFORE_METHOD)
```

이 애너테이션은 컨텍스트를 리로딩하는 애너테이션인데, 현재 적용된 속성대로면 메서드 시작 전에 애플리케이션 컨텍스트를 다시 로드한다.

컨텍스트를 리로딩함으로써 테스트 H2 데이터베이스는 새로운 것이 되고 식별자가 중첩해서 증가하지 않으리라 생각하여 이 방법을 적용했다.

그런데, 문제가 되는 케이스들을 실행할 때마다 전체 컨텍스트를 다시 로드하기 때문에 테스트 속도가 느려지는 또 다른 문제가 생겼다.

지금이야 20 ~ 30 개 정도의 테스트 케이스 밖에 없어서 좀 불편하더라도 넘어갈 수 있지만, 추후 계속된 기능 추가로 테스트 케이스가 훨씬 많아지고
100개 200개가 된다면? 속도는 아주 느릴 것이다.

그래서 이런 케이스들이 시작될 때마다 컨텍스트를 다시 로드하는 것은 좋은 방법이 아니라고 생각했다.

<br>

그러면 어떤 방법이 있을까?

좀 더 단순하게 생각하기로 했다.

테스트 DB에서 데이터가 삭제되어도 식별자가 중첩해서 증가한다면, 그냥 매 케이스 시작 전 회원이 생성될 때 식별자가 다시 1부터 시작하게끔 설정하면 된다.

이 방법은 회원 목업 데이터를 저장하기 전 리포지토리의 모든 데이터를 삭제하고 진행하기 때문에 데이터를 여러 개를 추가해도 문제가 없었다.

아래와 같이 jdbcTemplate 라이브러리를 활용하여 직접적으로 쿼리문을 입력해주었다.

```java
    // given : 회원 모킹
    @BeforeEach
    void setup() throws Exception {
      jdbcTemplate.execute("ALTER TABLE member ALTER COLUMN member_id RESTART WITH 1");
      ...
    }
```

이를 통해 컨텍스트를 다시 로드하지 않아도 회원이 생성될 때 항상 식별자는 1부터 다시 시작할 수 있게 되었다.

<br>

위와 같이 목업 데이터 생성 전에 모든 데이터를 지우고, 다시 식별자를 1부터 시작하는 쿼리를 사용해 문제를 해결했으며,
기존 컨텍스트를 리로드하는 방법을 사용했을 때는 10초 가량 걸리던 테스트가 3초까지 단축되었다.

오히려 단순하게 생각하여 테스트 성능을 70% 향상시켰다.

<br>
<br>
<br>

### 회원 통합 테스트

이제 잠깐 테스트 세부 내용을 살펴보고 가자.

그런데 사실 코드 자체는 컨트롤러 슬라이스 테스트랑 별반 차이가 없다.

```java
    @Test
    @DisplayName("회원가입 성공")
    @Order(1)
    void postMemberTest_Success() throws Exception {
        // given
        MemberDto.Post postDto = new MemberDto.Post(
                "test@gmail.com", "1234qwer!@#$", false, false
        );

        String content = gson.toJson(postDto);

        // when
        mockMvc.perform(
                        post("/members/signup")
                                .accept(MediaType.APPLICATION_JSON)
                                .contentType(MediaType.APPLICATION_JSON)
                                .characterEncoding("UTF-8")
                                .content(content)
                )

                // then
                .andExpect(status().isCreated())
                .andDo(print());
    }
```

통합 테스트라고 해서 뭔가 거창한 로직이 있어야할 것 같지만, 일단 이 테스트의 목적이 URI로 요청을 보내고 작동이 잘되는 지 테스트하는 것이 전부이다.

단, 슬라이스와는 다르게 서비스 로직 실행, DB에 저장까지 진행된다.

즉, 기대한 대로 응답이 온다면, 모든 레이어를 거쳐 이 기능이 정상 작동한다는 의미이다.

<br>

이외에 만약 외부 시스템을 활용해야하는 로직이 있다면

```java
    @Test
    @DisplayName("회원가입 시 이메일 인증 성공 : 이메일이 존재하지 않는 경우")
    @Order(13)
    void sendVerificationEmailTest_Success() throws Exception {
        // given
        EmailRequestDto emailRequestDto = new EmailRequestDto(
                "check@example.com", null
        );

        String content = gson.toJson(emailRequestDto);

        // 실제 이메일 전송 하지 않도록 모킹
        doNothing().when(emailSender).sendVerificationEmail(anyString(), anyString());

        // when
        mockMvc.perform(
                post("/members/signup/sendmail")
                        .contentType(MediaType.APPLICATION_JSON)
                        .accept(MediaType.APPLICATION_JSON)
                        .characterEncoding("UTF-8")
                        .content(content)
        )

                // then
                .andExpect(status().isOk())
                .andDo(print());
    }
```

이렇게 Mockito를 활용하여 아무 동작도 하지 않게 모킹해주면 된다.

<br>
<br>
<br>

### 데이터 검증에 대해

앞서 살펴봤던 "회원가입" 로직은 리턴 값이 없기 떄문에 입력한 대로 DB에 저장되었는지 캡처 라이브러리를 통해 DTO와 DB에 저장된 member의 데이터를 비교하는 로직도 작성했었는데,
이는 고정된 데이터기 때문에 불필요하다 생각했다.

그래서 아래와 같이 조회의 경우에만 검증했다.

```java
    @Test
    @DisplayName("회원조회 성공")
    @Order(9)
    void getMemberTest_Success() throws Exception {
        // when
        mockMvc.perform(
                        get("/members/get")
                                .header("Authorization", token)
                                .contentType(MediaType.APPLICATION_JSON)
                                .accept(MediaType.APPLICATION_JSON)
                                .characterEncoding("UTF-8")
                )

                // then
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.data.memberId").value(member.getMemberId()))
                .andExpect(jsonPath("$.data.email").value(member.getEmail()))
                .andExpect(jsonPath("$.data.emailAlarm").value(member.isEmailAlarm()))
                .andExpect(jsonPath("$.data.homeAlarm").value(member.isHomeAlarm()))
                .andExpect(jsonPath("$.data.incomeTags", hasSize(0)))
                .andExpect(jsonPath("$.data.outgoTags", hasSize(0)))
                .andDo(print());
    }
```

사실 이것도 위에 셋업 메서드로 세팅한 값들이기 때문에 굳이 안해도 될것이라 생각했지만, 일단 추후 필요할 수 있으니 작성했다.

<br>
<br>
<br>

### 테스트 커버리지

작성한 회원 통합 테스트가 실제 기능을 어느 정도 커버하는지 확인했다.

![](/assets/img/projects/salog/memberIntegrationTest_Coverage_byIntelliJ.png)

이는 인텔리제이에서 테스트 클래스를 with Coverage 옵션으로 실행하면 나오는 코드 커버리지 확인란인데, JaCoCo와 통합되어 사용할 수 있는 기능이다.

<br>

각 항목은 아래와 같다.

Class : 클래스 단위로 테스트된 비율

Method : 각 메서드 단위로 테스트된 비율

Line : 코드의 각 라인이 테스트되었는지 비율

Branch : 조건문 내 모든 분기점이 테스트 되었는지 비율

<br>

위에서 확인할 수 있듯이 수치가 떨어지는 부분이 mapper와 service의 Branch 항목이다.

특히 mapper에서는 내부적으로 생성되는 분기에 대한 테스트가 미흡하다고 나오는데, 대부분 태그 관련된 경로다.

태그는 따로 태그 통합 테스트에서 진행해야한다고 생각하여 null로 처리해버린 것이 문제였을 것이다.

<br>

다음은 service 쪽인데, 이건 외부 시스템인 이메일 전송 로직 때문이다.

이메일을 전송하면서 발생하는 에러를 핸들링 할 수 있도록 분기를 나누어 놓았지만, 현재 테스트 케이스에서는 Mockito를 활용해 아무 처리도 하고 있지 않다.

이를 핸들링하려면 외부 시스템까지 테스트를 해봐야할 것 같아 별도로 핸들링하지는 않을 것이다.

<br>

현재 회원 통합 테스트의 커버리지는 평균 83%가 나온다.

주요 로직을 커버하고 있다고 생각할 수 있으나, 단순히 커버리지 수치를 높이는 것이 아니라 의미 있는 테스트를 작성하는 것이 목적인 만큼 생각해 볼 수 있는 것들이 많다.

<br>
<br>
<br>

### 후기

내용이 상당히 길어졌다.

제대로 테스트를 진행해본 것이 이번 Salog 프로젝트가 처음이었고, 추후 이 포스트를 보면서 다른 데에 대입할 수 있지 않을까 해서 문제가 있던 부분을
최대한 상세히 작성했다.

그래도 부족할 수 있기 때문에 잘 모르겠으면 깃헙가서 코드를 보자는 생각이다.

또한, 커버리지를 도대체 어떻게 확인해야하지 생각하고 있었는데 의외로 인텔리제이에 기능이 많다는 것에 놀랐다.

아마 내가 사용하는 부분은 빙산의 일각이지 않을까싶다.

<br>

이어서 수입 쪽을 작성하려고 했는데, 작성했던 테스트들을 다시 보니 현재 작성된 회원 통합 테스트랑 큰 차이가 없었다.

대부분 문제였던 것들은 전반적으로 동일하게 있었고, 그것을 해결하는 내용을 이미 작성했기 때문에 별도로 작성할게 없다.

그래서 다음 포스트는 문서화에 대해 작성할 것이고, 이 때 수입 쪽을 살펴볼 것이다.
