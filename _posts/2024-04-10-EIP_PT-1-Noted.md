---
title: "정보처리기사 실기 1 / 요구사항 확인"
author: Jeremiah Lee
date: 2024-04-10
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

# C1. 소프트웨어 개발 방법론

## i. 소프트웨어 개발 방법론

### A. 소프트웨어 생명주기 모델

#### 1. 소프트웨어 생명 주기 모델 개념
   - 개념 : 소프트웨어 생명주기는 시스템의 요구분석부터 유지보수까지 전 공정을 체계화한 절차

<br>

#### 2. 소프트웨어 생명주기 모델 프로세스
   - **요구사항 분석** : 개발할 소프트웨어의 기능과 제약 조건, 목표 등을 소프트웨어 사용자와 함께 명확히 정의하는 단계
     - 기능 요구사항 / 비기능 요구사항
   - **설계** : 시스템 명세 단계에서 정의한 기능을 실제 수행할 수 있도록 수행방법을 논리적으로 결정하는 단계
     - 시스템 구조 설계 / 프로그램 설계 / UI 설계
   - **구현** : 설계 단계에서 논리적으로 결정한 문제 해결 방법을 특정 프로그래밍 언어를 사용하여 실제 프로그램을 작성하는 단계
     - 인터페이스 개발 / 자료 구조 개발 / 오류 처리
   - **테스트** : 시스템이 정해진 요구를 만족하는지, 예상과 실제 결과가 어떤 차이를 보이는지 검사하고 평가하는 단계
     - 단위 / 통합 / 시스템 / 인수 테스트
   - **유지보수** : 시스템이 인수되고 설치된 후 일어나는 모든 행동
     - 예방 / 완전 / 교정 / 적응 유지보수

<br>

#### 3. 소프트웨어 생명주기 모델 종류
   - **폭포수 모델 (WaterFall)** : 개발 시 각 단계를 확실히 마무리 후 다음 단계로 넘어감 
     - 가장 오래됨 / 선형 / 고전적 / 명확 / 요구사항 변경 어려움
     - 절차 : 검토 → 계획 → 분석 → 설계 → 구현 → 테스트 → 유지보수
   - **프로토타이핑 모델** : 주요 기능을 프로토타입으로 구현, 피드백을 반영하여 개발
     - 프로토타입 / 분석 용이 / 검증 가능 / 프로토타입 폐기 시 비용 증가
     - 발주자, 개발자 모두에게 공동의 참조 모델을 제공 / 구현 단계의 구현 골격
   - **나선형 모델** : 시스템 개발 시 위험을 최소화하기 위해 점진적으로 개발
     - 위험분석 / 반복개발 / 위험성 감소 / 유연 대처 / 관리 어려움
     - 절차 : 계획 및 정의 → 위험 분석 → 개발 → 고객 평가
   - **반복적 모델** : 병렬적으로 개발 후 통합, 혹은 반복적으로 개발
     - 증분방식 병행 개발 / 일정 단축 / 관리 비용 증가


<br>
<br>

### B. 소프트웨어 개발 방법론

#### 1. 소프트웨어 개발 방법론 개념
  - 소프트웨어 개발 전 과정에 지속적으로 적용할 수 있는 방법, 절차, 기법
  - 소프트웨어를 하나의 생명체로 간주, 시작부터 끝까지 전 과정을 형상화한 방법론

<br>

#### 2. 소프트웨어 개발 방법론 종류
  - **구조적 방법론** : 전체 시스템을 기능에 따라 나누어 개발, 이를 통합하는 분할과 정복 접근 방식의 방법론
    - 프로세스 중심의 하향식
    - 나씨-슈나이더만 차트 사용
  - **정보공학 방법론** : 정보시스템 개발에 필요한 관리 절차와 작업 기법을 체계화한 방법론
    - 개발 주기 활용, 대형 프로젝트 수행
  - **객체 지향 방법론** : 객체 단위로 시스템을 분석 및 설계하는 방법론
    - 객체, 클래스, 메시지 사용
  - **컴포넌트 기반 방법론** : 컴포넌트를 조립해서 하나의 새로운 프로그램을 작성하는 방법론
    - 개발 기간 단축 / 기능 추가 용이 / 재사용 가능
  - **애자일 방법론** : 절차보다는 사람이 중심, 변화에 유연하고 신속하게 적응하면서 효율적으로 개발하는 방법론
    - 개발 과정의 어려움 극복
  - **제품 계열 방법론** : 특정 제품에 적용하고 싶은 공통된 기능을 정의하여 개발하는 방법론
    - 영역 공학(영역 분석, 영역 설계, 자산 구현)과 응용 공학(요구분석, 제품 설계, 제품 구현)으로 구분

<br>

#### 3. 애자일(Agile)
  - 절차보다는 사람이 중심, 변화에 유연하고 신속하게 적응하면서 효율적으로 시스템 개발
  - 짧고 신속, 폭포수 모형에 대비되는 방법론
  - 기존 개발 방법론의 한계 극복을 위해 등장
  - 유동적 / 팀 중심 / 반복 주기 / 문서보다 코드
  - 유형
    - **XP (eXtreme Programming)**
      - 의사소통 개선, 즉각 피드백
      - 1 ~ 3주 반복 개발주기
      - 가치
        - 용기, 단순성, 의사소통, 피드백, 존중
      - 기본원리
        - 페어 프로그래밍, 공동 코드 소유, 지속적인 통합, 계획 세우기, 작은 릴리즈, 메타포어(공통적인 이름 체계, 시스템 서술서를 통해 의사소통), 간단한 디자인, 테스트 기반 개발, 리팩토링, 40시간 작업, 고객 상주, 코드 표준
    - **린 (Lean)**
      - 도요타의 린 시스템 품질기법을 적용
      - 낭비 요소 제거
      - 원칙
        - 낭비제거, 품질 내재화, 지식 창출, 늦은 확정, 빠른 인도, 사람 존중, 전체 최적화
    - **스크럼**
      - 매일 정해진 시간, 장소에서 짧은 시간의 개발을 하는 팀을 위한 프로젝트 관리 중심 방법론
      - 2 ~ 4주 단위 스프린트
      - 주요 개념
        - 백로그, 스프린트, 스크럼 미팅(매일), 스크럼 마스터, 스프린트 회고, 번 다운 차트(남아있는 백로그 대비 시간을 그래픽적으로 표현한 차트 / 수직축 : 백로그, 수평축 : 시간)

<br>
<br>

### C. 객체 지향 분석 방법론

#### 1. 객체 지향 개념
  - 객체 지향은 실세계의 개체를 속성과 메서드가 결합한 형태의 객체로 표현하는 기법

<br>

#### 2. 객체 지향 구성 요소
  - **클래스** : 특정 객체 내에 있는 변수와 메서드를 정의하는 틀
  - **객체** : 물리적, 추상적으로 자신과 다른 것을 식별 가능한 대상
  - **메서드** : 클래스로부터 생성된 객체를 사용하는 방법
  - **메시지** : 객체 간 상호 작용을 하기 위한 수단
  - **인스턴스** : 객체 지향 기법에서 클래스를 통해 만든 실제의 실형 객체
  - **속성** : 한 클래스 내에 속한 객체들이 가지고 있는 데이터 값들을 단위별로 정의

<br>

#### 3. 객체 지향 기법
  - **캡슐화**
    - 서로 연관된 데이터와 함수를 묶어 외부와 경계를 만들고 필요한 인터페이스만을 밖으로 드러내는 기법
    - 결합도 낮음 / 재사용 용이
    - 인터페이스 단순화
    - 정보 은닉
  - **상속성**
    - 상위 클래스의 속성과 메서드를 하위 클래스에서 재정의 없이 물려받아 사용하는 기법
  - **다형성**
    - 하나의 메시지에 대해 각 객체가 가지고 있는 고유한 방법으로 응답할 수 있는 능력
    - 상속받은 여러 개의 하위 객체들이 다른 형태의 특성을 갖는 객체로 이용될 수 있는 성질
    - 오버로딩(같은 이름 / 다른 매개변수) / 오버라이딩(상속 관계에서 상위의 메서드를 하위에서 재정의)
  - **추상화**
    - 공통 성질을 추출하여 추상 클래스를 설정하는 기법
    - 과정 추상화 / 자료 추상화 / 제어 추상화
  - **정보 은닉**
    - 코드 내부 데이터와 메서드를 숨기고 공개 인터페이스를 통해 접근이 가능하도록 하는 코드 보안 기술
    - 필요하지 않은 정보는 접근할 수 없도록 설계
    - 요구사항 등 변화에 따른 수정 용이
  - **관계성**
    - 두 개 이상의 엔티티 형에서 데이터를 참조하는 관계를 나타내는 기법
    - 연관화
      - is-member-of 관계
    - 집단화
      - is-part-of 관계 / part-whole 관계
    - 분류화
      - is-instance-of 관계
    - 일반화
      - is-a 관계
      - 클래스들 간의 개념적인 포함 관계
    - 특수화
      - is-a 관계
      - 상위 클래스의 특성들을 상속 받으면서 하위 클래스가 수정하고 고유특성을 갖는 관계

<br>

#### 4. 객체 지향 설계 원칙 (SOLID)
  - **단일 책임 원칙 (SRP)**
    - 하나의 클래스는 하나의 책임만
  - **개방 폐쇄 원칙 (OCP)**
    - 확장에는 열려있고, 변경에는 닫혀있어야함
  - **리스코프의 치환 원칙 (LSP)**
    - (상속 관계에서) 하위 클래스는 상위 클래스로 교체할 수 있어야함
  - **인터페이스 분리 원칙 (ISP)**
    - 사용하지 않는 인터페이스는 구현하지 말아야함
  - **의존 역전 원칙 (DIP)**
    - 사용 관계 변경 없이 추상을 매개로 메시지를 주고 받음으로 관계 느슨하게

<br>

#### 5. 객체 지향 분석의 개념
  - 사용자의 요구사항을 분석하여 요구된 문제와 관련된 모든 클래스, 속성과 연산, 관계를 정의하여 모델링하는 기법

<br>

#### 6. 객체 지향 분석 방법론 종류
  - **OOSE (Object Oriented Software Engineering)**
    - 야콥슨
    - 유스케이스에 의한 접근 방법으로 유스케이스를 모든 모델의 근간으로 활용
    - 분석-설계-구현 단계
    - 기능적 요구사항 중심 시스템
  - **OMT (Object Modeling Technology)**
    - 럼바우
    - 그래픽 표기법을 이용하여 소프트웨어 구성요소를 모델링하는 방법론
    - 분석 절차 : 객체 모델링 → 동적 모델링 → 기능 모델링 순
      - 객체 모델링
        - 정보 모델링
        - 시스템에서 요구하는 객체를 찾고 객체들 간의 관계를 정의하여 ER 다이어그램 제작
        - 객체 다이어그램 활용
      - 동적 모델링
        - 시간의 흐름에 따라 객체들 사이의 제어 흐름, 동작 순서 등의 동적인 행위를 표현
        - 상태 다이어그램 활용
      - 기능 모델링
        - 프로세스들의 자료 흐름을 중심으로 처리 과정 표현
        - 자료 흐름도 (DFD) 활용
  - **OOD (Object Oriented Design)**
    - 부치
    - 설계 문서화를 강조, 다이어그램 중심으로 개발하는 방법론
    - 분석과 설계의 분리가 불가
  - **코드-요든**
    - E-R 다이어그램 사용, 객체의 행위 모델링
  - **워프-브록**
    - 분석과 설계 간 구분 없음

<br>

#### 7. 기능 모델링 주요 기법
  - **데이터 흐름도 (DFD)**
    - 자료 흐름 그래프 또는 버블 차트
    - 데이터가 각 프로세스를 따라 흐르면서 변환되는 모습을 나타낸 그림
    - 구조적 분석 기법에서 활용
    - 데이터 흐름에 중심을 둠
    - 제어 흐름은 중요치 않음
    - 시간 흐름 명확치 않음
  - **자료 사전 (DD)**
    - 자료 요소, 자료 요소들의 집합, 자료의 흐름, 자료 저장소의 의미와 관계, 값, 범위, 단위들을 구체적으로 명시하는 사전
    - 조직 속 다른 이에게 특정한 자료 용어가 무엇을 의미하는 지 알려주기 위함

<br>
<br>
<br>

## ii. 프로젝트 관리

### A. 프로젝트 관리

#### 1. 프로젝트 관리의 개념
  - 프로젝트 관리는 주어진 기간 내에 최소의 비용으로 사용자를 만족시키는 시스템을 개발하기 위한 전반적인 활동
  - 소프트웨어 생명 주기의 전과정에 걸쳐서 진행

<br>

#### 2. 프로젝트 관리 대상
  - **계획 관리** : 프로젝트 계획, 비용 산정, 일정 계획, 조직 계획에 대한 관리
  - **품질 관리** : 품질 통제 및 품질 보증
  - **범위 관리** : 이해관계자가 요청한 모든 요구사항이 프로젝트 범위에 포함되었는지 보장하고, 필요한 작업만 수행될 수 있도록 관리

<br>

#### 3. 프로젝트 관리 3대 요소
  - **사람** : 기본이 되는 인적 자원
  - **문제** : 사용자 입장에서 문제 분석 인식
  - **프로세스** : 서프트웨어 개발에 필요한 전체적인 작업 계획 및 구조

<br>
<br>

### B. 비용산정 모형

#### 1. 비용산정 모형 개념
  - 소프트웨어 규모파악을 통한 투입자원, 소요시간을 파악 후 실행 가능한 계획을 수립하기 위해 비용을 산정하는 방식

<br>

#### 2. 비용산정 모형 분류
  - **하향식 산정방법**
    - 전문가에게 의뢰 혹은 조정자를 통해 산정
    - 델파이 기법 : 전문가의 지식을 통한 문제해결 및 미래예측 산정
  - **상향식 산정방법**
    - 세부적인 요구사항과 기능에 따라 필요한 비용 계산
    - 코드 라인 수(LoC) / Man Month / COCOMO / 푸트남 / 기능점수(FP)

<br>

#### 3. 비용산정 모형 종류
  - **LoC**
    - 원시 코드 라인 수의 낙관치, 중간치, 비관치를 측정 후 예측치를 구해 비용 산정
    - (낙관치 + (4 * 중간치) + 비관치) / 6
  - **Man Month**
    - 한 사람이 1개월 동안 할 수 있는 일의 양을 기준으로 산정
    - Man Month = LoC / 프로그래머 월간 생산성
    - 프로젝트 기간 = Man Month / 프로젝트 인력
  - **COCOMO**
    - 보헴이 제안
    - 프로그램 규모에 따라 비용 산정
    - 개발비 견적에 널리 통용
    - 조직형 (Organic Mode)
      - 중/소규모 소프트웨어, 5만 라인 이하
      - 반 분리형 (Semi-Detached Mode)
      - 중규모 소프트웨어, 30만 라인 이하
    - 임베디드형 (Embedded Mode)
      - 대규모 소프트웨어, 30만 라인 이상
  - **푸트남(Putnam)**
    - 소프트웨어 개발주기의 단계별로 요구할 인력의 분포를 가정
    - 생명주기 예측 모형
  - **기능점수(Function Point)**
    - 요구 기능을 증가시키는 인자별로 가중치 부여, 요인별 가중치 합산 후 총 기능 점수 계산
    - 기능점수 = 총 기능점수 * [0.65 (0.1 * 총 영향도)]

<br>
<br>

### C. 일정관리 모델

#### 1. 일정관리 모델 개념
  - 프로젝트가 일정 기한 내에 적절하게 완료될 수 있도록 관리하는 모델

<br>

#### 2. 일정관리 모델 종류
  - **주 공정법(CPM)**
    - 여러 작업의 수행 순서가 얽혀 있는 프로젝트의 일정을 계산하는 기법
    - 프로젝트 시작에서 종료까지 가장 긴 시간이 걸리는 경로
  - **PERT**
    - 일의 순서를 계획적으로 정리하기 위한 수렴 기법
    - 비관치, 중간치, 낙관치 3점 추정방식을 통해 일정 관리
  - **중요 연쇄 프로젝트 관리(CCPM)**
    - 주 공정 연쇄법
    - 자원제약사항을 고려하여 일정 작성

<br>
<br>

### D. 위험 관리

#### 1. 위험 관리 개념
  - 내재된 위험 요소 인식, 영향 분석 후 관리하는 활동

<br>

#### 2. 위험의 종류
  - **알려진 위험** : 프로젝트 계획서, 기술적 환경, 정보 등에 의해 발견
  - **예측 가능한 위험** : 과거 경험으로부터 예측
  - **예측 불가능한 위험** : 사전 예측이 매우 어려움

<br>

#### 3. 위험 대응 전략
  - **회피** : 발생 가능성을 원천적으로 제거
  - **전가** : 위험에 대한 책임을 제3자에게 전가
  - **완화** : 위험 발생 가능성 감소, 혹은 영향력 감소
  - **수용** : 위험을 그대로 받아들임

***

<br>
<br>
<br>
<br>

# C2. 현행 시스템 분석

## i. 현행 시스템 파악

### A. 현행 시스템 파악 개념
  - 현행 시스템이 어떤 하위 시스템으로 구성되어 있고, 제공 기능 및 연계 정보는 무엇이며 어떤 기술 요소를 사용하는지를 파악하는 활동

<br>
<br>

### B. 현행 시스템 파악 절차
  - **1단계** : 구성/기능/인터페이스 파악
  - **2단계** : 아키텍처/소프트웨어 구성 파악
  - **3단계** : 하드웨어/네트워크 구성 파악

<br>
<br>

### C. 소프트웨어 아키텍처

#### 1. 소프트웨어 아키텍처 개념
  - 소프트웨어의 여러 가지 구성요소와 구성요소가 가진 특성 중에서 외부에 드러나는 특성, 그리고 구성요소 간의 관계를 표현하는 시스템 구조나 구조체

<br>

#### 2. 소프트웨어 아키텍처 프레임워크
  - 소프트웨어 집약적인 시스템에서 아키텍처가 표현해야 하는 내용 및 이들 간의 관계를 제공하는 기술 표준
  - **아키텍처 명세서**
    - 아키텍처를 기록하기 위한 산출물들
    - 이해관계자들의 시스템에 대한 관심을 관점에 맞춰 뷰로 표현
    - 개별 뷰 / 개괄 문서 / 인터페이스 명세
  - **이해관계자**
    - 시스템 개발에 관련된 모든 사람과 조직
  - **관심사**
    - 시스템에 대해 이해관계자들의 서로 다른 의견과 목표
  - **관점**
    - 개별 뷰를 개발할 때 도대가 되는 패턴이나 양식
  - **뷰**
    - 서로 관련 있는 관심사들의 집합이라는 관점에서 전체 시스템 표현
  - **근거**
    - 아키텍처 결정 근거
  - **목표**
    - 환경 안에서 한 명 이상의 이해관계자들이 의도하는 시스템의 목적, 사용, 운영 방법
  - **환경**
    - 시스템에 영향을 주는 요인으로 개발, 운영 등의 외부 요인 등으로 시스템에 영향을 주는 요인
  - **시스템**
    - 각 애플리케이션, 서브 시스템, 시스템의 집합, 제품군 등의 구현체

<br>

#### 3. 소프트웨어 아키텍처 4+1 뷰
  - 고객의 요구사항을 정리해 놓은 시나리오를 4개의 관점에서 바라보는 소프트웨어적인 접근 방법
  - 4개의 분리된 구조로 제시, 서로 충돌되지 않는지, 요구사항 충족시키는지를 증명하기 위해 유스케이스 사용
  - **유스케이스 뷰**
    - 유스케이스 또는 아키텍처를 도출하고 설계하며 다른 뷰를 검증하는 데 사용
    - 사용자, 설계자, 개발자, 테스트 관점
  - **논리 뷰**
    - 시스템의 기능적인 요구사항이 어떻게 제공되는지 설명해주는 뷰
    - 설계자, 개발자 관점
  - **프로세스 뷰**
    - 시스템의 비기능적인 속성으로서 자원의 효율적인 사용, 병행 실행, 비동기, 이벤트 처리 등을 표현한 뷰
    - 개발자, 시스템 통합자 관점
  - **구현 뷰**
    - 개발 환경 안에서 정적인 소프트웨어 모듈의 구성을 보여주는 뷰
  - **배포 뷰**
    - 컴포넌트가 물리적인 아키텍처에 어떻게 배치되는가를 매핑해서 보여주는 뷰

<br>

#### 4. 소프트웨어 아키텍처 패턴
  - 소프트웨어를 설계할 때 참조할 수 있는 전형적인 해결 방식
  - 일반적으로 발생하는 문제점들에 대한 일반화되고 재사용 가능한 솔루션
  - **계층화 패턴**
    - 시스템을 계층으로 구분하여 구성하는 패턴
    - 각 하위 모듈들은 특정한 수준의 추상화를 제공, 각 계층은 다음 상위 계층에 서비스 제공
  - **클라이언트-서버 패턴**
    - 하나의 서버와 다수의 클라이언트로 구성된 패턴
    - 사용자가 클라이언트를 통해 서버에 요청, 서버는 클라이언트에 응답
  - **파이프-필터 패턴**
    - 데이터 스트림을 생성하고 처리하는 시스템에서 사용 가능한 패턴
    - 서브 시스템이 입력 데이터를 받아 처리, 결과를 다음 서브 시스템으로 넘겨주는 과정 반복
  - **브로커 패턴**
    - 분리된 컴포넌트들로 이루어진 분산 시스템에서 사용, 컴포넌트들은 원격 서비스 실행을 통해 상호작용이 가능
    - 브로커 컴포넌트는 컴포넌트 간의 통신을 조정하는 역할 수행
  - **MVC 패턴**
    - 대화형 애플리케이션을 모델, 뷰, 컨트롤러 3개의 서브 시스템으로 구조화하는 패턴
    - 각 부분이 별도의 컴포넌트로 분리되어 있어서 서로 영향을 받지 않고 개발 가능
    - **모델** : 핵심 기능과 데이터 보관
    - **뷰** : 사용자에게 정보 표시
    - **컨트롤러** : 사용자로부터 요청을 입력받아 처리

<br>

#### 5. 소프트웨어 아키텍처 비용 평가 모델
  - 아키텍처 접근법이 품질 속성에 미치는 영향을 판단 후, 적합성 평가
  - **SAAM** : 변경 용이성과 기능성에 집중, 평가 용이
  - **ATAM** : 아키텍처 품질 속성을 만족시키는지 판단 및 품질 속성들의 이해 상충관계까지 평가
  - **CBAM** : ATAM 바탕의 시스템 아키텍처 분석 중심으로 경제적 의사결정에 대한 요구를 충족하는 비용 평가 모델
  - **ADR** : 소프트웨어 아키텍처 구성요소 간 응집도를 평가
  - **ARID** : 전체 아키텍처가 아닌 특정 부분에 대한 품질요소에 집중하는 비용 평가 모델

<br>
<br>

### D. 디자인 패턴

#### 1. 디자인 패턴 개념
  - 소프트웨어 설계에서 공통으로 발생하는 문제에 대해 자주 쓰이는 설계 방법을 정리한 패턴

<br>

#### 2. 디자인 패턴 구성요소
  - **패턴 이름** : 부를 때 사용하는 이름과 디자인 패턴의 유형
  - **문제 및 배경** : 사용되는 분야 또는 배경, 해결하는 문제를 의미
  - **솔루션** : 요소들, 관계, 협동 과정
  - **사례** : 간단한 적용 사례
  - **결과** : 사용하면 얻게 되는 이점이나 영향
  - **샘플 코드** : 적용된 원시 코드

<br>

#### 3. 디자인 패턴 유형
  - **목적**
    - **생성** : 객체 인스턴스 생성에 관여, 클래스 정의와 객체 생성 방식을 구조화, 캡슐화를 수행하는 패턴
    - **구조** : 더 큰 구조 형성을 목적으로 클래스나 객체의 조합을 다루는 패턴
    - **행위** : 클래스나 객체들이 상호 작용하는 방법과 역할 분담을 다루는 패턴
  - **범위**
    - **클래스** : 클래스 간 관련성, 상속 관계를 다루는 패턴 / 컴파일 타임
    - **객체** : 객체 간 관련성을 다루는 패턴 / 런타임

<br>

#### 4. 디자인 패턴 종류
  - **생성 패턴**
    - **Builder**
      - 복잡한 인스턴스를 조립하여 만드는 구조
      - 복합 객체를 생성할 때 객체를 생성하는 방법과 구현하는 방법을 분리
      - 동일한 생성 절차에서 서로 다른 표현 결과 도출
    - **Prototype**
      - 일반적인 원형 제작 후 그것을 복사하여 필요한 부분만 수정해서 사용
      - 생성할 객체의 원형을 제공하는 인스턴스에서 생성할 객체들의 타입이 결정되도록 설정
      - 객체를 생성할 때 갖추어야 할 기본 형태가 있을 때 사용
    - **Factory Method**
      - 상위 클래스에서 객체를 생성하는 인터페이스를 정의 후 하위 클래스에서 인스턴스를 생성하도록 하는 방식
      - 상위 클래스에서는 인스턴스를 만드는 방법만 결정, 하위 클래스에서 데이터 생성을 책임지고 조작하는 함수들을 오버라이딩
    - **Abstract Factory**
      - 구체적인 클래스에 의존하지 않고 서로 연관되거나 의존적인 객체들의 조합을 만드는 인터페이스를 제공
      - 이 패턴을 통해 생성된 클래스에서는 사용자에게 인터페이스를 제공, 구체적인 구현은 Concrete Product 클래스에서 이루어짐
    - **Singleton**
      - 전역 변수를 사용하지 않고 객체를 하나만 생성, 생성된 객체를 어디서든지 참조할 수 있도록 하는 패턴
      - 한 클래스에 한 객체만
  - **구조 패턴**
    - **Bridge**
      - 기능의 클래스 계층과 구현의 클래스 계층을 연결
      - 구현부에서 추상 계층을 분리하여 각 부분을 독립적으로 확장할 수 있는 패턴
    - **Decorator**
      - 기존에 구현되어 있는 클래스에 필요한 기능을 추가해 나가는 설계 패턴
      - 기능 확장이 필요할 때 객체 간의 결합을 통해 기능을 동적으로 유현하게 확장 가능
      - 상속의 대안
    - **Facade**
      - 복잡한 시스템에 대해 단순한 인터페이스를 제공함으로 사용자와 시스템 간 또는 여타 시스템 간의 결합도를 낮춤
      - 시스템 구조에 대해 파악을 쉽게 함
    - **Flyweight**
      - 다수의 객체로 생성될 경우 모두가 갖는 본질적인 요소를 클래스 화하여 공유
      - 메모리 절약, 클래스의 경략화
    - **Proxy**
      - 실제 객체에 대한 대리 객체
      - 특정 객체로의 접근 제어하기 위한 용도
    - **Composite**
      - 객체들의 관계를 트리 구조로 구성
      - 복합 객체와 단일 객체를 동일하게 취급
    - **Adapter**
      - 기존에 생성된 클래스를 재사용할 수 있도록 중간에서 맞춰주는 역할을 하는 인터페이스를 생성
  - **행위 패턴**
    - **Mediator**
      - 객체 수가 많을 때 중간에서 통제하고 지시할 수 있는 역할을 하는 중재자를 두고 통신의 빈도수를 줄이는 패턴
      - 상호 작용의 유연한 변경을 지원
    - **Interpreter**
      - 언어의 다양한 해석, 여러 형태의 언어 구문을 해석할 수 있게 만드는 패턴
      - 문법 자체를 캡슐화
    - **Iterator**
      - 컬렉션 구현 방법을 노출 시키지 않으면서 컬렉션 안의 모든 항목에 반복자를 사용하여 접근할 수 있는 패턴
      - 내부 구조 노출 없이 복잡 객체의 원소에 순차적으로 접근 가능
    - **Template Method**
      - 어떤 작업을 처리하는 일부분을 서브 클래스로 캡슐화해 전체 일을 수행하는 구조는 바꾸지 않으면서 특정 단계에서 수행하는 내역을 바꾸는 패턴
      - 상위 작업의 구조를 바꾸지 않으면서 서브 클래스로 작업의 일부분을 수행
    - **Observer**
      - 한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들에 연락이 가고, 자동으로 내용이 갱신되는 방법으로 상호작용하는 객체 사이를 느슨하게 결합하는 패턴
      - 객체의 상태 변화에 따라 다른 객체의 상태도 연동
    - **State**
      - 객체 상태를 캡슐화하여 클래스화함, 그것을 참조하게 하는 방식으로 상태에 따라 행위 내용을 변경
    - **visitor**
      - 각 클래스 데이터 구조로부터 처리 기능을 분리하여 별도의 클래스를 만들고, 해당 클래스의 메서드가 각 클래스를 돌아다니며 특정 작업을 수행
      - 특정 구조를 이루는 복합 객체의 원소 특성에 따라 동작을 수행할 수 있도록 지원
    - **Command**
      - 실행될 기능을 캡슐화함으로 주어진 여러 기능을 시행할 수 있는 재사용성이 높은 클래스를 설계
      - 요구사항을 객체로 캡슐화
    - **Strategy**
      - 알고리즘 군을 정의하고 같은 알고리즘을 각각 하나의 클래스로 캡슐화, 필요할 때 서로 교환 사용
      - 행위 객체를 클래스로 캡슐화해 동적으로 행위 변환
    - **Memento**
      - 클래스 설계 관점에서 객체의 정보를 저장할 필요가 있을 때 적용
      - 객체를 이전 상태로 복구시켜야하는 경우 Undo 요청 가능
    - **Chain of Responsibility**
      - 정적으로 어떤 기능에 대한 처리의 연결이 하드 코딩 되어 있을 경우, 연결 변경 불가능한데, 이를 동적으로 연결 되어 있는 경우에 따라 다르게 처리
      - 한 요청을 2개 이상의 객체에서 처리

<br>
<br>
<br>

## ii. 개발 기술 환경 정의

### A. 개발 기술 환경 현행 시스템 분석

#### 1. 운영체제 현행 시스템 분석
  - 운영체제는 컴퓨터 시스템이 제공하는 모든 하드웨어, 소프트웨어를 사용할 수 있도록 해주고 컴퓨터 사용자와 컴퓨터 하드웨어 간의 인터페이스를 담당하는 프로그램
  - 운영체제 현행 시스템 분석
    - **품질 측면**
      - **신뢰도** : 장기간 시스템 웅영 시 운영체제의 장애 발생 가능성
      - **성능** : 대규모 및 대량 파일 작업(배치 작업) 처리
    - **지원 측면**
      - **기술 지원** : 공급사들의 안정적인 기술 지원
      - **주변 기기** : 설치 가능한 하드웨어
      - **구축 비용** : 지원 가능한 하드웨어 비용
  - 운영체제 종류 및 특징
    - **PC**
      - **Windows** : Microsoft : 중/소규모 서버, 일반 PC 등 유지, 관리 비용 장점
      - **UNIX** : IBM, HP, SUN : 대용량 처리, 안정성 높은 엔터프라이즈급 서버
      - **Linux** : Linus Torvalds : 중/대규모 서버 대상, 높은 보안성 제공
    - **모바일**
      - **안드로이드** : Google : 리눅스 운영체제 위에서 구동, 휴대용 장치를 위한 운영체제와 미들웨어, 표준 응용 프로그램 등을 포함
      - **IOS** : Apple : 스마트폰, 태블릿 PC의 높은 보안성과 고성능 제공

<br>

#### 2. 네트워크 현행 시스템 분석
  - 네트워크는 컴퓨터 장치들의 노드 간 연결을 사용하여 서로 데이터를 교환할 수 있도록 하는 기술
  - OSI 7계층

| 계층                   |                설명                 | 프로토콜        | 전송단위    |
|----------------------|:---------------------------------:|-------------|---------|
| 응용 계층(Application)   |   사용자와 네트워크 간 응용서비스 연결, 데이터 생성    | HTTP / FTP  | DATA    |
| 표현 계층(Presentation)  |      데이터 형식 설정과 부호교환, 암/복호화       | JPEG / MPEG | DATA    |
| 세션 계층(Session)       |           연결 접속 및 동기제어            | SSH / TLS   | DATA    |
| 전송 계층(Transport)     |           신뢰성 있는 통신 보장            | TCP / UDP   | Segment |
| 네트워크 계층(Network)     |    단말기 간 데이터 전송을 위한 최적화된 경로 제공    | IP / ICMP   | Packet  |
| 데이터 링크 계층(Data Link) |     인접 시스템 간 데이터 전송, 전송 오류 제어     | 이더넷         | Frame   |
| 물리 계층(Physical)      | 0과 1의 비트 정보를 회선에 보내기 위한 전기적 신호 변환 | RS-232C     | Bit     |

<br>

#### 3. DBMS 현행 시스템 분석
  - DBMS는 데이터베이스를 만들고, 저장 및 관리할 수 있는 기능을 제공하는 응용 프로그램
  - DBMS 기능
    - **중복 제어** : 동일한 데이터가 여러 위치에 중복으로 저장되는 현상 방지
    - **접근 통제** : 권한에 따라 데이터에 대한 접근 제어
    - **인터페이스 제공** : 사용자에게 SQL 및 CLI, GUI 등 다양한 인터페이스 제공
    - **관계표현** : 서로 다른 데이터 간의 다양한 관계를 표현할 수 있는 기능 제공
    - **샤딩/파티셔닝** : 구조 최적화를 위해 작은 단위로 나누는 기능 제공
    - **무결성 제약 조건** : 무결성에 관한 제약 조건을 정의/검사하는 기능 제공
    - **백업 및 회복** : 데이터베이스 장애 발생 시 데이터의 보존 기능 제공
  - DBMS 현행 시스템 분석 시 고려 사항
    - **성능 측면**
      - **가용성** : 장기간 시스템을 운영할 때 장애 발생 가능성 / 백업 및 복구 편의
      - **성능** : 대규모 데이터 처리 성능 / 다양한 튜닝 옵션
      - **상호 호환성** : 설치 가능한 운영체제 종류
    - **지원 측면**
      - **기술 지원** : 공급 업체들의 안정적인 기술 지원 여부
      - **구축 비용** : 라이선스 정책 및 비용

<br>

#### 4. 미들웨어의 현행 시스템 분석
  - 분산 컴퓨팅 환경에서 응용 프로그램과 프로그램이 운영되는 환경 간에 원만한 통신이 이루어질 수 있도록 제어해주는 소프트웨어
  - 대표 미들웨어 WAS
  - 웹 애플리케이션 서버(WAS)는 서버계층에서 애플리케이션이 동작할 수 있는 환경을 제공, 안정적인 처리 관리, 타기종 시스템과 연동 지원
  - 미들웨어 현행 시스템 분석 시 고려 사항
    - **성능 측면**
      - **가용성** : 장기간 시스템을 운영할 때 장애 발생 가능성 / WAS 버그 등 개선 패치 설치 위한 재기동 기능 지원
      - **성능** : 대규모 데이터 처리 성능 / 가비지 컬렉션의 다양한 옵션
    - **지원 측면**
      - **기술 지원** : 안정적인 기술 지원 여부 / 오픈 소스 여부
      - **구축 비용** : 라이선스 정책 및 비용 / 총 소유 비용

<br>

#### 5. 오픈 소스 사용 시 고려 사항
  - 라이선스의 종류, 사용자 수, 기술의 지속 가능성
  - 자유 배포, 소스 코드 공개, 파생작업 허용, 소스 코드 일관성 확보, 차별금지, 라이선스 배포, 포괄적 허용

***

<br>
<br>
<br>
<br>

# C3. 요구사항 확인

## i. 요구사항

### A. 요구사항 개념

#### 1. 요구공학의 개념
  - 사용자의 요구가 반영된 시스템을 개발하기 위해 사용자 요구사항에 대한 도출, 분석, 명세, 확인 및 검증하는 구조화된 활동

<br>

#### 2. 요구공학의 목적
  - 이해관계자 사이에 효과적인 의사소통 수단을 제공, 시스템 개발의 요구사항에 대한 공통된 이해 설정
  - 요구사항 누락 방지 및 이해 오류로 인한 불필요한 비용을 절감, 요구사항 변경 추척
  - 초기 요구사항 관리로 개발 비용과 시간을 절약, 효과적인 의사소통 수단 제공

<br>

#### 3. 요구사항의 분류
| 구분 | 기능적 요구사항                              | 비기능적 요구사항                                       |
|----|---------------------------------------|-------------------------------------------------|
| 개념 | 시스템이 제공하는 기능, 서비스에 대한 요구사항            | 시스템이 수행하는 기능 이외의 사항, 시스템 구축에 대한 제약사항에 관한 요구 사항  |
| 도출 | 방법 특정 입력에 대해 시스템이 어떻게 반응해야 하는지에 대한 기술 | 품질 속성에 관련하여 시스템이 갖춰야할 사항에 관한 기술                 |
| 특징 | 기능성, 완전성, 일관성                         | 신뢰성, 사용성, 효율성, 유지보수성, 이식성, 보안성, 품질관련 요구사항, 제약사항 |

<br>
<br>

### B. 요구공학 프로세스

#### 1. 요구사항 개발 단계 구성
  - **요구사항 도출**
    - 소프트웨어가 해결해야 할 문제를 이해, 고객으로부터 제시되는 추상적 요구에 대해 정보 식별, 수집, 구체적으로 표현
  - **요구사항 분석**
    - 도출된 요구사항에 대해 충돌, 중복, 누락 등의 분석을 통해 완전성과 일관성을 확보
  - **요구사항 명세**
    - 체계적으로 검토, 평가, 승인될 수 있는 문서를 작성하는 단계
  - **요구사항 확인 및 검증**
    - 요구사항을 이해했는지 확인하고, 요구사항 문서가 적합고 이해 가능하며, 일관성 있고, 완전한지 검증하는 단계

<br>

#### 2. 요구사항 개발 단계 상세
  - 요구사항 도출 단계 주요 기법
    - **인터뷰** : 이해관계자와 직접 대화
    - **브레인스토밍** : 회의 참석자들의 아이디어를 비판 없이 수용
    - **델파이 기법** : 전문가의 경험적 지식을 통해 문제 해결 및 미래예측
    - **롤 플레잉** : 현실 장면 설정, 여러 사람이 각자 연기 후 분석 수집
    - **워크숍** : 단기간의 집중적인 노력을 통해 다양한 전문 정보를 획득, 공유
    - **설문 조사**
  - 요구사항 분석 단계 절차
    - **요구사항 분류** : 기능인지 비기능인지 확인
    - **개념 모델링 생성 및 분석** : 요구사항을 더 쉽게 이해할 수 있도록 단순화, 개념적으로 표현
    - **요구사항 할당** : 요구사항을 만족시키기 위한 아키텍처 구성요소 식별
    - **요구사항 협상** : 두 명의 이해관계자가 서로 상충되는 내용 요구 시 적절한 지점에서 합의
    - **정형 분석** : 형식적으로 정의된 의미를 지닌 언어로 요구사항 표현
  - 요구사항 분석 단계 기법
    - **자료 흐름 지향 분석** : 데이터 흐름도(DFD) 및 자료 사전(DD)으로 부터 소프트웨어 구조를 유도
    - **객체 지향 분석** : 시스템의 기능과 데이터를 함꼐 분석, UML로 표준화
  - 요구사항 분석 기술
    - **청취 기술** : 이해관계자로부터 의견을 드는 기술
    - **인터뷰와 질문 기술** : 이해관계자를 만나 정보를 수집
    - **분석 기술** : 추출된 요구사항에 대해 충돌, 중복, 누락 등의 분석을 통해 완전성과 일관성 확보
    - **중재 기술** : 이해관계자들의 상반된 요구에 대한 중재기술
    - **관찰 기술** : 사용자가 작업하는 것을 관찰하면서 사용자가 언급하지 않은 의미를 탐지
    - **작성 기술** : 문서 작성 기술
    - **조직 기술** : 수집된 방대한 정보를 일관성 있는 정보로 구조화
    - **모델 작성 기술** : 수집한 자료를 바탕으로 모델로 작성하는 기술
  - 요구사항 명세 단계 주요 기법
    - **비정형 명세 기법** : 사용자의 요구를 표현할 때 자연어를 기반으로 서술
    - **정형 명세 기법** : 사용자의 요구를 표현할 때 수학적인 원리와 표기법으로 서술
  - 요구사학 명세 원리 및 검증 항목
    - **명확성** : 각각의 요구사항 명세 내용은 하나의 의미만 부여해야 함
    - **완전성** : 기능, 성능, 속성, 인터페이스, 설계 제약 등에 대한 모든 시스템 요구사항 포함
    - **검증 가능성** : 요구사항의 충족 여부와 달성 정도에 대한 확인
    - **일관성** : 요구사항의 내용 간 상호 모순 없어야 함
    - **수정 용이성** : 요구사항 변경 시 쉽게 수정 가능해야함
    - **추적 가능성** : 각 요구사항 근거에 대한 추적과 상호참조가 가능해야 함
    - **개발 후 이용성** : 개발 후 운영 및 유지보수에 효과적인 이용이 가능해야 함
  - 요구사항 확인 및 검증 절차
    - **요구사항 목록 확인** : 요구사항 목록에서 업무 기능에 대한 요구사항 반영 여부 확인
    - **요구사항 정의서 작성 여부 확인** : 요구사항 목록 중 수용인 경우, 요구사항 정의서가 작성되었는지 확인
    - **비기능적 요구사항의 확인** : 시스템 특성, 품질, 제약사항 등 비기능적 요구사항이 명확하게 도출되었는지 검토
    - **타 시스템 연계 및 인터페이스 요구사항 확인** : 타 시스템 또는 하위 시스템 등과의 모든 인터페이스 요구사항이 정의되어 있는지 확인
  - 요구사항 확인 및 검증 단계의 주요 기법
    - **요구사항 검토** : 여러 검토자들이 에러, 불명확성, 차이 등 검토
    - **정형 기술 검토 활용**
      - **동료 검토** : 2~3명이 진행하는 리뷰의 형태
      - **워크 스루** : 오류를 조기에 검출하는 데 목적이 있는 검토 방법, 사전검토 후 짧은 시간 회의
      - **인스펙션** : 소프트웨어 요구, 설계, 원시 코드 등의 저작자 외의 다른 전문가 또는 팀이 검사
    - **프로토타이핑 활용** : 개발할 시스템에 대한 주요 기능이나 일부분을 개발 후 사용자나 고객이 경험할 수 있게하여 요구사항 확인
    - **모델 검증** : 분석단계에서 개발된 모델의 품질 검증 필요
    - **테스트 케이스 및 테스트를 통한 확인** : 중요 속성은 최종 제품이 요구사항을 만족시키는지 확인 가능해야 함
    - **CASE 도구 활용 검증** : 구조화된 요구사항 명세서에 대해서는 자동화된 일관성 분석을 제공하는 CASE 도구 활용 가능
    - **베이스라인을 통한 검증** : 요구사항 변경을 체계적으로 추적하고 통제하는 시점인 베이스라인을 통한 지속적 검증
    - **요구사항 추적표** : 요구사항 정의서를 기준으로 개발 단계별 최종 산출물 반영, 변경 확인 가능 문서
  - 상세 정형 기술 검토 기법
    - **관리 리뷰** : 프로젝트 진행 상황에 대한 전반적인 검토를 바탕으로 통제 및 의사결정 지원
    - **기술 리뷰** : 정의된 계획 및 명세를 준수하고 있는지에 대한 검토
    - **인스펙션** : 동료 검토
    - **워크 스루** : 짧은 회의 진행
    - **감사** : 소프트웨어 제품의 제공자, 소비자, 제3기관 수행

<br>

### 3. 요구사항 관리 단계
  - 요구사항 관리 단계 절차
    - **요구사항 협상** : 가용 자원과 수용 가능한 위험 수준에서 구현 가능한 기능을 협상
    - **요구사항 기준선 설정** : 공식적으로 검토되고 합의된 요구사항 명세서를 통해 기준선을 설정
    - **요구사항 변경관리** : 요구사항 기준선을 기반으로 모든 변경을 공식 통제
    - **요구사항 확인 및 검증** : 구축된 시스템이 이해관계자가 기대한 요구사항에 부합하는지 확인

<br>
<br>
<br>

## ii. 요구사항의 시스템화 타당성 분석

### A. 요구사항의 기술적 타당성 검토
  - **성능 및 용량 산정의 적정성** : 목표 시스템의 용량이 산정되면, 적정성 여부 판단
  - **시스템 간 상호 운용성** : 요구사항 중 타 시스템과의 연동을 요구하는 경우 가능한지 판단
  - **IT 시장 성숙도 및 트렌드 부합성** : 향후 사용되지 않을 가능성이 높은 시스템들은 향후 유지보수가 어려울 가능성
  - **기술적 위험 분석** : 요구사항을 만족시키기 위해 적용한 기술의 복잡성 등에 대하여 영향도 파악

<br>
<br>

### B. 요구사항의 기술적 타당성 분석 프로세스
  - **타당성 분석 결과 기록** : 요구사항 목록에 타당성 분석을 위한 속성을 추가하고 타당성 분석 결과 기록
  - **타당성 분석 결과의 이해관계자 검증** : 요구사항의 시스템화 타당성 분석 결과를 요구사항 관련 이해관계자에게 배포하여 사전 검토 요청
  - **타당성 분석 결과 확인 및 배포/공유** : 확정된 타당성 분석 결과를 이해관계자에게 배포하여 공유

***

<br>
<br>
<br>

***

이제 서적의 1과목인데 내용이 너무 많다.

단어-의미로 최대한 정리, 줄여서 작성하려 했는데도 이 정도다.

일단 이번 포스트의 목적 자체는 "정독"을 하는 느낌으로 외우다시피 하기 위해 타이핑을 한 것이라 "요약본"과는 확연히 다른데,
추후 12과목을 모두 정독, 타이핑하고 나면 정말 중요한 부분만 짚어서 요약할 예정이다.

손꾸락과 손목이 부러질 것 같지만 정처기 실기는 이렇게까지 해야하나? 싶을 정도로 해야한다고들 하니 최대한 타이핑해보자














