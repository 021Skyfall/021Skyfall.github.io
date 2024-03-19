---
title: "프로젝트:샐로그 / 보안 1 - Oauth 2.0 구현 과정 1 / 개념"
author: Jeremiah Lee
date: 2024-03-06
categories: [ 프로젝트, 샐로그, 보안 ]
tags: [프로젝트, 샐로그, 보안, Oauth ]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Project_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

사실 Oauth 2.0을 구현한 것은 2월 중이었다.   
당시에 프로젝트를 마무리하느라 블로깅을 하지 못해 지금와서 정리한다.   
그리고 당초 생각한 기능을 마무리했다는 것이지 프로젝트를 "완성"한 것은 아니다.

테스트 케이스 작성이 미숙해 아직 테스트 케이스를 작성하지 못 했고, 그에 따라 API 문서화도 해보지 못했다.   
또한 리펙터링이 필요한 부분이 많이 남아있고, 성능 테스트도 아직 남아있다.   
그래서 개선 목표를 두고 있긴한데, 일단은 이전에 구현했던 내용들을 정리하고자 한다.

<br>

우선 Oauth 2.0 구현이 먼저였기 때문에 Oauth 2.0 구현 과정과 문제 해결 과정을 작성하고
그 다음으로 HTTPS 프로토콜 수동 적용에 관해 작성할 예정이다.

<br>

그리고 블로깅 방식에 대해 조금 생각이 변했다.   
이전 배포에 관련된 내용, 혹은 이전 블로그에서 기술했던 내용들을 보면 한 페이지에 꽤 길게 이어서 작성했었다.

그런데, 해당 방식으로 작성하게 되면 나 뿐만 아니라 다른 이들도 읽기 힘들고 어려울 것이라는 생각이 들었다.   
그래서 이 포스트를 기점으로 내용이 길어진다 싶으면 분할해서 작성할 예정이다.

그 편이 내가 작성하기도 편하고, 읽기도 편하리라 생각한다.

<br>
<br>
<br>

### ***시작하기 앞서***

소셜 로그인 기능에 대한 내용이 내가 생각하던 것 보다 어렵고 복잡해서 거의 프로젝트 막바지에 구현했다.   
우선 기술을 사용하기 위해서는 그 기술이 무엇인지에 대해 알아야 한다.   

개념에 대해 알아내기 위해 시간이 꽤 걸렸고, 현재 서비스에 구글 로그인만 구현해놓았다.   
애초에 개념을 이제 알게 되었으니 카카오, 네이버 로그인도 마찬가지로 구현할 수 있으리라 생각해 조금 더 미뤄두었다.

<br>

처음에는 스프링 시큐리티 설정 클래스에서 HttpSecurity 클래스의 메서드 중 Oauth2Login 메서드만 사용하면
자동적으로 Oauth 2.0이 적용되는 줄 알았다.

결과적으로는 틀린 얘기는 아니지만, 구글 서버에 요청하여 로그인한 회원의 정보를 받아온다던가 해당 정보를 바탕으로
JWT를 생성해주어야 한다던가, 이런 내용을 몰랐기 때문에 많이 헤맸다.

고작 메서드 하나 사용한다고 내가 생각한대로 마법처럼 이루어진다는 것 자체가 지금 생각하니 어이가 없다.

<br>

앞서 말했듯이 스프링 시큐리티에서 설정을 하면 Oauth 2.0 프로토콜로 로그인을 할 수 있게 된다.

그러나 어떤 플랫폼을 사용할 것인지, 어떻게 유저의 정보를 그 플랫폼에서 받아올 것인지, 그리고 자체적으로 구현한 JWT와
어떻게 연결을 시킬 것인지에 대한 부분들은 순전히 개발자의 몫이다.

그래서 이번 포스트는 어떤 흐름을 거쳐 소셜 로그인을 구현했는지에 대해 집중적으로 다룰 것이다.

다만, 분할 작성법으로 접근한다고 했으니 이번 포스트는 개념에 대해, 다음 포스트는 적용과 문제해결에 대해 작성할 것이다.

개념에 대한 상당 부분은 이전 블로그의 내용이 정리가 잘 되어 있기 때문에 해당 내용을 갈무리해서 작성할 것이고,
그 부분은 JWT와 마찬가지로 따로 이사시키지 않을 예정이다.

<br>
<br>
<br>

### ***OAuth 2.0이란?***

추후 별개의 포스트로 Oauth 2.0 에 대해 자세히 알아볼 것이기 때문에 간단히 이번에 활용된 OAuth 2.0이 무엇인지만 짚고 넘어갈 것이다.

OAuth 2.0는 정말 간단하게 말하면, 소셜 로그인 인증 방식을 구현할 수 있는 기술이다.

일반적인 애플리케이션 인증 처리 방식은 서비스를 사용하는 사용자의 크리덴셜을 백엔드 인증 서버에서 직접 관리하지만
OAuth 2.0의 경우는 약간 다르다.

![](/assets/img/projects/salog/전통적인_애플리케이션에서_사용자의_크리덴셜을_저장하는_아키텍처.png)

<br>

위와 같은 방식이 전통적인 애플리케이션의 인증 처리 방식이다.

일반적으로 JWT를 예를 들어, 회원의 인증 정보를 클라이언트로 부터 받아오고, 이를 백엔드 서버 크리덴셜 저장소에 저장한 다음 해당 정보를 바탕으로
JWT를 클라이언트에게 응답하여 인증, 인가처리를 거친다.

그런데 이 경우의 문제점은 만약 제 3자 플랫폼, 구글이나 네이버, 카카오 등의 플랫폼 서비스를 우리 서비스 측에서 사용해야한다고 할 때,
사용자는 해당 플랫폼의 로그인 정보를 우리 서비스에 등록해야한다.

다시 말해 불필요하고 불안하게 우리한테 별개의 플랫폼 개인 정보를 전달해야한다는 것이다.

마찬가지로 우리 측에서도 사용자의 개인 정보를 두 배로 저장해야하기 때문에 찝찝하다.
우리 서버가 털리면 모든 회원의 구글, 카카오, 네이버 정보도 같이 털리는데 말이 안된다.

이를 해결하기 위해 등장한 것이 Oauth 1.0 라는 기술이다.

Oauth 1.0에서 더욱 발전하여 훨씬 유연하고 확장성이 뛰어나며, 구현이 간단해진 업그레이드 버전이 Oauth 2.0이며,
현재까지 가장 널리 사용되는 인증 표준 중 하나이다.

이러한 Oauth 2.0은 아래와 같이 전통적인 인증 처리 방식과는 다르다.

![](/assets/img/projects/salog/오오스_인증_방식.png)

<br>

클라이언트 측에서 구글 Oauth 2.0 인증 요청을 시도하면 구글이 제공하는 로그인 페이지로 리다이렉트되고, 말 그대로 구글에 로그인하게 된다.
구글 보안 서버에서는 이 사용자의 로그인 정보를 바탕으로 자신들의 서비스를 사용할 수 있도록 백엔드 서버로 Access Token을 전송하게 되고
백엔드 서버에서는 이 Access Token을 가지고 구글 서버에 해당 사용자의 정보를 요청할 수 있다.

위 그림의 예를 들면 해당 인증 처리된 사용자의 구글 캘린더 정보를 받아온다던가 하는 식으로 말이다.

이와 같이 처리된다면 우리 측 백엔드 서버에서는 크리덴셜 저장소에 해당 회원의 로그인 인증 정보를 저장하지 않아도 된다.

간편하게 구글로 로그인 되었으며, 관리하는 크리덴션일 줄어 들었다는 의미는 그만큼 보안성이 향상되었다는 의미와 같다.

<br>
<br>
<br>

### ***OAuth 2.0을 사용하는 애플리케이션의 유형***

앞서 설명한 Oauth 2.0 인증 처리 방식은 써드 파티 애플리케이션에서 제공하는 API를 직접적으로 사용하기 위한 방식이다.   
해당 방식은 보통 별도의 플랫폼에서 제공하는 API를 활용한 커스텀 서비스를 제공하는 경우 사용된다.

그렇다면 우리의 Salog는 구글 API를 사용하는가? 아니다.   
그냥 단순히 추가적인 인증 서비스 제공 용도이다.   

다시 말해, 그냥 간편 로그인을 위해 Oauth 2.0을 활용한 것일 뿐이다.
기존의 아이디/패스워드를 활용한 로그인 인증 외에 추가적인 방법을 제공하여 사용자 편의성을 향상시키기 위함이다.   

그럼 어떻게 구현했는가? 에 대한 내용은 다음 포스트에 작성하겠다.

<br>
<br>
<br>

***

이번 포스트는 Oauth 2.0에 대한 간략한 설명과 우리 프로젝트에서는 어떤 유형을 채택했는지에 대한 내용이다.

다음 포스트에서는 어떻게 구현했는지를 다룰 것이다.