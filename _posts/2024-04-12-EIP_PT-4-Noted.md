---
title: "정보처리기사 실기 / iV - 통합 구현"
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

# C1. 연계 메커니즘 구성

<br>

## i. 연계 메커니즘 정의

### A. 연계 메커니즘의 개념
  - 응용 소프트웨어와 연계 대상 모듈 간의 데이터 연계 시 요구사항을 고려한 연계방법과 주기를 설계하기 위한 메커니즘

<br>
<br>

### B. 연계 방식

#### 1. 연계 방식의 분류
  - **직접 연계** : 연계 및 통합 구현이 단순, 용이 / 시스템 환경이 제한적
  - **간접 연계** : 상이한 네트워크, 프로토콜 연계 및 통합 가능 / 테스트 기간이 장기적

<br>

#### 2. 주요 연계 기술
  - **직접 연계**
    - **DB 링크(DB Link)** : 데이터베이스에서 제공하는 DB 링크 객체 이용
    - **DB 연결(DB Connection)** : 수신 WAS 에서 송신 DB로 연결하는 커넥션 풀 생성, 연결
    - **API/Open API** : 송신 시스템의 DB에서 데이터를 읽고 제공하는 애플리케이션 프로그래밍 인터페이스 프로그램
    - **JDBC** : 수신 시스템의 프로그램에서 JDBC 드라이버를 이용, 송신 시스템 DB와 연결
    - **하이퍼 링크** : 현재 페이지에서 다른 부분으로 가거나 전혀 다른 페이지로 이동하게 해주는 속성
  - **간접 연계**
    - **연계 솔루션(EAI)** : 기업에서 운영, 다른 플랫폼, 애플리케이션 간 정보 전달, 연계, 통합 가능하게 해주는 솔루션
    - **Web Service/ESB** : 웹 서비스가 설명된 WSDL과 SOAP 프로토콜을 이용한 시스템 간 연계
    - **소켓** : 소켓을 생성하여 포트 할당, 클라이언트의 요청 연결 후 통신

***

<br>
<br>
<br>
<br>

# C2. 내외부 연계 모듈 구현

<br>

## i. 연계 모듈 구현 환경 구성 및 개발

### A. 연계 모듈 기능 구현
  - 개발하고자 하는 응용 소프트웨어와 연계 모듈 간의 세부 설계서를 확인하여 일관되고 정형화된 연계 기능 구현 가능
  - EAI/ESB 방식과 웹 서비스 방식으로 구분

<br>
<br>

### B. EAI(Enterprise Application Integration) 방식

#### 1. EAI 개념
  - 기업에서 운영되는 서로 다른 플랫폼 및 애플리케이션 간의 정보를 전달, 연계, 통합이 가능하도록 해주는 솔루션
  - 미들웨어(Hub) 사용

<br>

#### 2. EAI 구성요소
  - **EAI 플랫폼** : 이기종 시스템 간 애플리케이션 상호 운영
  - **어댑터** : 다양한 패키지 애플리케이션 및 기업에서 자체 개발한 애플리케이션을 연결하는 EAI의 핵심 장치, 데이터 입출력 도구
  - **브로커** : 시스템 상호 간 데이터가 전송될 때, 데이터 포맷과 코드를 변환하는 솔루션
  - **메시지 큐** : 비동기 메시지를 사용하는 다른 응용 프로그램 사이에서 데이터를 송수신하는 기술
  - **비지니스 워크플로우** : 미리 정의된 기업의 비지니스 워크플로우에 따라 업무 처리하는 기능

<br>

#### 3. EAI 구축 유형
  - **포인트 투 포인트** : 가장 기초적인 애플리케이션 통합 방법, 1:1 단순 통합방법
  - **허브 앤 스포크** : 단일한 접점의 허브 시스템을 통해 데이터를 전송, 중앙 집중식
  - **메시지 버스** : 애플리케이션 사이 미들웨어(버스)를 두어 연계하는 미들웨어 통합 방식
  - **하이브리드** : 그룹 내는 허브 앤 스포크 방식 사용, 그룹 간에는 메시지 버스 방식 사용

<br>
<br>

### C. ESB(Enterprise Service Bus) 방식

#### 1. ESB 개념
  - 기업에서 운영되는 서로 다른 플랫폼 및 애플리케이션들 간을 하나의 시스템으로 관리, 웅영 가능하도록 서비스 중심 통합 지향 아키텍처
  - 느슨한 결합
  - 미들웨어(Bus) 사용

<br>

#### 2. ESB 특징
  - 서비스를 컴포넌트화된 논리적 집합으로 묶는 핵심 미들웨어, 비지니스 프로세스 환경에 맞게 설계 및 전개

<br>
<br>

### D. 웹 서비스 방식

#### 1. 웹 서비스 개념
  - 네트워크에 분산된 정보를 서비스 형태로 개방, 표준화된 방식으로 공유하는 기술
  - **HTTP** : WWW에서 HTML 문서를 송수신하기 위한 규칙 정의 프로토콜
  - **하이퍼텍스트** : 문장이나 단어 등이 링크를 통해 서로 연결된 네트워크처럼 구성된 문서
  - **HTML** : 웹 기초 구성 요소, 콘텐츠의 의미와 구조를 정의

<br>

#### 2. 웹 서비스 유형
  - **SOAP(Simple Object Access Protocol)**
    - HTTP, HTTPS, SMTP 등을 사용, XML 기반의 메시지를 네트워크 상태에서 교환하는 프로토콜
  - **WSDL(Web Service Description Language)**
    - 웹 서비스명, 제공 위치, 메시지 포맷, 프로토콜 정보 등 웹 서비스에 대한 상세 정보가 기술된 XML 형식으로 구현되어 있는 언어
  - **UDDI(Universal Description, Discovery & Integration)**
    - 웹 서비스에 대한 정보인 WSDL을 등록하고 검색하기 위한 저장소
    - 공개적으로 접근, 검색이 가능한 레지스트리이자 표준

<br>
<br>

### E. IPC 방식

#### 1. IPC(Inter-Process Communication) 개념
  - 운영체제에서 프로세스 간 서로 데이터를 주고받기 위한 통신 기술

<br>

#### 2. IPC 주요 기법
  - **메시지 큐** : 메시지 또는 패킷 단위로 동작, 프로세스 간 통신
  - **공유 메모리** : 한 프로세스의 일부분을 다른 프로세스와 공유
  - **소켓** : 클라이언트와 서버 프로세스 둘 사이에 통신을 가능하게 함
  - **세마포어** : 프로세스 사이의 동기를 맞추는 기능 제공

***

<br>
<br>
<br>

***

이번 과목은 내용이 그리 많지 않다.

그러나 기출 문제를 봤을 때 한번 씩 출제될 수 있는 과목이기도 하고, 나름 문제를 낼 만한 용어들이 있기 때문에
출제될 수 있다고 생각한다.

사실 그렇기 때문에 이번 과목의 내용은 왠만하면 다 알고 있어야 할 것 같지만 그래도 추려보자면

주요 연계 기술

EAI 개념
EAI 구성요소
EAI 구축 유형
ESB 개념
웹 서비스 개념
웹 서비스 유형
IPC 개념
IPC 기법

가 될거 같다.

그래도 단어-뜻이 딱 맞아 떨어지는게 많아서 왠만하면 다 알고 가자.