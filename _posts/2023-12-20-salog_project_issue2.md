---
title: "프로젝트:샐로그 / 이슈 2 - JWT"
author: Jeremiah Lee
date: 2023-12-20
categories: [ 프로젝트, 샐로그, 문제해결 ]
tags: [프로젝트, 샐로그, 문제해결, 배포 ]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Project_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

이 포스트는 이슈라기 보다는 활용법에 더 가깝다.

회원 보안 관련으로 JWT를 사용하는데, 주로 이전 프로젝트인 wireating의 소스코드를 참고하여 구현했다.

그런데 배포가 끝나고 조금 지나자 생각치도 못한게 있었다.

바로 "로그아웃"과 "토큰 재발급"이다.

당시 wrieating 프로젝트 진행했을 때는 소스코드의 의미를 완벽히 이해하지 못해 이전에 배웠던, 찾아본 내용을 추가하는데에 급급했는데,
이제 어느정도 알게되고 직접 사용해보니 로그아웃, 토큰 재발급에 대한 소스코드가 일절 없던 것이다.

그래서 해당 기능을 추가하였으며, 진행된 내용을 정리하기 위해 이번 포스트를 작성한다.


[깃짱코딩 - JWT](https://engineerinsight.tistory.com/232)

jwt의 로그아웃, 토큰 재발급에 대한 개념은 위 블로그를 참고했다.

<br>
<br>
<br>

### ***1. 로그아웃***

우선 로그아웃과 같은 경우는 프론트 측에서 사용자의 세션을 종료하거나 토큰을 삭제함으로써 완성된다고 알 고 있다.

보통 1차 적인 검증은 프론트에서 이루어진다.

하지만 전적으로 프론트에게만 맡겨놓기는 불안하기 때문에, 탈취되었을 경우 피해를 최소화하기 위한 방법으로 나는 블랙리스트를 활용했다.

다시 말해, 서버에서 2차 검증을 거쳐야 어느정도 안전한 웹 애플리케이션을 구성할 수 있다는 의미이다.

<br>

토큰을 블랙리스트 처리하는 것은 생각보다 간단하다.

효율좋은 자료구조 하나를 사용해 해당 자료구조에 클라이언트가 사용하던 토큰을 저장하고, 요청 접근에 토큰이 필요할 때 블랙리스트에
이 토큰이 들어있는지 파악만 하면 된다.

그래서 블랙리스트 처리하기 위한 클래스를 하나 구현했다.

```java
import org.springframework.stereotype.Component;

import java.util.HashSet;
import java.util.Set;

// 로그아웃 - 토큰 블랙리스트 처리
// todo-2023-12-19 현재 모든 컨트롤러와 결합도가 높아, 인터셉터 혹은 필터를 활용해 선 검증을 거치도록 리펙터링 해야함
@Component
public class TokenBlackListService {
    private Set<String> blackList = new HashSet<>();

    public void addToBlackList(String token) {
        blackList.add(token);
    }

    public void isBlackListed(String token) {
        if (blackList.contains(token))
            throw new BusinessLogicException(ExceptionCode.LOGOUT);
    }
}
```

우선 이 클래스는 auth/utils 패키지에 위치해 있는데, 이는 기능이 여러 곳에서 공통적으로 사용되는 기능이기 때문이다.

다시 말해, 토큰 블랙리스트 처리, 검증은 애플리케이션 전반에서 공통적으로 사용 되기 때문에 유틸리티라고 생각하여 위치 구성을 이렇게 했다.

다시 클래스로 돌아와, 다른 자료구조가 아닌 hashset을 콕 찝어 사용한 이유는 다음과 같다.
1. 검색 성능 : 해시 테이블을 사용하여 요소를 저장하기 때문에 검색이 빠름. 데이터 양에 상관없이 일정한 검색 성능을 유지할 수 있음
2. 중복 요소 제거 : 중복이 허용되지 않기 때문에 같은 토큰이 여러 번 들어와도 하나 만 저장됨

이 두 가지 이유 때문에 hashset을 사용했다.

사실 우리 서비스의 규모로 보면 그냥 적당히 ArrayList를 사용해도 되겠지만, 
항상 어디서 어떻게 될지 모르기 때문에 개발자는 할 수 있는 최선의 선택을 해야한다고 생각했다. (가능하다면)

소스 코드 중 첫 번째 함수는 블랙리스트에 토큰을 저장하는 내용이고
두 번째 함수는 블랙리스트에 매개변수로 입력된 토큰이 들어있다면 커스텀 에러코드를 리턴하게 끔 작성되어 있다.

이 LOGOUT이라는 커스텀 익셉션은 "로그아웃 되었으니 재 로그인 하세요."라는 내용을 리턴한다.

<br>

이 TokenBlackListService 클래스는 기본적으로 로그아웃 API에서 활용되어야 하기 때문에 
MemberController에서 생성자 주입을 통해 사용한다.

```java
@RestController
@RequestMapping("/members")
@Validated
@AllArgsConstructor
@Slf4j
public class MemberController {
    private final MemberService memberService;
    private final TokenBlackListService tokenBlackListService;
    
  ...

    // todo 2023-12-19 로그아웃 시 토큰 블랙리스트에 리프레쉬도 추가해야함 (리프레쉬 탈취 방지)
    // 로그아웃
    @PostMapping("/logout")
    public String logout(@RequestHeader(name = "Authorization") String token) {
        tokenBlackListService.addToBlackList(token);
        return "redirect:/";
    }
}
```

로그아웃 통신은 이런식으로 구성되어 있다.

코드는 간단하다. 

사용자의 헤더에 있는 토큰을 받아와서 해당 토큰을 블랙리스트 처리하고, 명시적으로 메인 화면으로 돌려보내라는 문자열을 리턴한다.

사실 리턴 값은 void를 리턴하게끔해서 응답이 없게 만들어도 되지만 프론트가 알기 쉽게하려고 작성했다.

이 로직을 통해 로그아웃을 구현했고,

```java
    @GetMapping("/get")
    public ResponseEntity getMember(@RequestHeader(name = "Authorization") String token) {
        tokenBlackListService.isBlackListed(token); // 로그아웃 된 회원인지 체크
        MemberDto.Response response = memberService.findMember(token);
        return new ResponseEntity<>(
                new SingleResponseDto<>(response), HttpStatus.OK);
    }
```

이런 식으로 기능 접근에 토큰이 필요한 컨트롤러 메서드에서 해당 토큰이 플랙리스트 처리되어 있진 않은 지 체크한다.

모든 메서드에 대해 이런 식으로 토큰 블랙리스트를 먼저 처리하기 때문에 로그아웃 되었다면 기능에 접근할 수 없다.

<br>

이로써 프론트에서 토큰을 삭제하여 로그아웃 처리, 서버에서는 해당 토큰을 블랙리스트 처리함으로 로그아웃에 대한 2중 처리가 끝났다.

<br>

### ***1-1 아쉬운 점***

다만 위 로직에서 아쉬운 점이 두 가지가 있다.

##### *첫 번째는*

로그아웃 시 리프레쉬 토큰도 블랙리스트에 저장하지 않는다는 점이다.

이 방식을 구현할 때는 생각하지 못 했는데, 이 경우, 리프레쉬 토큰이 탈취되면 액세스 토큰을 재발급 받을 수 있게 되기 때문에 보안 상 문제가 생긴다.

이를 방지하기 위해서는 액세스 토큰과 이 엑세스 토큰의 리프레쉬 토큰을 받아 둘 다 블랙리스트 처리해야한다.

하지만 이어서 두 번째 문제가 있다.

##### *두 번째는*

현재 이 로그아웃 후 토큰이 블랙리스트 처리되었는지 검증하는 메서드를 모든 컨트롤러의 모든 메서드에서 사용해야한다는 점이다.

즉, 이 로그아웃 로직에 변경이 있다면 하나하나 찾아서 고쳐주어야한다.

첫 번째 문제에서 리프레쉬 토큰도 블랙리스트 처리를 하게 된다면 모든 컨트롤러의 모든 메서드에서 리프레쉬 토큰을 받게하던, 액세스 토큰을 바탕으로 리프레쉬 토큰을 찾아야한다.

의존성이 크게 결합되어 있다고 할 수 있다.

이를 해결하기 위해 인터셉터나 필터를 활용하여 중복된 메서드들을 제거하고 토큰이 필요한 로직 진행 이전에 블랙리스트부터 체크하게 끔 리팩터링할 예정이다.

<br>
<br>
<br>

### ***2.토큰 재발급***

다음은 토큰 재발급 로직이다.

기존에 참고했던 프로젝트에서는 이 리프레쉬 토큰을 바탕으로 액세스 토큰을 재발급해주는 로직이 없었기 때문에 구현하게 되었다.

이 로직이 없다면 사실 리프레쉬 토큰 발급은 의미가 없다.

리프레쉬 토큰은 액세스 토큰을 발급 받을 때 동시에 발급되게 끔 작성해놓았었고, 이를 활용할 차례이다.

우선 리프레쉬 토큰을 받아와 새로운 액세스 토큰을 발급 받는 과정을 먼저 설명한다.

```java
    public Map<String, String> tokenReissue(String refreshToken) {
    // 리프레쉬 토큰 검증
    Claims claims;
    try {
        claims = this.getClaims(refreshToken, this.encodeBase64SecretKey(this.secretKey)).getBody();
    } catch (ExpiredJwtException e) {
        throw new BusinessLogicException(ExceptionCode.TOKEN_EXPIRED);
    } catch (JwtException e) {
        throw new BusinessLogicException(ExceptionCode.TOKEN_INVALID);
    }

    // 사용자 정보 조회
    String email = claims.getSubject();
    Optional<Member> member = memberRepository.findByEmail(email);

    // 조회된 사용자 정보를 바탕으로 클레임 생성
    Map<String, Object> newClaims = new HashMap<>();
    newClaims.put("roles", member.get().getRoles());
    newClaims.put("memberId", member.get().getMemberId());
    newClaims.put("username", member.get().getEmail());

    // 새로운 액세스 토큰 발급
    Date expiration = this.getTokenExpiration(this.accessTokenExpirationMinutes);
    String newAccessToken = this.generateAccessToken(newClaims, email, expiration, this.encodeBase64SecretKey(this.secretKey));

    // 새로운 리프레쉬 토큰 발급
    expiration = this.getTokenExpiration(this.refreshTokenExpirationMinutes);
    String newRefreshToken = this.generateRefreshToken(email, expiration, this.encodeBase64SecretKey(this.secretKey));

    Map<String, String> tokens = new HashMap<>();
    tokens.put("accessToken", "Bearer " + newAccessToken);
    tokens.put("refreshToken", newRefreshToken);

    return tokens;
}
```

이 메서드는 JwtTokenizer 클래스에 위치해 있는데, 이는 JWT 토큰과 관련된 로직을 처리하는 클래스이기 때문에 재발급 로직 역시 이 클래스에서
처리하는 게 맞다고 보았기 때문이다.

다시 말해, JWT 토큰 관련 책임은 JwtTokenizer 클래스가 지고 있다고 보는게 맞다.

위의 재발급 로직을 살펴보면, 우선은 리프레쉬 토큰을 매개 변수로 입력 받아, 이 리프레쉬 토큰에서 클레임을 추출한다. 

이 추출 과정 중에 토큰이 만료되었거나, 유효하지 않은 토큰이라면 각 커스텀 에러코드를 리턴한다.

그 다음으로 이 클레임에 담겨있는 사용자 이메일을 추출해 회원 DB에서 해당 회원을 조회한다.

조회한 다음 액세스 토큰을 재발급하기 위해 필요한 정보를 뽑아 새로운 클레임을 생성한다.

그 다음 액세스 토큰의 생존 기간을 다시 설정하고 액세스 토큰을 발급한다.

마찬가지로 리프레쉬 토큰도 재발급한다.

<br>

여기서 주의해야할 점은 리프레쉬 토큰의 클레임 정보와 액세스 토큰의 클레임 정보는 다르다는 점이다.

애초부터 생성할 때 리프레쉬 토큰에는 간단한 정보만을 담고 있게 만들었다.

만약 헷갈린다면, [jwt 디코더](https://jwt.io/)에서 토큰을 넣고 확인하는 방법을 사용하자.

이 정보를 제대로 확인하지 않고서는 유효한 액세스 토큰을 발급할 수 없다.

<br>

우선 내가 생성한 액세스 토큰과 리프레쉬 토큰의 클레임은 아래와 같이 다르다.

1. 액세스 토큰
![](/assets/img/projects/salog/jwtProblem-decoder1.png)

2. 리프레쉬 토큰
![](/assets/img/projects/salog/jwtProblem-decoder2.png)

<br>

처음에는 내가 만들어놓고도 헷갈려서 디코더를 사용했고 이 디코딩 된 클레임을 바탕으로 액세스 토큰을 재발급했다.

<br>

이 다음에는 클라이언트와 통신하여 토큰을 재발급 해줄 수 있어야한다.

API가 필요하다.

```java
@RestController
@AllArgsConstructor
public class TokenController {
    private final JwtTokenizer jwtTokenizer;
    private final TokenBlackListService tokenBlackListService;

    @PostMapping("/refresh")
    public ResponseEntity<?> refresh(@RequestBody Map<String, String> payload) {
        String oldRefreshToken = payload.get("refreshToken");

        tokenBlackListService.isBlackListed(oldRefreshToken);

        // 기존 Refresh 토큰의 유효성을 검사하고, 새로운 Access 토큰과 refresh 토큰을 생성
        Map<String, String> tokens = jwtTokenizer.tokenReissue(oldRefreshToken);

        // 기존 Refresh 토큰을 블랙리스트에 추가
        tokenBlackListService.addToBlackList(oldRefreshToken);

        return ResponseEntity.ok(tokens);
    }
}
```

그렇기 때문에 auth 패키지에 controller를 두었다.

이 컨트롤러를 활용해 통신하고, 토큰 재발급 메서드는 바디에 맥세스 토큰과 리프레쉬 토큰을 받아온다.

사실 리프레쉬만 받아도 되는데, 일단 나는 처음 발급해준 그대로 전부 복사하여 요청할 것이라 생각했기 때문에 둘 다 포함하는 로직으로 작성했다.

이 바디에서 리프레쉬 토큰만 추출해 가장 먼저 이 리프레쉬 토큰이 블랙리스트에 포함되어 있는지 부터 확인한다.

이미 이 리프레쉬 토큰으로 액세스 토큰이 발급되었다면 에러를 리턴할 것이고, 추후 로그아웃 시 리프레쉬 토큰도 블랙리스트에 등록하게 된다면 동일하게 에러를 리턴하게 될 것이다.

다음으로 아까 작성한 JwtTokenizer의 토큰 재발급 메서드를 활용하여, 리프레쉬 토큰을 검증하고, 액세스 토큰과 리프레쉬 토큰을 재발급 받는다.

이후 사용된 리프레쉬 토큰은 블랙리스트에 추가하고 해당 토큰들을 클라이언트에 리턴한다.

<br>

초기에는 액세스 토큰은 JwtTokenizer에서 생성하고, 리프레쉬 토큰은 TokenController에서 생성되는 등 좀 엉켜있었어서

이 글을 작성하면서 리팩토링을 거쳤다.

그래서 현재는 토큰 검증, 생성 로직은 전부 JwtTokenizer로 넘기고, TokenController에서는 블랙리스트 확인과 Tokenizer로 검증과 생성의 책임을 전부 넘겼다.

또한, 이 블랙리스트 확인도 추후 로그아웃 시 리팩터링 된다면 이 컨트롤러는 통신에 대한 책임만 지게 될 것이다.

<br>
<br>
<br>

***

이로써 로그아웃과 토큰 재발급에 대한 로직이 끝났다.

처음 시도해보는 로그아웃과 토큰 재발급 기능이기 때문에 아직 부족한 점이 많이보인다.

특히 앞서 얘기했던 문제점이나 토큰의 클레임 내용이 헷갈려 재작성을 여러 번 거치는 등 수정이 많이 이루어졌다.

그냥 쉽게 써내려간 것 처럼 보이지만 하루종일 고민했다...

로직들을 어디에 배치할 것인가에 대한 생각도 많이 했다.

한편으로는 추후에 내용들을 다시 읽어보고 핸들링할 수 있을 거라 생각하니 든든하기도 하다.
