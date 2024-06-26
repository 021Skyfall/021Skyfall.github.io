---
title: "정보처리기사 실기 / V - 인터페이스 구현"
author: Jeremiah Lee
date: 2024-04-12
categories: [ 정보처리기사, 실기 ]
tags: [자격증]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Certificates/EIP/Certificate_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

실기 시험 준비

수제비 2023 참조

***

<br>
<br>
<br>

# C1. 인터페이스 설계 확인

<br>

## i. 외부 및 내부 모듈 연계를 위한 인터페이스 기능 식별

### A. 외부, 내부 모듈 연계 방법(EAI, ESB 연계 방법)

#### 1. EAI(Enterprise Application Integration) 방식
  - 기업에서 운영되는 서로 다른 플랫폼 및 애플리케이션 간의 정보를 전달, 연계, 통합이 가능하다록 해주는 솔루션
  - **EAI 구축 유형**
    - **포인트 투 포인트** : 가장 기초적인 애플리케이션 통합 방법, 1:1 단순 통합방법
    - **허브 앤 스포크** : 단일한 접점의 허브 시스템을 통해 데이터를 전송하는 중앙 집중형 방식
    - **메시지 버스** : 애플리케이션 사이 미들웨어(버스)를 두어 연계하는 미들웨어 통합 방식
    - **하이브리드** : 그룹 내부는 허브 앤 스포크, 그룹 간은 메시지 버스

<br>

#### 2. ESB(Enterprise Service Bus) 방식
  - 기업에서 운영되는 서로 다른 플랫폼 및 애플리케이션 간을 하나의 시스템으로 관리, 운영할 수 있도록 서비스 중심의 통합을 지향하는 아키텍처
  - 느슨한 결합

<br>
<br>
<br>

## ii. 외부 및 내부 모듈 간 인터페이스 데이터 표준 확인

### A. 인터페이스 데이터 표준 확인
  - 상호 연계하고자 하는 시스템 간 인터페이스가 되어야 할 범위의 데이터 형식과 표준을 정의하는 활동
  - 데이터 형태가 동일한 경우 그대로 전송 / 다른 경우 변환 후 전송

<br>
<br>

### B. 송수신 시스템 간 인터페이스 데이터 표준 확인 절차

#### 1. 식별된 데이터 인터페이스를 통해 인터페이스 데이터 표준 확인
  - 식별된 데이터 인터페이스의 입력값, 출력값이 의미하는 내용 파악
  - 데이터 인터페이스의 각 항목의 의미 분석 후, 데이터 표준 확인

<br>

#### 2. 인터페이스 기능을 통한 인터페이스 데이터 항목 식별
  - 식별된 인터페이스 기능을 통해 인터페이스 데이터 항목을 식별

<br>

#### 3. 데이터 표준 최종 확인
  - 식별된 인터페이스 기능 및 데이터 항목을 통해 필요한 데이터 표준 및 조정해야 할 항목 검토 및 확인, 데이터 표준 최종 확인

***

<br>
<br>
<br>
<br>

# C2. 인터페이스 기능 구현

<br>

## i. 인터페이스 기능 구현

### A. 인터페이스 기능 구현 기술

#### 1. JSON(Javascript Object Notation)
  - 속성-값 쌍 또는 키-값 쌍으로 이루어진 데이터 오브젝트를 전달하기 위해 인간이 읽을 수 있는 텍스트를 사용하는 개방형 표준 포맷
  - AJAX(Asynchronous Markup Language)에서 많이 사용
  - XML(eXtensible Markup Language)을 대체
  - **자료형 종류**
    - **숫자**
    - **문자**
    - **배열**
    - **객체**

<br>

#### 2. XML(eXtensible Markup Language)
  - HTML의 단점을 보완한 마크업 언어
  - 송수신 시스템 간 데이터 연계의 편의성을 위해 전송되는 데이터 구조를 동일한 형태로 정의

<br>

#### 3. AJAX(Asynchronous Javascript And XML)
  - 자바스크립트를 사용하여 웹 서버와 클라이언트 간 비동기적으로 XML 데이터를 교환하고 조작하기 위한 웹 기술
  - XMLHttpRequest 객체를 사용, 일부 페이지 데이터만 로드
  - **주요 기술**
    - **XMLHttpRequest** : 브라우저와 서버 간 메서드가 데이터를 전송하는 객체 폼의 API
    - **Javascript** : 객체 기반의 스크립트 프로그래밍 언어
    - **XML** : HTML 단점 보완한 SGML의 단점 개선한 특수 목적 마크업 언어
    - **DOM(Document Object Model)** : XML 문서를 트리 구조의 형태로 접근할 수 있게 해주는 API
    - **XSLT(eXtensible Stylesheet Language Transformations)** : XML 문서를 다른 XML 문서로 변환하는 데 사용하는 XML 기반 언어
    - **HTML** : 웹 문서 표현 표준 마크업 언어
    - **CSS(Cascading Style Sheets)** : 마크업 언어가 실제 표시되는 방법을 기술하는 언어

<br>

#### 4. REST(Representational State Transfer)
  - 웹과 같은 분산 하이퍼미디어 환경에서 자원의 존재/상태 정보를 표준화된 HTTP 메서드로 주고받는 웹 아키텍처
  - **리소스**, **메서드**, **메시지** 로 구성
  - **클라이언트/서버 구조** : 클라이언트와 서버는 독립적으로 구현, 서로 간 의존성 축소
  - **무 상태성** : 상태 정보를 저장하지 않고 API 서버는 요청만 단순 처리
  - **일관된 인터페이스** : HTTP 표준에 따르면 언어나 기술에 종속되지 않음
  - **캐시 처리 가능** : HTTP가 가진 캐싱 기능 적용 가능
  - **자체 표현 구조** : API 메시지 자체만 보고도 API를 이해할 수 있는 구조를 가짐

<br>
<br>
<br>

## ii. 인터페이스 보안 기능 적용

### A. 인터페이스 보안 취약점

#### 1. 데이터 통신 시 데이터 탈취 위협
  - 스니핑을 통해 감청하여 탈취하는 위협
  - **스니핑** : 공격 대상에게 직접 공격하지 않고, 데이터만 몰래 들여다 보는 수동적 공격기법

#### 2. 데이터 통신 시 데이터 위변조 위협
  - 데이터 삽입, 삭제, 변조 공격

<br>
<br>

### B. 인터페이스 보안 구현 방안

#### 1. 시큐어 코딩 가이드 적용
  - **입력 데이터 검증 및 표현** : 입력값에 대한 검증 누락 등
  - **보안 기능** : 보안 기능 부적절 구현
  - **시간 및 상태** : 병렬 시스템 등에서 부적절 관리
  - **에러 처리** : 에러 미처리, 불충분 등
  - **코드 오류** 
  - **캡슐화** : 불충분한 캡슐화
  - **API 오용** : 보안에 취약한 API 사용

<br>

#### 2. 데이터베이스 보안 적용
  - **데이터베이스 암호화 알고리즘
    - **대칭 키 암호화 알고리즘**
      - 암-복호화에 같은 암호 키 사용
      - ARIA 128/192/256, SEED
    - **비대칭 키 암호화 알고리즘**
      - 공개키는 누구나, 비밀키는 소유자만
      - RSA, ECC, ECDSA
    - **해시 암호화 알고리즘**
      - 해시값으로 원래 입력값을 알아낼 수 없는 일방향성 알고리즘
      - SHA-256/384/512, HAS-160
  - **데이터베이스 암호화 기법**
    - **API 방식** : 애플리케이션 레벨에서 암호 모듈을 적용하는 애플리케이션 수정 방식
    - **Plug-in 방식** : 암-복호화 모듈이 DB 서버에 설치된 방식
    - **TDE 방식** : DB 서버의 DBMS 커널이 자체적으로 암-복호화 기능을 수행하는 방식
    - **Hybrid 방식** : API 방식 + Plug-in 방식

<br>

#### 3. 중요 인터페이스 데이터의 암호화 전송 보안 기술
  - **IPSec** : IP 계층(3계층)에서 무결성과 인증을 보장하는 인증 헤더와 기밀성 보장하는 암호화 사용, 종단 간 보안 제공
  - **SSL/TLS** : 전송계층(4계층)과 응용계층(7계층) 사이에서 클라이언트-서버 간 암호화, 무결성 보장
  - **S-HTTP** : 웹상 네트워크 트래픽을 암호화하는 주 방법 중 하나

***

<br>
<br>
<br>
<br>

# C3. 인터페이스 구현 검증

<br>

## i. 인터페이스 구현 검증

### A. 인터페이스 구현 검증 도구의 개념
  - 인터페이스 동작 상태를 검증하고 모니터링할 수 있는 도구
  - 세부 기능 단위로 단위 테스트 / 전체 흐름으로 통합 테스트

<br>
<br>

### B. 인터페이스 구현 검증 도구의 종류
  - **xUnit** : 다양한 언어를 지원하는 단위 테스트 프레임워크
  - **STAF** : 서비스 호출, 컴포넌트 재사용 등 다양한 환경을 지원
  - **FitNesse** : 웹 기반 테스트 케이스 설계/실행/결과 확인 등을 지원
  - **NTAF** : FitNesse 협업 기능 + STAF 재사용 및 확장성
  - **Selenium** : 다양한 브라우저 지원, 개발언어 지원 웹 애플리케이션 테스트 프레임워크
  - **watir** : 루비 기반 웹 애플리케이션 테스트 프레임 워크

<br>
<br>

### C. 인터페이스 감시 도구
  - APM(Application Performance Management)
  - **스카우터** : 모니터링 및 DB Agent 지원
  - **제니퍼** : 개발부터 안정화까지 전 생애주기 모니터링

***

<br>
<br>
<br>

***

이번 과목은 앞 과목에서 상세히 나왔던 EAI, ESB가 나왔는데, 패스하려다 그냥 적었다.

이번 과목에서 제일 중요한 건 인터페이스 기능 구현 부분의 JSON, XML, AJAX 개념 부분일 것이다.

이외에 뒤의 인터페이스 보안 부분은 애초에 보안 과목이 별도로 있기 때문에 짧게 작성했고,
뒷 부분의 보고서 작성 수행, 이런 부분은 그냥 날렸다.

절대평가 시험이기 때문에 선택과 집중이 필요할 것이고, 앞으로도 한번도 나온 적 없거나
문제로 출제하기 애매한 부분 같으면 그냥 패스할 예정이다.

이번 과목에서 중요한 것은

EAI/ESB     
인터페이스 기능 구현 기술 JSON XML AJAX REST     
인터페이스 구현 검증 도구 종류     

정도가 되겠고, 보안은 뒤의 보안 과목에서 상세히 다시 보자.

그리고, 프로그래밍 언어 과목이 제일 중요하기 때문에 마지막으로 미루고
나머지 부분 작성 후에 2~3일 정도 투자해서 진행할 예정이다.

























