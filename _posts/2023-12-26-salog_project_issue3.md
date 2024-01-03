---
title: "프로젝트:샐로그 / 이슈 3 - 에러 응답"
author: Jeremiah Lee
date: 2023-12-26
categories: [ 프로젝트, 샐로그, 문제해결 ]
tags: [프로젝트, 샐로그, 문제해결, 비지니스 에러 ]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Project_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

이전, Wrieating 프로젝트 진행 당시 멘토님에게 들었던 얘기가 있었다.

"에러 응답이 너무 불친절하다."는 얘기였는데,
이번 기회에 내가 담당했던 회원 관련에서 에러 응답을 신경쓰도록 노력했다.

회원 뿐만 아니라 일기 관련 파트를 담당하고 있는 팀원에게도 리턴되는 에러를 좀 더 친절하게 변경해보자고 장려했고,
현재 진행 중인 가계부 도메인 관련 로직에서도 튼튼하게 가져갈 생각이다.

사실 이 이슈는 이슈라기 보다는 통찰에 가깝지만 일단 프론트의 피드백이 들어오고 나서 수정이 가해진 것이기 때문에 이슈로 분리했다.

특히, 회원 쪽을 담당하는 만큼 회원 관련해서 작성할 것이다.

<br>
<br>

### ***1. 계기***

앞서 말했듯이, 가장 먼저 "친절한 에러 응답"에 대해 생각해본 계기는 프론트의 피드백이다.

회원 관련 테스트 진행 중, 특정 에러들에 대해 응답이 어떻게 와야하는지 대화를 나누고부터 신경 쓰이기 시작했다.

이때 기억 난 것이, 이전에 받았던 멘토님으로부터의 피드백인 "불친절하다."이다.

우선은 프론트 팀원에게 피드백 받았던 부분과 이후 어떻게 수정해 나갔는지, 보안과 관련해 중요한 부분인 만큼 내가 어떤 것을 느꼈는 지를 작성할 것이다.

<br>
<br>

### ***2. 발생***

프론트 팀원이 피드백을 준 것은 로그인 실패 시의 에러 응답이다.

이 때 응답 코드가 전부 동일하게 401 Unauthorized 였는데, 이 경우가 맞는지 생각이 들었다.

왜냐하면 로그인 시 이메일, 혹은 비밀번호가 틀렸을 경우 둘 다 Unauthorized 가 리턴되기 때문에 클라이언트 측에서 
어떤 정보가 틀린 것인지 인지하기 어려웠기 때문이다.

특히 프론트에서 분기를 하지 못하기 때문에 나에게 질문을 던진 것이라 생각한다.

그래서 이 부분에 대해서는 로그인 시 이메일이 틀린 경우와 비밀번호가 틀린 경우를 분기하여 서로 다른 에러 응답이 리턴되어야한다고 생각했다.

이 응답을 변경하기 위해서는 기존에 작성된 인증 실패 핸들러 클래스에서 수정을 해야했고, 이 커스텀 핸들러를 건들면서 이것저것 알게되었다.

<br>
<br>

### ***3. 시도와 변경***

앞서 말했듯이 로그인 실패, 즉 인증 실패에 대한 핸들링은 회원 인증 실패 핸들러에서 담당하고 있기 때문에 해당 클래스를 먼저 찾았다.

MemberAuthenticationFailureHandler 는 문자 그대로 회원 인증 실패 시 핸들링하는 클래스인데,
이 클래스는 스프링 시큐리티의 AuthenticationFailureHandler 인터페이스의 구현체이다.

나는 처음에 onAuthenticationFailure 메서드를 구현하고 이 메서드를 활용해 에러 응답 코드를 작성하기 위해
sendErrorResponse 메서드도 구현했다.

이 로그인 시 잘못된 정보에 대한 분기를 하기 위해 살펴보았을 때, 즉, 처음에는 단순히 401 Unauthorized 만 리턴되게 되어 있었다.

```java
@Slf4j
public class MemberAuthenticationFailureHandler implements AuthenticationFailureHandler {
    @Override
    public void onAuthenticationFailure(HttpServletRequest request,
                                        HttpServletResponse response,
                                        AuthenticationException exception) throws IOException {
        // 인증 실패 시, 에러 로그를 기록하거나 error response를 전송할 수 있다.
        log.error("# Authentication failed: {}", exception.getMessage());

        sendErrorResponse(response);
    }

    private void sendErrorResponse(HttpServletResponse response) throws IOException{
        Gson gson = new Gson();
        ErrorResponse errorResponse = ErrorResponse.of(ExceptionCode.MEMBER_NOT_FOUND);
        response.setContentType(MediaType.APPLICATION_JSON_VALUE);
        response.setStatus(HttpStatus.UNAUTHORIZED.value());
        response.getWriter().write(gson.toJson(errorResponse, ErrorResponse.class));
    }
}
```

코드에서 알 수 있듯이 AuthenticationFailureHandler의 구현체인 MemberAuthenticationFailureHandler 클래스인데,
구현된 onAuthenticationFailure 메서드에서 sendErrorResponse 메서드를 사용해 고정적으로 Unauthorized status 만 넣어 보내주고 있다.

여기서 이메일, 비밀번호에 대한 인증 실패를 분기하기 위해 아래와 같이 수정했다.

```java
@Slf4j
public class MemberAuthenticationFailureHandler implements AuthenticationFailureHandler {
    @Override
    public void onAuthenticationFailure(HttpServletRequest request,
                                        HttpServletResponse response,
                                        AuthenticationException exception) throws IOException {
        // 인증 실패 시, 에러 로그를 기록하거나 error response를 전송할 수 있다.
        log.error("# Authentication failed: {}", exception.getMessage());

        if (exception instanceof BadCredentialsException) {
            // 비밀번호가 잘못된 경우
            sendErrorResponse(response, ExceptionCode.PASSWORD_MISMATCHED);
        } else {
            sendErrorResponse(response, ExceptionCode.MEMBER_NOT_FOUND);
        }
    }

    private void sendErrorResponse(HttpServletResponse response, ExceptionCode exceptionCode) throws IOException{
        Gson gson = new Gson();
        ErrorResponse errorResponse = ErrorResponse.of(exceptionCode);
        response.setCharacterEncoding("UTF-8"); // 한글 인코딩
        response.setContentType(MediaType.APPLICATION_JSON_VALUE);
        response.getWriter().write(gson.toJson(errorResponse, ErrorResponse.class));
    }
}
```

응답 status로 고정적으로 넣어주던 Unauthorized 상태코드를 삭제하고 에러 코드를 직접 sendErrorResponse 함수에 매개변수로 입력하게끔 변경하였다.

이제 sendErrorResponse 함수 사용 시 두 번째 매개변수로 직접 작성한 에러 코드를 함께 삽입해 주면 커스텀 가능하다.

다음으로는 비밀번호가 틀린 경우와 이메일 주소, 아이디가 틀린 경우 두 가지 경우를 분기하여 각각 커스텀한 에러 코드를 리턴해야한다.

우선 if문의 조건으로 삽입된 BadCredentialsException는 클라이언트가 제공한 자격 증명 정보가 잘못되었을 때 발생하는 예외이다.

이를 이용해 비밀번호가 틀렸을 경우의 에러 응답 코드를 리턴한다.

하지만 주의해야할 점은 BadCredentialsException은 말 그대로 크레덴셜, 자격 증명이 잘못되었을 때 발생하기 때문에, 이메일이 잘못되었을 경우에도 발생할 수 있다.

그러나,

```java
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        Optional<Member> optionalMember = memberRepository.findByEmail(username);
        Member findMember = optionalMember.orElseThrow(() -> new BusinessLogicException(ExceptionCode.MEMBER_NOT_FOUND));

        return new MemberDetails(findMember);
    }
```

앞선 UserDetailsService의 구현체인 MemberDetailsService 클래스에서 위와 같이 크레덴셜을 생성할 때 유저를 먼저 찾게되는데,
해당 유저가 존재하지 않으면 크레덴셜 자체가 생성되지 않고 먼저 MEMBER_NOT_FOUND 예외를 발생시킨다.

반면에, 비밀번호가 틀린 경우 예외를 발생시키는 BadCredentialsException 의 경우 회원 정보를 찾아서 userDetails 객체를 생성하였지만,
사용자가 입력한 비밀번호와 userDetails의 비밀번호가 다르기 때문에 발생한다.

그렇기 때문에 인증 실패 처리 핸들러로 전달이 되고 예상한대로 예외 코드를 리턴할 수 있는 것이다.

다시 말해, 에러 발생 시점이 다른 것이다.

<br>

이렇게 분기하여 이메일이 잘못된 경우와 비밀번호가 잘못된 경우를 분기하였지만, 아쉬운 점은 인증 실패 처리 핸들러에서 else로 들어갈 수 있는 예외 상황이
다른 것도 있는 것이다.

이 "다른 상황"에는 인증 서버 문제도 있는 등 몇 가지가 더 있다. 

그래서 추후에는 해당 별개의 에러들을 else로 묶어 server 에러로 리턴할 예정이고, 조건을 한 가지 더 분기하여 BusinessLogicException 을 거르는 조건을 만들 예정이다.

<br>
<br>

### ***4. 또 다른 문제와 추후에는***

위에서 설명된 방식으로 처리하여 클라이언트 측에서 로그인 시 이메일이 틀렸는지, 혹은 비밀번호가 틀렸는지를 쉽게 파악할 수 있게 되었다.

그러나 한 가지 마음에 걸리는 점은 "보안"이다.

아무리 생각해봐도 이렇게 너무 명확히 구분을 지어주면 사용자 정보를 탈취하려는 공격자 입장에서 좋은 일을 했다고 밖에 생각이 들지 않는다.

클라이언트가 알아보기 쉬운 만큼 해커도 알아보기 쉬워졌기 때문에 스스로 딜레마에 빠졌다.

목적은 "불친절한 에러 응답을 고치자."였기 때문에 클라이언트 친화적으로 응답 코드를 변경했는데,
이렇게 됨으로써 보안이 취약해져 브루트 포스 공격 등을 받아 문제가 생길 수도 있다.

보통은 "이메일 혹은 비밀번호가 틀렸습니다."라는 중의적인 표현으로 응답을 한다고 한다.

나 역시도 단순히 ID_OR_PASSWORD_NOT_MATCH 과 같은 응답 코드를 리턴할 수도 있을 것이다.

아직까지도 이 문제에 대한 해결을 생각 중이지만 특별히 이렇다 할 방법이 떠오르진 않는다.

<br>

정리하면, "친절한 에러 코드는 공격자도 알아보기 쉽다."가 될 것이다.

에러 응답을 친절하게 모든 것을 알려주는 것이 오히려 보안에는 독이 될 수 있다.

그렇다면 어떻게 해야하는가?

이는 솔직히 혼자 결정할 문제가 아니라고 생각한다.

요구 사항에 다 맞추어 구현하고 수정하고 서비스하고 있다면, 팀원들에게 특정 방법에 대해 어떤 문제가 있는지 등 
충분히 설명을 한 후에 함께 결정하는 것이 맞다고 생각한다.

프로그래밍은 혼자하는 것이 아니기 때문에 팀원들과의 의사소통도 신경써야 한다.

추후 리팩토링 과정 중 팀원들과 이에 대한 내용을 변경할지 다시 한번 상의해볼 것이다.

<br>
<br>
<br>

***

사실 코드를 변경하는 것은 그리 오래 걸리지 않았다.

원래 처음에는 "로그인 할 때 그냥 Unauthorized 만 리턴되는게 맞아요?"라는 말을 듣고서
실패 시 핸들링하는 핸들러로 가서 곧바로 변경했기 때문이다.

물론 예외 발생 시점에 대한 게 헷갈려서 detailsService 등을 여러 번 왔다갔다하면서 헤매긴 했지만.

하여튼 수정하여 에러를 친절하게 리턴하는 것 보다 오래 고민한 것이 "보안" 관련이다.

앞서 말했듯, 저런 문제들이 있어서 어떻게 해야하는지를 훨씬 오래 고민한 것 같다.

보안 vs 친절...

솔직한 마음으로는 보안이 먼저라고 생각한다. 친절도 좋지만 보안이 뚫려버리면 손실이 훨씬 클 것이라고 생각하기 때문이다.

어찌됐던 아직까지 보안을 어떻게 신경써야하는지 어떻게 대응해야하는지가 많이 미숙하다.
