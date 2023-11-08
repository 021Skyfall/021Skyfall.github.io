---
title: "8/09 면접 준비"
author: Jeremiah Lee
date: 2023-08-09
categories: [ 면접 준비, 스터디 ]
tags: [취준 스터디]
pin: true
math: true
mermaid: true
image: "/assets/img/StudyGroupLog/interview_jelly.png"
#path: /commons/devices-mockup.png
#lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#alt: Responsive rendering of Chirpy theme on multiple devices.
---
***

### **Part 1-1 Development common sense**
1. 좋은 코드란 무엇인가
2. 객체 지향 프로그래밍이란 무엇인가
3. 객체 지향 개발 원칙은 무엇인가?
4. RESTful API 란
5. TDD 란 무엇이며 어떠한 장점이 있는가
6. 함수형 프로그래밍
7. MVC 패턴이란 무엇인가?
8. Git 과 GitHub 에 대해서
   <br><br>
   <br><br>

#### **1. 좋은 코드란 무엇인가**
- 좋은 코드는 지극히 주관적인 문제이다. 보통 읽기 쉬운 코드, 중복이 없는 코드, 테스트가 용이한 코드라고 하는데, 개인적으로 이 전체를 아우르는 말이 있다고 생각한다. 바로, "회사에 돈을 벌어다 주는 코드이다.". 돈을 벌어다 준다는 것이 모호하게 들릴 수 있겠지만 앞서 말한 세 개의 조건을 합친 것이라 생각한다. 첫번 째로 읽기 쉬운 코드는 가독성이 뛰어나다는 것이기 때문에 나만이 아니라 다른 사람도 보기 편할 것이고, 혼자서 하는 개발이 아닌 이상 협업하기 굉장히 용이할 것이다. 이런 코드는 협업에 용이하기 때문에 코드리뷰의 불필요한 질의응답을 줄일 수 있으므로 커뮤니케이션이 수월해지고, 그로 인한 시간 소요가 적어질 것이다. 이로써 돈을 벌어다 주는 코드라고 할 수 있다. 다음으로 중복이 없는 코드는 같은 코드를 반복할 필요가 없으므로 코드 구현에 대한 시간이 줄어들고, 공통적인 기능을 하나로 묶을 수 있기 때문에 수정이 필요하더라도 금방 다시 찾아 수정할 수 있다. 그렇기 때문에 리소스 소비가 줄어들며, 돈을 벌어다 준다고 할 수 있다. 마지막으로 테스트가 용이한 코드라함은 기능에 대한 테스트 케이스를 구현하기 쉬운 코드인데, 이것도 마찬가지로 안정성이 높다는 의미이기 때문에 추후 에러가 발생하거나 하는 일이 적다는 것이고 그 만큼 해당 기능에 대한 유지보수가 필요한 순간이 줄어든다는 의미이다. 이렇기 때문에 제시된 좋은 코드들은 모두 회사에 돈을 벌어다 줄 수 있다고 생각한다.
  <br><br>
  <br><br>

#### **2. 객체 지향 프로그래밍이란 무엇인가?**
<https://jongminfire.dev/%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%EC%9D%B4%EB%9E%80>
- 객체 지향 프로그래밍은 컴퓨터 프로그래밍의 패러다임 중 하나이다. 객체 지향 프로그래밍은 컴퓨터 프로그램을 명령어의 목록으로 보는 시각에서 벗어나 여러 개의 독립된 단위, 즉 "객체"들의 모임으로 파악하고자 하는 것이다. 각각의 객체는 메시지를 주고받고, 데이터를 처리할 수 있다.
  <br><br>
  <br><br>

#### **3. 객체 지향 개발 원칙은 무엇인가?**
<https://hckcksrl.medium.com/solid-%EC%9B%90%EC%B9%99-182f04d0d2b>
- SOLID 원칙 : 객체 지향 프로그래밍을 하면서 지켜야하는 5대 원칙
- SRP(Single Responsibility Principle) : 단일 책임 원칙
   * 클래스는 단 하나의 책임을 가져야 하며 클래스를 변경하는 이유는 단 하나의 이유이어야 한다.
- OCP(Open-Closed Principle) : 개방-폐쇄 원칙
   * 확장에는 열려 있어야 하고 변경에는 닫혀 있어야 한다.
- LSP(Liskov Substitution Principle) : 리스코프 치환 원칙
   * 상위 타입의 객체를 하위 타입의 객체로 치환해도 상위 타입을 사용하는 프로그램은 정상적으로 동작해야 한다.
- ISP(Interface Segregation Principle) : 인터페이스 분리 원칙
   * 인터페이스는 그 인터페이스를 사용하는 클라이언트를 기준으로 분리해야 한다.
- DIP(Dependency Inversion Principle) : 의존 역전 원칙
   * 고수준 모듈은 저수준 모듈의 구현에 의존해서는 안된다.
     <br><br>
     <br><br>

#### **4. Restful API 란**
<https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html>
- RESTful은 일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어이다. ‘REST API’를 제공하는 웹 서비스를 ‘RESTful’하다고 할 수 있다. RESTful은 REST를 REST답게 쓰기 위한 방법으로, REST 원리를 따르는 시스템은 RESTful이란 용어로 지칭된다.
- Restful API의 목적은 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것. RESTful한 API를 구현하는 근본적인 목적이 성능 향상에 있는 것이 아니라 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것이 주 동기이니, 성능이 중요한 상황에서는 굳이 RESTful한 API를 구현할 필요는 없다.
  <br><br>
  <br><br>

#### **5. TDD 란 무엇이며 어떠한 장점이 있는가**
<https://itsowavy.oopy.io/develop-knowledge/tdd>
- 테스트 주도 개발은 아주 짧은 개발 사이클의 반복에 의존하는 소프트웨어 개발 프로세스이며 개발자는 새로운 기능을 추가할 때 우선 해당 기능의 요구사항에 근거해 테스트 코드를 작성한다. 그리고 이 테스트를 통과하는 가장 간단한 코드를 작성하고 해당 코드를 리팩토링해 나가며 기능을 구현해 나간다.

##### 장점
- 코드 품질 향상
- 빠른 개발
- 협업에 용이

##### 단점
- 초기 투자 시간
- 테스트 코드를 작성하기 어려울 수 있음
  <br><br>
  <br><br>

#### **6. 함수형 프로그래밍**
<https://jongminfire.dev/%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%EC%9D%B4%EB%9E%80>
- 함수형 프로그래밍은 하나의 프로그래밍 패러다임으로 정의되는 일련의 코딩 접근 방식이며, 자료처리를 수학적 함수의 계산으로 취급하고 상태와 가변 데이터를 멀리하는 프로그래밍 패러다임을 의미한다. 함수형 프로그래밍은 기존 절차적 프로그래밍과 객체 지향형 프로그래밍과는 다른 새로운 방식이다.

##### 장점
- 높은 수준의 추상화를 제공한다
- 함수 단위의 코드 재사용이 수월하다
- 불변성을 지향하기 때문에 프로그램의 동작을 예측하기 쉬워진다

##### 단점
- 순수함수를 구현하기 위해서는 코드의 가독성이 좋지 않을 수 있다
- 함수형 프로그래밍에서는 반복이 for문이 아닌 재귀를 통해 이루어지는데 (deep copy), 재귀적 코드 스타일은 무한 루프에 빠질 수 있다
- 순수함수를 사용하는 것은 쉬울 수 있지만 조합하는 것은 쉽지 않다
   
##### 특징
1. 순수함수 (Pure function)
2. 비상태, 불변성 (Stateless, Immutability)
3. 선언형 함수 (Expressions)
4. 1급 객체와 고차함수 (Fist-class, Higher-order functions)
   <br><br>
   <br><br>

#### **7. MVC 패턴이란 무엇인가?**
<https://cocoon1787.tistory.com/733>
- MVC란 Model-View-Controller의 약자로 애플리케이션을 세 가지 역할로 구분한 개발 방법론. 사용자가 Controller를 조작하면 Controller는 Model을 통해 데이터를 가져오고 그 데이터를 바탕으로 View를 통해 시각적 표현을 제어하여 사용자에게 전달하게 됨
   
##### web에 적용 시
1. 사용자가 웹사이트에 접속 (Users)
2. Controller는 사용자가 요청한 웹페이지를 서비스하기 위해서 모델을 호출 (Manipulates)
3. Model은 데이터베이스나 파일과 같은 데이터 소스를 제어한 후 그 결과를 Return
4. Controller는 Model이 리턴한 결과를 View에 반영 (Updates)
5. 데이터가 반영된 View는 사용자에게 보여짐 (Sees)
   <br><br>
   <br><br>

#### **8. Git 과 GitHub 에 대해서**
<https://study-melody.tistory.com/25>

##### 깃
- 깃(Git)은 분산형 버전 관리 시스템(Version Control System)의 한 종류. 버전 관리는 파일들을 복사, 백업, 저장 등을 해서 관리하는 것을 의미함. 이러한 버전 관리는 크게 1) 클라이언트-서버 모델(CVS, SVN 등)과 2) 분산 모델, 두 가지로 나눠서 볼 수 있음. 깃(Git)은 분산 모델에 속함. 깃(Git)의 장점은 별도로 주고 받는 작업 없이 같은 파일을 여러 명이 동시에, 즉 병렬 개발이 가능. 두 번째 장점으로 작업한 파일에 대한 변경된 정보를 실시간으로 저장해 줌. 마지막 장점으로 같은 파일에 대한 각각 다른 버전을 보관할 수 있다는 점
   
##### 깃헙
- 깃허브(Github)는 Git을 지원하는 웹 호스팅 서비스 시스템(클라우드)의 한 종류라고 보면 됨. 즉 내 컴퓨터에 있는 깃의 히스토리(기록)를 가져와서 깃허브 웹사이트(클라우드)에 올릴 수 있고 변경된 히스토리를 확인할 수 있음.
- 사실 이러한 서비스를 하고 있는 것이 깃허브뿐만 있는 것이 아님. Gitlab과 bitbucket 등도 있음. 가장 유명한 것은 깃허브(Github)임.
- 깃허브(Github)의 장점으로 굉장히 많은 오픈소스들을 열람할 수 있다는 점도 있음.
