---
title: "프로젝트:샐로그 / 보안 2 - Oauth 2.0 구현 과정 2 / 채택한 승인 부여 유형과 사전 작업"
author: Jeremiah Lee
date: 2024-03-08
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

### ***시작하기 앞서***

이번 포스트에서 설명하고자 하는 내용은 Oauth 2.0의 승인 부여 유형과 이 Oauth 2.0을 사용하기 전에 어떠한 작업을 먼저 
진행해야하는 지에 관한 것이다.

여기서 말하는 "Oauth 2.0의 동작 방식 유형"은 이전 [Oauth 2.0 구현 과정 1 / 개념](https://021skyfall.github.io/posts/salog_project_security1_Oauth2.0-1/)
에 등장했던 유형과는 다르다.

해당 포스트에서 말하는 "유형"은 "어떻게 사용할 것인지"에 대한 내용이고, 이번에 말하는 "유형"은 "어떠한 동작 방식을 채택할 것인지"에 대한 내용이다.

또한, 여러 동작 방식 유형이 있지만 현재 프로젝트에 적용된 유형만 설명할 것이다.

<br>
<br>
<br>

### ***Oauth 2.0의 승인 부여 유형***

Oauth 2.0의 승인 방식(Authorization Grant) 유형은 크게 4 가지가 있다.

가장 많이 사용되는 Authorization Code Grant 유형(권한 부여 승인 코드 방식)과 이어서 Implicit Grant 유형(암묵적 승인 방식),
Resource Owner Password Credential Grant 유형(자원 소유자 자격 증명 승인 방식), Client Credential Grant 유형(클라이언트 자격 증명 승인 방식)이 있다.

이 중에서도 가장 기본이 되고 가장 널리 사용되는 Authorization Code Grant 유형을 채택하여 사용했다.   
나머지 세 가지 방식은 각각의 상황에 맞게 사용하는 승인 부여 방식이기 때문에 크게 필요성을 느끼지 못 했다.

Authorization Code Grant 유형은 아래와 같은 흐름으로 동작한다.

![](/assets/img/projects/salog/권한_부여_승인_코드_방식.png)

사실 다짜고짜 흐름도부터 보면 복잡하기 때문에 각 컴포넌트에 대한 설명이 우선이다.

좌측에서 우측으로, 첫 번째의 Resource Owner는 말 그대로 리소스를 가진 사람을 말한다.     
즉, 구글이든, 네이버, 카카오든 해당 플랫폼의 계정의 주인이라는 뜻이며, 
이는 곧 해당 플랫폼을 통해 우리 서비스를 사용하려는 사용자를 말한다.

다음으로 Client는 사용자와 제 3자 인증 서버 사이에서 중계하는 역할을 한다.   
여기서는 우리의 애플리케이션을 의미한다.

보통의 웹 서비스에서, 사용자가 웹 사이트 어떠한 기능에 요청을 보내면 프론트가 해당 요청을 받아 
서버로 전달하고, 요청을 처리한 결과 값을 다시 클라이언트에 보내주어 사용자에게 보여준다.

여기서는 그 중간 다리 역할을 하는 것이 애플리케이션인 것 뿐이다.

세번 째의 Authorization Server는 우리의 애플리케이션 서버를 의미하는 것이 아닌 제 3자 인증 서버를 의미한다.
쉽게 말하면, 구글, 네이버, 카카오 등의 플랫폼에 구축되어 있는 인증 서버를 뜻한다.

마지막으로 Resource Server는 마찬가지로 우리의 애플리케이션 서버가 아니라 인증에 사용된 플랫폼의 리소스 서버를 의미한다.    
남의 백엔드 서버이다.

이 네 가지의 컴포넌트가 각각 상호작용을 진행하여 Oauth 2.0 인증 처리가 이루어진다.

<br>

정리를 하기 위해, 조금 더 설명하기 쉽게 구글을 기준으로   
Resource Owner = 사용자      
Client = 애플리케이션     
Authorization Server = 소셜 인증 서버    
Resource Server = 소셜 리소스 서버    
로 표현하겠다.

<br>

이제 각 흐름을 순서대로 살펴보자.

이해를 돕기 위해 다시 흐름도를 가져왔다.

![](/assets/img/projects/salog/권한_부여_승인_코드_방식.png)

##### **1. 서비스 사용 요청**
사용자는 우리의 웹 애플리케이션에서 소셜 로그인 버튼을 누르는 등, 로그인 서비스 요청을 하게 된다.    

<br>

##### **2. Authorization Code 요청**
로그인 요청을 하게 되면 소셜 인증 서버에 Authorization Code를 요청하게 된다.   
이 때 "사전 작업"으로 미리 생성한 Client ID와 Redirect URI, 응답 타입을 함께 전송하게 된다.    
그러면 요청과 동시에 소셜 로그인 화면이 사용자에게 응답된다.

<br>

##### **3. 로그인**
응답된 소셜 로그인 페이지에서 사용자가 로그인을 진행하게 된다.    
이 때 로그인 정보를 바탕으로 인증 처리를 맡은 인증 서버는 소셜 플랫폼의 인증 서버이다.    

<br>

##### **4. Authorization Code 전달**
전달된 로그인 정보가 일치하여 로그인에 성공하면 소셜 인증 서버는 Authorization Code를 애플리케이션에 전달하게 된다.   
이 경우, 이 전에 요청과 함께 전달한 Redirect URI로 해당 코드를 전달하게 된다.

<br>

##### **5. 액세스 토큰 요청**
이제 애플리케이션은 해당 사용자의 소셜 플랫폼 내에 저장된 "리소스"가 필요한 경우 전달 받은 Authorization Code를
소셜 인증 서버로 보내 Access Token을 요청한다.     
이 때, Authorization Code 뿐만 아니라 "사전 작업" 중 발급 받은 client secret과 redirect uri, 권한 부여 방식을 함께
전송하게 된다.

<br>

##### **6. 액세스 토큰 전달**
애플리케이션에서 전달한 요청 시 필요한 데이터 들이 일치하면 소셜 인증 서버에서는 소셜 리소스 서버 내에 저장된
해당 회원의 "리소스"에 접근할 수 있는
Access Token을 애플리케이션에게 발급해준다.

<br>

##### **7. 자원 요청**
발급 받은 Access Token을 활용하여 애플리케이션은 다시 소셜 리소스 서버로 회원의 정보를 요청한다.

<br>

##### **8. 요청 자원 전달**
소셜 리소스 서버는 해당 Access Token을 확인한 후 요청 받은 정보를 다시 애플리케이션에 전달하게 된다.

<br>

이후에는 해당 "정보"를 가공하여 커스텀 서비스를 제공할 수 있게 된다.

물론 우리는 정보를 가공하여 소셜 관련 커스텀 서비스를 제공하진 않는다.    
단순히 로그인하기 위해서만 사용한다.

그렇다면 왜 자원이 필요한가? 에 대한 부분은 서비스 관련된 내용이 있기 때문에 다음 포스트에서 상세하게 다룰 것이다.    
우선은 앞서 간간히 언급되었던 "사전 작업"에 대해 정리하고 넘어가자.

<br>
<br>
<br>

### ***구글 Oauth 2.0을 사용하기 위한 사전 작업***

기본적으로 이 Oauth 2.0 프로토콜을 사용하려면 어떤 플랫폼에서든 사전 작업이 필요하다.    
결과적으로는 우리 애플리케이션도 "인증"을 하는 과정이라고 생각하면 된다.

현재 "샐로그"에 구현되어 있고 사용 가능한 구글 로그인을 기반으로 설명하겠다.    
다른 플랫폼도 근본적으로는 동일한 방식이기 때문에 큰 차이는 없었다.

<br>

우선은 구글의 Oauth 2.0 시스템을 사용하기 위해 [구글 API 및 서비스 콘솔](https://console.cloud.google.com/apis)에서 client id와 secret을 생성해야한다.

<br>

#### **1. 프로젝트 생성**
가장 먼저 서비스를 사용하기 위해 프로젝트를 생성해야한다.    
다음 순서를 따라 프로젝트를 생성할 수 있다.

##### **a. 프로젝트 만들기**
![](/assets/img/projects/salog/1._프로젝트_생성_.png)

##### **b. 새로운 프로젝트 생성**
![](/assets/img/projects/salog/새로운_프로젝트를_만듭니다..png)

##### **c. 프로젝트 선택**
![](/assets/img/projects/salog/생성한_프로젝트를_선택.png)

<br>

#### **2. OAuth 동의 화면 만들기**
서비스 중 하나인 Oauth 2.0을 사용하기 위해 간단한 정보가 담긴 동의 화면을 생성해야한다.   
다음 순서를 따라 생성 가능하다.

##### **a. OAuth 동의 화면**
![](/assets/img/projects/salog/OAuth_동의_화면.png)

##### **b. User Type**
![](/assets/img/projects/salog/User_Type.png)

##### **c. 앱 등록 수정**
![](/assets/img/projects/salog/앱_등록_수정.png)

##### **d. 테스트 사용자**
![](/assets/img/projects/salog/테스트_사용자.png)

<br>

#### **3. 사용자 인증 정보 생성**
마지막으로 OAuth 2.0 client id와 secret을 생성하기 위한 작업이다.
마찬가지로 아래의 순서를 따라 생성하면 된다.    
여기서 리디렉션 URI의 경우 이 엔드포인트로 우리의 애플리케이션에 param으로 "인증 코드"가 전송되기 때문에 원하는 주소로 작성하면 된다.    
다만, 현재 적혀있는 저 주소 방식이 기본 방식이다.

##### **a. OAuth 동의 화면**
![](/assets/img/projects/salog/사용자_인증_정보.png)

##### **b. User Type**
![](/assets/img/projects/salog/사용자_인증_정보_만들기.png)

##### **c. 앱 등록 수정**
![](/assets/img/projects/salog/OAuth_클라이언트_ID_만들기.png)

##### **d. 테스트 사용자**
![](/assets/img/projects/salog/OAuth_클라이언트가_생성_완료.png)

<br>

마지막으로 중요한 점은 client id와 secret의 경우 애플리케이션 설정 정보(.yml)에 필요한 요소이고, 당연하지만 
해당 정보를 토대로 Oauth 2.0 로그인을 하기 때문에 중요한 보안 요소이다.

이제 OAuth 2.0 로그인 사용 전 준비가 완료 되었다.

<br>
<br>
<br>

***

OAuth 2.0 사용 준비와 기본 개념에 대해 알아보았다.

이제 다음 포스트에서는 실제로 우리 애플리케이션에서 어떻게 구현했는 지, 어떤 어려움이 있었는 지에 대해 작성할 것이다.

또한 다음 포스트를 보면 JWT와 함께 어떻게 구현할 수 있는지도 파악 가능할 것이다.
