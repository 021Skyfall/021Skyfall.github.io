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

이 LOGOUT이라는 커스텀 익셉션은 "로그아웃 되었으니 재 로그입 하세요."라는 내용을 리턴한다.

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
