---
title: "프로젝트:샐로그 / 보안 1 - Oauth 2.0 구현 과정 3 / 구현과 문제해결"
author: Jeremiah Lee
date: 2024-03-11
categories: [ 프로젝트, 샐로그, 보안, 문제해결 ]
tags: [프로젝트, 샐로그, 보안, Oauth, 문제해결 ]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Project_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

### ***시작하기 앞서***

이전까지는 OAuth 2.0의 기본적인 개념에 대한 설명과 사용하기 전 각 플랫폼에서 어떠한 사전 작업을 거쳐야 하는지에 대해 작성했다.

이렇게 개념부터 접근하는 방법은 내가 "신기술"을 채택하기 위한 과정이며, 잘 모르는 기술을 사용하고자할 때 필수적인 코스라 생각한다.

물론 프로젝트 카테고리가 아닌 별개의 카테고리로 분리하여 개념 학습 따로, 구현 따로 할 수 는 있겠지만 "이전에 사용해보지 않은 기술"이라는 것은
아직 미숙한 부분이 많을 것이고, 다음에 또 활용하고자 할 때도 마찬가지로 동일한 부분에서 헤맬 수 있다는 의미라고 생각한다.

즉, 개념부터 구현까지의 프로세스 전체를 이해해야 "신기술"이라는 것을 사용할 수 있게 된다는 것이기 때문에 이런 식으로 정리했다.

이 일련의 과정이 기록일 뿐만 아니라 복습인 것이다.

<br>

이번 포스트는 OAuth 2.0 활용의 마지막 포스트가 될 것이고, 이전 블로그에서 참고한 내용과 현재 "샐로그 프로젝트"에는 어떤 식으로 적용이 되었는지,
어떻게 적용했으며 왜 그렇게 활용했는지에 대한 설명과 어떤 문제가 있었는 지까지 작성할 것이다.

물론 길어지면 읽기 편하게 분할할 수는 있지만 최대한 압축해서 작성할 것이다.

<br>
<br>
<br>

### ***이전 블로그에서 참고한 것들***

기존에 사용하던 네이버 블로그에는 부트캠프에서 배웠던 내용들이 담겨있다.

그 중 OAuth 2.0에 대한 사용법도 담겨있는데, 부분적으로 도움은 되더라도 큰 내용은 없다.

"실습"이라고 해서 정리된 글을 따라하기만 하면 되는 작업이었고, 샘플을 구현해보는 단계라 SSR 방식으로 진행했기 때문이다.

또한, 각 개발 상황에 맞게 핸들링해야하는 기술 특성 상 완전히 똑같이는 하지 못 한다.

그래서 참고했던 부분만 체리피킹하여 갖고 오고, 나머지는 OAuth 기능 적용을 위한 소규모 JWT 구현, 컨트롤러 구현 이런 것이기 때문에 이사시키지도 않고
그대로 둘 예정이다.

기본적으로 이번 프로젝트에서 직접 적용한 내용을 정리하면 다음 번에는 어떻게 처리해야 할지 느낌이 올 것이다.

<br>

정리된 순서대로 참고한 내용을 정리할 것이다.

##### **의존성 추가**

어떤 애플리케이션에 기술을 추가할 때 가장 먼저 행해야 할 것은 스프링 부트 의존성 추가이다.

```
implementation 'org.springframework.boot:spring-boot-starter-oauth2-client'
```

특별한 것은 없고 위의 라이브러리를 추가하면 된다.

<br>

##### **설정 정보 추가**

다음으로는 이전에 사전 작업으로 생성했던 구글(혹은 다른 플랫폼)의 client id와 client secret, Authorization code를 받을 redirect uri,
마지막으로 회원의 정보 범위를 지정해주면 된다.

당연히 설정 정보이기 때문에 application.yml 파일에 넣으면 된다.

```yaml
spring:

...

  security:
    oauth2:
      client:
        registration:
          google:
            client-id: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
            client-secret: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
            redirect-uri: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
            scope:
              - email
```

depth는 당연하지만 spring에 적용할 내용이기 때문에 spring 아래에 포함된다.

사전 작업에서 발급 받거나 지정한 내용을 입력하면 된다.

물론 보안에 관련된 중요한 부분이기 때문에 외부에 노출되지 않도록 배포 시에는 github secrets를 통해 환경변수로 주입 받게 끔 설정해두었다.

<br>

##### **보안 설정 추가**

그 다음으로는 복잡한 OAuth 2.0의 설정을 스프링 시큐리티에 맡기기 위해 보안 설정 정보에 OAuth 2.0 로그인을 추가해야 한다.

WebMvcConfigurer 를 구현한 SecurityConfiguration 클래스에 구현된 필터 체인에 포함 시킨다.

```java
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
                .headers().frameOptions().sameOrigin()
                .and()

                .csrf().disable()
                .cors().configurationSource(corsConfiguration)
                .and()
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()

                .formLogin().disable()
                .httpBasic().disable()
                .exceptionHandling()
                .authenticationEntryPoint(new MemberAuthenticationEntryPoint())
                .accessDeniedHandler(new MemberAccessDeniedHandler())
                .and()

                .apply(new CustomFilterConfigurer())
                .and()

                .authorizeHttpRequests(authorize -> authorize
                  ...
                )
                .oauth2Login();  // OAuth2 로그인 설정
        return http.build();
    }
```

이를 통해 복잡한 설정을 가진 OAuth 2.0의 기본 동작을 스프링 시큐리티에서 처리하고 소셜 로그인을 활성화시켰다.

<br>

여기까지가 이전 블로그에서 참고한 기본 구성에 대한 내용이고 이후 부터는 JWT를 복합한 구현 내용이다.

다만 내용 자체가 샘플 애플리케이션 하나를 "제작"하는 것이었기 때문에 오히려 헷갈린 부분이 많았다.

또한, AuthenticationSuccessHandler를 별도로 구현하여 소셜 로그인이 성공하면 특정 html 파일로 리다이렉션 시켜 jwt를 리턴하도록 구현되어 있었는데,
우리 프로젝트에서 회원을 구분하기 위해 특정한 방법이 필요하여 핸들러를 활용하지 않고 직접 구현했다.

그래도 일단 프론트와 백 간에 어떤 식으로 OAuth 2.0이 활용될 수 있는지는 참고했다.

<br>

##### **Frontend와 Backend 간의 OAuth 2.0 인증 처리 흐름 (핸들러)**

기본적인 프론트, 백 간의 OAuth 2.0 인증 처리 흐름은 다음과 같다.

![](/assets/img/projects/salog/Frontend와_Backend_간의_OAuth_2_인증_처리_흐름.png)

순서대로 보면

1. 사용자가 웹 브라우저에서 "소셜 로그인 링크"를 클릭하여 요청함
2. 프론트 애플리케이션에서 백엔드 애플리케이션의 기본 소셜 로그인 주소(http://localhost:8080/oauth2/authorization/google 등)로 요청을 전송함, 이 때 URI에 대한 요청은 OAuth2LoginAuthenticationFilter 가 처리함
3. 소셜 로그인 화면을 요청하는 URI로 리다이렉트 시킴, 이 때 소셜 인증 서버가 백엔드 애플리케이션으로 "인증 코드"를 전송할 Redirect URI(사전 작업시 지정)를 쿼리 파라미터로 전달함 / 스프링 부트 yaml 파일에 설정된 정보를 자동 활용함
4. 구글 로그인 화면을 오픈함
5. 사용자가 로그인 수행
6. 로그인을 성공하면 3.에서 전달한 백엔드 측 Redirect URI로 "인증 코드"를 요청함
7. 소셜 인증 서버가 백엔드 애플리케이션에게 "인증 코드"를 응답으로 전송함 / 이 때 인증 코드는 Redirect URI의 파라미터로 들어감
8. 백엔드 애플리케이션이 소셜 인증 서버에 Access Token을 요청함
9. 소셜 인증 서버가 Backend 애플리케이션에 Access Token을 전송함 / 여기의 Access Token은 소셜 리소스 서버에 요청을 보내기 위한 토큰
10. 백엔드 애플리케이션이 소셜 리소스 서버에 유저 정보를 요청
11. 소셜 리소스 서버가 백엔드 애플리케이션에 유저 정보를 응답
12. 백엔드 애플리케이션은 JWT를 URI를 통해 전송함

여기서 6. ~ 11. 까지는 스프링 시큐리티에서 내부적으로 처리해주기 때문에 건드릴 필요가 없다고 하지만, 이는 스프링 시큐리티의 자동화된 OAuth2 로그인을 사용했을 때의 얘기이며,
"샐로그 프로젝트"에서는 로그인 시도 시 redirect URI를 통해 인증 코드를 받아오고 해당 인증 코드를 활용하여
액세스 토큰을 발급, 이메일 정보 취득, jwt 토큰 생성 후 리턴하는 과정을 직접 구현했다.

우리의 웹 서비스에서 가장 유니크한 값이며, 실제 사용되는 지에 대한 인증 절차를 거치고, 회원을 식별하는 기본 값인 이메일을 DB에 한 번 저장한 다음
jwt를 생성하도록 처리했다.

이렇게 인증 절차를 직접 구현하고 나니 인증 처리 흐름에 대해 좀 더 잘 알게 되었다.

추후에는 핸들러를 활용하여 OAuth 2.0 인증 처리를 할 수 있기 때문에 이 내용도 정리하는 게 맞지만 현재 포스트에서는 직접 구현한 내용을 다룰 것이고,
해당 내용은 별도의 포스트를 활용하여 이전에 정리하지 않은 상세한 내용과 함께 하나의 포스트에 정리해 둘 것이다.

<br>
<br>
<br>

### ***샐로그에서의 OAuth 2.0 인증 처리 구현 과정***

다시 돌아와 이전 블로그에서 참조한 내용은 정리가 끝났다.

결국은 설정이나 프론트 측과의 처리 과정에 대한 내용이었다.

이후 정리된 핸들러를 활용한 인증 처리 과정은 우리 애플리케이션의 특수한 상황에 적합하지 않다고 생각하여 채택하지 않고 복잡한 처리 과정을 직접 구현했다.

전체적인 흐름을 먼저 짚고 넘어가자면, 
SecurityConfiguration 클래스에서 OAuth2Login이 설정되어 있어 소셜 로그인을 활성화 했고, 특정 플랫폼의 소셜 로그인 주소로 접속하여 로그인 시도를 하게 된다.

현재 구글 로그인만 활성화 되어 있기 때문에 구글 로그인 버튼을 클릭하면 구글 로그인 폼이 열리고 해당 폼에서 로그인을 하면
인증 절차를 거쳐 사전에 지정해둔 Redirect URI로 인증 코드 등이 파라미터에 포함되어 리다이렉션된다.

리다이렉션되면 백엔드 컨트롤러에서 해당 URI를 get 매핑하여 인증 코드를 문자열 파라미터로 받아오고 해당 인증 코드를 바탕으로
액세스 토큰 발급, 사용자 정보 취득, 사용자 저장 및 식별, jwt 생성 후 반환 처리 하게 된다.

여기서 사용자 정보 취득까지는 스프링 시큐리티를 통해 자동적으로 처리할 수 있으며 해당 정보를 추출하여 jwt를 발급하기만 하면 된다.

그러나 학습을 위해 과정을 직접 핸들링 해보았다.

<br>

##### **OAuth2 인증 컨트롤러 구현**

의존성 추가, 시큐리티 설정, yml 설정 등을 제외하고 먼저 구현한 게 컨트롤러이다.

이 컨트롤러는 사용자가 로그인에 성공한 경우 사전에 지정된 Redirect URI으로 리다이렉션 된 후의 처리를 핸들링한다.

GET 매핑을 통해 Redirect URI로 리다이렉션되면 호출된다.

인증 코드의 경우 Redirect URI의 파라미터로 포함되어 있으며 이 코드를 바탕으로 모든 과정을 처리한다.

```java
@RestController
@RequestMapping("/members")
@Validated
@AllArgsConstructor
@Slf4j
public class OauthController {
    private final OauthService oauthService;

    @GetMapping("/login/oauth2/code/google")
    public void googleOauth2Callback(@RequestParam("code") String authCode, HttpServletResponse response) {
        try {
            String accessToken = oauthService.getAccessToken(authCode);
            String email = oauthService.getUserEmail(accessToken); // 액세스 토큰으로 이메일 정보 가져오기

            Map<String, String> tokens = oauthService.oauthUserHandler(email); // JWT 생성

            // 토큰을 쿼리 파라미터로 추가합니다.
            String redirectUrl = "http://www.salog.kro.kr/oauthGoogle?accessToken=" + URLEncoder.encode(tokens.get("accessToken"), "UTF-8")
                    + "&refreshToken=" + URLEncoder.encode(tokens.get("refreshToken"), "UTF-8");

            response.sendRedirect(redirectUrl);
        } catch (Exception e) {
            throw new RuntimeException("Failed to get JWT", e);
        }
    }
}
```

oauthService라는 서비스 객체에서 모든 작업을 도맡아 처리하고 있다.

한 흐름으로 email 까지 추출할 수 있게끔 할 수 있지만 일련의 과정을 다른 사람이 봐도 이해할 수 있도록 분리해 놓았다.

<br>

가장 먼저 인증 코드를 바탕으로 소셜 인증 서버에서 AccessToken을 발급 받기위해 getAccessToken 메서드에 인증 코드를 전달한다.

```java
...
@Value("${spring.security.oauth2.client.registration.google.client-id}")
private String GOOGLE_CLIENT_ID;
@Value("${spring.security.oauth2.client.registration.google.client-secret}")
private String GOOGLE_CLIENT_SECRET;
@Value("${spring.security.oauth2.client.registration.google.redirect-uri}")
private String REDIRECT_URL;

    public String getAccessToken(String authCode) throws JsonProcessingException {
        RestTemplate restTemplate = new RestTemplate();
        restTemplate.getMessageConverters().add(new FormHttpMessageConverter());

        MultiValueMap<String, String> params = new LinkedMultiValueMap<>();

        params.add("code", authCode);
        params.add("client_id", GOOGLE_CLIENT_ID);
        params.add("client_secret", GOOGLE_CLIENT_SECRET);
        params.add("redirect_uri", REDIRECT_URL);
        params.add("grant_type", "authorization_code");

        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_FORM_URLENCODED);

        HttpEntity<MultiValueMap<String, String>> request =
                new HttpEntity<>(params, headers);

        ResponseEntity<String> response = restTemplate.postForEntity(
                "https://oauth2.googleapis.com/token",
                request,
                String.class
        );

        if (response.getStatusCode() == HttpStatus.OK) {
            // response.getBody()에는 액세스 토큰 정보가 JSON 형태로 포함
            // 이를 파싱하여 액세스 토큰을 추출
            ObjectMapper objectMapper = new ObjectMapper();
            JsonNode rootNode = objectMapper.readTree(response.getBody());

            return rootNode.get("access_token").asText();
        } else {
            // 토큰 요청이 실패한 경우 에러 메시지를 반환
            return "Failed to get access token";
        }
    }
```

해당 로직을 담당하는 oauthService의 메서드는 이렇게 구성되어 있다.

yml 파일에 지정된 각 client id, client secret, redirect uri를 받아와서 multivaluemap을 활용하여 삽입했다.

마찬가지로 인증코드도 포함시켰다.

요청을 보내기 위해 RestTemplate를 활용했으며, 구글 인증 서버 주소로 해당 요청을 담아 보냈다.

참고로 담아야하는 파라미터들은 구글 OAuth2 공식 문서에 나와있다.

이후 응답이 돌아왔을 때, 실패하면 에러 메시지를 반환하고, 성공하면 JSON 형태인 액세스 토큰을 ObjectMapper를 사용해 파싱하고 문자열로 access token을 추출한다.

<br>

액세스 토큰을 발급 받았으니 다음으로는 해당 액세스 토큰을 가지고서 구글 리소스 서버에 사용자 정보를 요청 해야한다.

현재의 경우 email만 있으면 되기 때문에 email만 요청했다.

```java
    public String getUserEmail(String accessToken) throws JsonProcessingException {
        RestTemplate restTemplate = new RestTemplate();

        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_JSON);
        headers.set("Authorization", "Bearer " + accessToken);

        HttpEntity<String> entity = new HttpEntity<>("parameters", headers);

        ResponseEntity<String> response = restTemplate.exchange(
                "https://www.googleapis.com/oauth2/v3/userinfo",
                HttpMethod.GET,
                entity,
                String.class);

        if (response.getStatusCode() == HttpStatus.OK) {
            ObjectMapper objectMapper = new ObjectMapper();
            JsonNode rootNode = objectMapper.readTree(response.getBody());

            return rootNode.get("email").asText();
        } else {
            return "Failed to get user email";
        }
    }
```

액세스 토큰을 발급 받을 때와 마찬가지로 RestTemplate을 사용하여 요청을 보낸다.

헤더에 미디어 타입을 지정하고 access token을 포함시켜 요청했다.

만약 요청이 실패한다면 메시지가 리턴되고, 성공한다면 액세스 토큰 때와 같은 방식으로 파싱해 사용자의 이메일을 문자열로 변환 후 리턴한다.

<br>

이제 사용자의 이메일까지 받아왔기 때문에 이 이메일을 바탕으로 JWT를 생성해야한다.

```java
    //Oauth 회원 핸들링
    public Map<String, String> oauthUserHandler(String email) {

        Member oauthMember;

        if (verifiedEmail(email)) {
            oauthMember = memberRepository.findByEmail(email).get();
        } else {
            Member member = new Member();
            member.setEmail(email);
            member.setPassword(null);
            member.setHomeAlarm(false);
            member.setEmailAlarm(false);

            // 권한
            List<String> roles = authorityUtils.createRoles(member.getEmail());
            member.setRoles(roles);

            oauthMember = memberRepository.save(member);
        }

        // jwt 생성
        Map<String, Object> claims = new HashMap<>();
        claims.put("username", oauthMember.getEmail());
        claims.put("roles", oauthMember.getRoles());
        claims.put("memberId", oauthMember.getMemberId());

        String subject = oauthMember.getEmail();

        String base64EncodedSecretKey = jwtTokenizer.encodeBase64SecretKey(jwtTokenizer.getSecretKey());

        Date accessTokenExpiration = jwtTokenizer.getTokenExpiration(jwtTokenizer.getAccessTokenExpirationMinutes());
        String accessToken = jwtTokenizer.generateAccessToken(claims, subject, accessTokenExpiration, base64EncodedSecretKey);

        Date refreshTokenExpiration = jwtTokenizer.getTokenExpiration(jwtTokenizer.getRefreshTokenExpirationMinutes());
        String refreshToken = jwtTokenizer.generateRefreshToken(subject, refreshTokenExpiration, base64EncodedSecretKey);

        Map<String, String> tokens = new HashMap<>();
        tokens.put("accessToken", accessToken);
        tokens.put("refreshToken", refreshToken);

        return tokens;
    }
```

이 로직이 본격적으로 커스텀된 로직이다.

가장 먼저 등장하는 조건문은 회원의 email을 db에 있는지 확인하고 각 상태에 따라 분기한다.

```java
    // 존재하는 이메일인지 체크 (boolean)
    public boolean verifiedEmail(String email) {
        return memberRepository.existsByEmail(email);
    }
```

조건을 분기하기 위해 memberService에도 있는 로직을 옮겨왔으며, 이 로직을 통해 DB에 현재 토큰 발급을 요청한 회원의 이메일이 존재하는지 여부를
확인한다.

해당 이메일이 존재한다면 다시 DB에서 해당 회원을 찾아 oauthMember에 할당하고,
없다면 새로운 회원으로 저장한다.

이렇게 핸들링하는 이유는 앞서 말했지만 JWT의 페이로드에 회원의 식별자를 포함하기 위함이다.

우리 서비스는 헤더로 액세스 토큰을 받아온 다음 복호화 후 페이로드에 담겨있는 회원의 DB 식별자를 활용하여 회원을 구분하기 때문에 식별자가 필요하다.

자체 서비스를 제공하기 때문에 회원에 대한 구분은 필수적이라 이런 로직이 도출되었다.

다만 OAuth2로 로그인한 회원의 경우 별도로 PW가 존재하지 않기 때문에 회원 테이블에서 PW를 nullable로 변경하였다.

또한 pw를 nullable로 변경했기 때문에 관련된 로직 전체를 수정했고 null 인 경우에 대해 핸들링했다.

다시 oauthService로 돌아와서, 해당 회원 정보를 바탕으로 jwt를 생성했다.

생성된 jwt를 map에 담아 리턴했다.

<br>

이후 부터는 조금 복잡했는데, 프론트 측에서 이 Redirect URI로 접근이 안되고, 별도의 url로 리다이렉션 후 쿠키에 담아 보내려 했더니 서버는 https 프로토콜로 통신하는데,
프론트는 이 당시 http 프로토콜로 통신했기 때문에 상태 변경에 따른 쿠키 증발 문제가 있었다.

그래서 이 발급된 jwt를 어떻게 클라이언트 측에 전송해야하나 생각하다가, 새로 만든 url의 파라미터로 전송하기로 했다.

발급 후 대쉬보드 페이지로 리다이렉션되는 것이 한 순간이더라도 기록이 남거나 불이익이 있을 것이라 생각하여 꺼려지긴 했지만 당장은 구현이 우선이라 
보안에 대한 걱정을 미뤄두로 파라미터로 담아냈다.

추후 이 문제는 다시 구현하면서 고쳐볼 생각이다.

또한 프론트 측의 팀원과 대화를 해보니 로그인 처리 과정을 클라이언트에서 담당하고 처리 후 인증 코드만 백엔드 api로 전송해주어 oauth2 로그인을 해결한다고는 했는데,
내가 알기로는 내가 한 방식이 기본 소셜 로그인 방식으로 알고 있어서 좀 고민이 되기도 했다.

그러나 이미 구현이 끝났기 때문에 이 상태로 적용하긴 했지만 조금 더 알아봐야 할 것 같다.

<br>
<br>
<br>

### ***구현 도중 발생했던 문제들***

사실 처음부터 이렇게 매끄럽게 구현된 게 아니다.

찾아보던 정보들과 내가 구현하려는 방향성이 맞지 않아 헷갈려서 몇 가지 문제가 있었다.


##### **1. 설정 정보 누락**
처음 마주한 문제는 내가 인증 처리 흐름에 대해 이해하지 못해서 발생한 문제였는데,

```
org.springframework.security.oauth2.client.ClientAuthorizationRequiredException: 
  [client_authorization_required] Authorization required for Client Registration Id: google
```

이렇게 Client id가 맞지 않다는 에러가 계속 발생한 것이었다.

이 당시 모든 정보는 일치했다.    
client id, secret, redirect uri, scope, 의존성 등 설정해둔 정보는 사전 작업 때 발급 받고 작성했던 그대로였다.

그래서 한참을 헤맸는데, 이전에 작성했던 블로그 글과 구글링을 통해 다른 사람들이 구성한 OAuth2 로그인 구현을 보고
스프링 시큐리티 설정 클래스인 SecurityConfiguration 클래스에서 .oauth2Login() 옵션을 추가하지 않아서 발생한 문제였다.

```java
@Configuration
@EnableWebSecurity(debug = true)
@AllArgsConstructor
public class SecurityConfiguration implements WebMvcConfigurer {
    private final CustomAuthorityUtils authorityUtils;
    private final CustomCorsConfiguration corsConfiguration;
    private final MemberRepository memberRepository;

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
                .headers().frameOptions().sameOrigin()
                .and()

                .csrf().disable()
                .cors().configurationSource(corsConfiguration)
                .and()
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()

                .formLogin().disable()
                .httpBasic().disable()
                .exceptionHandling()
                .authenticationEntryPoint(new MemberAuthenticationEntryPoint())
                .accessDeniedHandler(new MemberAccessDeniedHandler())
                .and()

                .apply(new CustomFilterConfigurer())
                .and()

                .authorizeHttpRequests(authorize -> authorize
                  ...
                )
                .oauth2Login();  // OAuth2 로그인 설정
        return http.build();
    }
```

OAuth2 로그인 기능을 사용하기 위해 HttpSecurity 객체를 통해 해당 옵션을 명시적으로 설정해야한다.

이 설정은 OAuth2 기반의 로그인 매커니즘을 활성화하고, OAuth2 로그인 프로세스를 처리하는 필터들을 필터 체인에 추가하는 역할을 한다.

이를 설정하지 않으면, OAuth2 로그인을 처리할 수 있는 필터들을 필터 체인에 추가하지 않게 되고,
OAuth2 프로세스를 시작하려고 할 때 인증 메커니즘과 리다이렉션 처리등이 제대로 구성되지 않아
ClientAuthorizationRequiredException 같은 예외가 발생한 것이다.

한 마디로, 설정이 누락되어 사전에 작성한 OAuth2의 다른 설정 정보를 찾지 못한 것이다.

간단하지만 당시에는 구현 방법을 잘 몰라 놓쳤다.

<br>

##### **2. Redirect URI를 리스닝하는 컨트롤러 누락**

이 경우는 흐름에 대해 알고는 있지만 어떻게 자바-스프링 프로젝트에서 핸들링해야하는 지 몰랐던 경우다.

```java
@RestController
@RequestMapping("/members")
@Validated
@AllArgsConstructor
@Slf4j
public class OauthController {
    private final OauthService oauthService;

    @GetMapping("/login/oauth2/code/google")
    public void googleOauth2Callback(@RequestParam("code") String authCode, HttpServletResponse response) {
      ...
        }
    }
}
```

OAuth2로 로그인이 성공한 후 미리 지정해둔 Redirect URI를 통해 사용자를 원래 애플리케이션으로 리다이렉트하기 위해 필요하다.    
또한 이 과정 중에 인증 코드와 같은 중요한 정보를 전달 받는다.

여기서부터 인증 코드를 전달 받아 백단에서 액세스 토큰을 발급 받고 사용자 정보를 바탕으로 추가적인 로직을 실행시켜야 한다.

즉, 리스닝하는 컨트롤러가 있어야 리다이렉트 URI로 오는 정보를 전달받기 위함이다.

만약 이러한 작업이 없다면 404 에러 코드가 담긴 페이지가 출력된다.

<br>

##### **3. 프론트와 백 간의 OAuth2 인증**

처음에는 리다이렉트 URI에서 인증 코드를 받아오고 바로 jwt를 바디로 리턴했다.

다만 이렇게 처리하니 클라이언트 측에서 이 리다이렉트 URI가 서버 도메인이라 핸들링이 안된다고 했다.

그래서 그 다음으로는 쿠키를 생성하여 jwt를 담았는데, 이마저도 클라이언트 통신 프로토콜이 서버 프로토콜과 일치하지 않아서 
쿠키가 증발해버렸다...

이 때 들은 얘기가 일반적으로는 클라이언트 측에서 인증 처리를 담당하고, 백엔드로 jwt 발급만 받는다고 했는데, 이미 구현되어 있는 상태이고, 애매해서
결국은 JWT를 발급 받고나서 특정 URL로 이동시키고, 해당 URL의 파라미터로 토큰을 포함시키는 방법을 사용했다.

<br>

마지막으로, 모든 로직은 백단에서 처리하기 때문에 프론트에서는 단순히 구글 로그인 주소만 연결하고서
모든 인증 절차가 끝난 뒤 백에서 지정한 URI로 토큰을 담아 리다이렉션 시키면 해당 URI를 바탕으로 대쉬보드로 연결만 해준다.

앞서 말한 프론트에서 인증 처리를 거치고 인증 코드만 백으로 받아서 처리하는 과정도 다시 알아봐야겠다.

<br>
<br>
<br>

### ***보완할 수 있을 만한 것들***

이렇게 구현은 끝났지만 아직 보완할 수 있을 것 같은 부분은 많이 남아있다.

<br>

**첫 번째로**, 리다이렉션 URI가 백엔드 서버 주소로 되어 있기 때문에 별도의 url로 리다이렉트 하는 과정 중에 파라미터로 jwt를 포함해서 보낸다.

이 과정 중 파라미터로 토큰을 담아 보내는 것은 기록이 남아있을 수 있는 만큼 굉장히 보안 적으로 안 좋을 것이다.

그래서 쿠키에 토큰을 담아서 특정 주소로 리다이렉션 시키던지, 아니면 아예 헤더에 발급된 토큰을 담아 대쉬보드 API로 직접 요청을 보내는 방법을 생각해보았다.

전자의 경우는 구현이 끝난 당시에는 클라이언트 측 통신 프로토콜이 http라 프로토콜 불일치 문제로 쿠키가 증발해버리는 문제가 있었는데,
현재는 클라이언트도 https 프로토콜로 변경한 만큼 시도해볼 여지가 있다고 생각한다.

후자의 경우는 백단에서 백단으로 요청과 응답을 보내는 것이기 때문에 웹 통신의 개념 상 왠지 꺼려진다.

그래서 추후 특정 url로 리다이렉션 함과 동시에 쿠키에 토큰을 담아 보내는 작업을 진행해볼 예정이다.

<br>

**두 번째로**, 인증 코드를 발급 받아 액세스 토큰을 발급 받고 이메일 정보까지 요청하는 과정을 스프링 시큐리티가 자동적으로 처리할 수 있게 변경해보는 것이다.

지금은 로그인이 성공한 뒤 리다이렉션 URI로 발급되는 인증 코드를 백단에서 직접 소셜 인증 서버에 요청을 보내 액세스 토큰을 발급 받고,
해당 토큰을 헤더에 포함시켜 다시 리소스 서버로 요청을 보내서 사용자의 이메일 정보를 받아온다.

이러한 과정을 OAuth2 로그인 성공 핸들러 메서드나 CustomOAuth2UserService 클래스 등으로 자동화할 수 있을 것이다.

그러면 결국 인증 과정을 자동화하여 백단으로 받는 것은 사용자 정보가 될 것이고, 이를 바탕으로 서비스에 대한 로직만 작성하면 된다.

<br>

**세 번째로**, 현재 소셜 로그인한 사용자를 DB에 저장하여 식별자를 지정하기 위해 중요한 보안 정보인 password가 null이어도 괜찮은 문제가 있다.

로그인 요청 시에 Validation 처리를 했기 때문에 pw가 없다면 로그인이 안되게끔 해놓았지만 뭔가 어색하다.

오히려 보안 정보인 pw가 없어서 다행인 점도 있을 수 있다고 생각은 하고 있지만 찝찝한 느낌을 지울 수 없다.

이 경우 소셜 로그인한 사용자의 pw를 임의로 지정하는 방법도 생각해 보았다.

이 문제에 대해서는 조금 더 생각해봐야할 것 같다.


