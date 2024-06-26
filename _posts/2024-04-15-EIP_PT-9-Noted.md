---
title: "정보처리기사 실기 / X - 애플리케이션 테스트 관리 ☆"
author: Jeremiah Lee
date: 2024-04-15
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

# C1. 애플리케이션 테스트 케이스 설계

<br>

## i. 애플리케이션 테스트 케이스 작성

### A. 소프트웨어 테스트의 이해
  - 개발된 응용 애플리케이션이나 시스템이 사용자가 요구하는 기능과 성능, 사용성, 안정성 등을 만족하는지 확인하고, 노출되지 않은 숨어있는 결함을 찾아내는 활동

#### 1. 소프트웨어 테스트의 기본 원칙
  - **소프트웨어 테스트 원리**
    - **결함 존재 증명** 
      - 결함이 존재함을 밝히는 활동
      - 결함이 없다는 것을 증명할 수는 없음
    - **완벽 테스팅 불가능**
      - 완벽하게 테스팅하려는 시도는 불필요한 시간과 자원 낭비
      - 무한 경로, 무한 입력값으로 인한 테스트 어려움
    - **초기 집중**
      - 조기 테스트 설계 시 테스팅 결과를 단시간에 알 수 있고, 기간 단축, 재작업을 줄여 개발 기간 단축 및 결함 예방
      - **요르돈의 법칙** : 스노우볼 이펙트, 개발 초기부터 테스트가 수행되지 못하면 후반에 영향을 미쳐 비용이 커짐
    - **결함 집중**
      - 적은 수의 모듈에서 대다수의 결함이 발견됨
      - **파레토의 법칙** : SW 테스트에서의 오류의 80%는 전체 모듈의 20% 내에서 발견
    - **살충제 패러독스** 
      - 동일한 테스트 케이스에 의한 반복적 테스트는 새로운 버그를 찾지 못함
    - **정황 의존성**
      - 소프트웨어의 성격에 맞게 테스트 실시
    - **오류-부재의 궤변**
      - 요구사항을 충족시켜주지 못한다면, 결함이 없다고 해도 품질이 높다고 볼 수 없음
  - **소프트웨어 테스트 프로세스**
    - **1. 테스트 계획**
    - **2. 테스트 분석 및 디자인**
    - **3. 테스트 케이스 및 시나리오 작성**
    - **4. 테스트 수행**
    - **5. 테스트 결과 평가 및 리포팅**
  - **소프트웨어 테스트 산출물**
    - **테스트 계획서**
      - 테스트 목적과 범위 정의, 대상 시스템 구조파악 등 테스트 수행을 계획한 문서
    - **테스트 베이시스**
      - 분석, 설계 단계의 논리적인 Case로 테스트 설계 기준이 되는 문서
    - **테스트 케이스**
      - 응용 SW가 사용자의 요구사항을 준수하는 지 확인하기 위해 설계된 입력값, 조건, 결과로 구성된 테스트 항목의 명세서
    - **테스트 슈트**
      - 테스트 케이스를 실행환경에 따라 구분해 놓은 테스트 케이스의 집합
    - **테스트 시나리오**
      - 테스트 되어야 할 기능 및 특징, 테스트가 필요한 상황을 작성한 문서
    - **테스트 스크립트**
      - 테스트 케이스의 실행 순서를 작성한 문서
    - **테스트 결과서**
      - 테스트 결과를 정리한 문서

<br>
<br>

### B. 소프트웨어 테스트 유형

#### 1. 프로그램 실행 여부에 따른 분류
  - **동적 테스트**
    - 소프트웨어를 실행하는 방식으로 테스트 수행, 결함 검출하는 테스트
    - 화이트박스 테스트, 블랙박스 테스트, 경험기반 테스트
  - **정적 테스트**
    - 테스트 대상을 실행하지 않고 구조를 분석하여 검증하는 테스트
    - 리뷰, 정적 분석

<br>

#### 2. 테스트 기법에 따른 분류 (동적)
  - **화이트박스 테스트**
    - 각 응용 프로그램의 내부 구조와 동작을 검사하는 테스트
    - 모든 문장을 한 번 이상 수행
    - **화이트박스 테스트 유형**
      - **구문 커버리지(= 문장(Statement Coverage))**
        - 프로그램 내의 모든 명령문을 적어도 한 번 수행
      - **결정 커버리지(=선택(Decision) / =분기(Branch))**
        - 전체 조건식이 적어도 한 번은 참과 거짓의 결과를 수행
      - **조건 커버리지(Condition)**
        - 개별 조건식이 적어도 한 번은 참과 거짓의 결과가 되도록 수행
      - **조건/결정 커버리지(Condition/Decision)**
        - 전체 조건식 + 개별 조건식
      - **변경 조건/결정 커버리지(Modified Condition/Decision)**
        - 개별 조건식이 다른 개별 조건식에 영향을 받지 않고 전체 조건식에 독립적으로 영향을 주도록 함으로 조건/결정 커버리지를 향상시킴
      - **다중 조건 커버리지(Multi Condition)**
        - 모든 개별 조건식의 모든 가능한 조합을 100% 보장
      - **기본 경로 커버리지(=경로(Base Path))**
        - 수행 가능한 모든 경로를 테스트
        - **맥케이브 순환복잡도** : 제어 흐름의 복잡한 정보를 정량적으로 표시하는 기법
      - **제어 흐름 테스트(Control Flow)**
        - 프로그램 제어 구조를 그래프 형태로 나타내어 내부 로직을 테스트
      - **데이터 흐름 테스트(Data Flow)**
        - 제어 흐름 그래프에 데이터 사용현황을 추가한 그래프를 통해 테스트
      - **루프 테스트(Loop)**
        - 프로그램의 반복 구조에 초점을 맞춰 실시
  - **블랙박스 테스트**
    - 외부 사용자의 요구사항 명세를 보면서 수행하는 테스트
    - **블렉박스 테스트 유형**
      - **동등분할 테스트(=동치 분할 / =균등 분할 / =동치 클래스 분해(Equivalence Partitioning))**
        - 입력 데이터의 영역을 유사한 도메인 별로 유효값/무효값을 그룹핑하고 대푯값 테스트 케이스를 도출하여 테스트
      - **경곗값 분석 테스트(=한곗값(Boundary Value Analysis))**
        - 등가 분할 후 경곗값 부분에서 오류 발생 확률이 높기 때문에 경곗값을 포함하여 테스트 케이스 설계 후 테스트
      - **결정 테이블 테스트(Decision Table)**
        - 요구사항의 논리와 발생조건을 테이블 형태로 나열, 조건과 행위를 모두 조합하여 테스트
      - **상태 전이 테스트(State Transition)**
        - 테스트 대상이나 객체의 상태를 구분, 이벤트에 의해 어느 한 상태에서 다른 상태로 전이되는 경우의 수를 수행하는 테스트
      - **유스케이스 테스트(Use Case)**
        - 시스템이 실제 사용되는 유스케이스로 모델링 되어 있을 때 프로세스 흐름을 기반으로 테스트 케이스를 명세화하여 수행하는 테스트
      - **분류 트리 테스트(Classification Tree Method)**
        - SW의 일부 또는 전체를 트리 구조로 분석 및 표현하여 테스트 케이스를 설계 후 테스트
      - **페어와이즈 테스트(Pairwise)**
        - 테스트 데이터 값들 간에 최소한 한 번씩을 조합하는 방식, 커버해야 할 기능적 범위를 모든 조합에 비해 상대적으로 적은 양의 테스트 세트를 구성하기 위한 테스트
      - **원인-결과 그래프 테스트(Cause-Effect Graph)**
        - 그래프를 활용하여 입력 데이터 간의 관계 및 출력에 미치는 영향을 분석 후 효용성이 높은 테스트 케이스를 선정하여 테스트
      - **비교 테스트(Comparison)**
        - 여러 버전의 프로그램에 같은 입력값을 넣어서 동일한 결과 데이터가 나오는지 비교해 보는 테스트
      - **오류 추정 테스트(Error Guessing)**
        - 개발자가 범할 수 있는 실수를 추정하고 이에 따른 결함이 검출되도록 테스트 케이스를 설계 후 테스트
  - **경험 기반 테스트**
    - 유사 소프트웨어나 유사 기술 평가에서 테스터의 경험을 토대로 한 테스트
    - 탐색적 테스트 / 오류 추정

<br>

#### 3. 테스트 시각에 따른 분류
  - **검증(Verification)**
    - 개발 과정 테스트
    - 올바른 제품을 생산하고 있는지 검증
  - **확인(Validation)**
    - 결과를 테스트
    - 만들어진 제품이 제대로 동작하는지 확인

<br>

#### 4. 테스트 목적에 따른 분류
  - **회복 테스트(Recovery)** : 시스템에 고의로 실패를 유도하고, 시스템의 정상 복귀 여부를 테스트
  - **안전 테스트(Security)** : 불법적인 SW가 접근하여 시스템을 파괴하지 못하도록 소스 코드 내의 보안적인 결함을 미리 점검하는 테스트
  - **성능 테스트(Performance)** : 사용자의 이벤트에 시스템이 응답하는 시간, 특정 시간 내에 처리하는 업무량, 사용자의 요구에 시스템이 반응하는 속도 등을 측정하는 테스트
    - **부하 테스트(Load)** : 시스템에 부하를 계속 증가시켜 시스템의 임계점을 찾는 테스트
    - **강도 테스트(Stress)** : 시스템 임계점 이상의 부하를 가하여 비정상적인 상황에서 시스템의 동작 상태를 확인하는 테스트
    - **스파이크 테스트(Spike)** : 짧은 시간에 사용자가 몰릴 때 시스템의 반응을 측정하는 테스트
    - **내구성 테스트(Endurance)** : 오랜 시간 동안 시스템에 높은 부하를 가하여 시스템의 반응을 테스트
  - **구조 테스트(Structure)** : 시스템의 내부 논리 경로, 소스 코드의 복잡도를 평가하는 테스트
  - **회귀 테스트(Regression)** : 오류를 제거하거나 수정한 시스템에서 오류 제거와 수정에 의해 새로이 유입된 오류가 없는지 확인하는 반복 테스트
  - **병행 테스트(Parallel)** : 변경된 시스템과 기존 시스템에 동일한 데이터를 입력 후 결과를 비교하는 테스트

<br>
<br>

### C. 정적 테스트

#### 1. 리뷰
  - 소프트웨어의 다양한 산출물에 존재하는 결함을 검출하거나 프로젝트의 진행 상황을 점검하기 위한 활동
  - **동료 검토(Peer Review)** : 2~3명이 진행하는 리뷰의 형태, 요구사항 명세서 작성자가 설명, 이해관계자가 들으며 결함 발견
  - **인스펙션(Inspection)** : 제작자 외의 다른 전문가 또는 팀이 검사하여 식별 후 검토
  - **워크 스루(Walk Through)** : 자료를 회의 전에 배포 후 가전 검토 후 짧은 시간 회의를 진행하는 형태

<br>

#### 2. 정적 분석(Static Analysis)
  - 도구의 지원을 받아 정적 테스트를 수행하는 방법
  - 자동화된 도구를 사용하여 산출물의 결함을 검출하거나 복잡도를 측정
  - 코딩 표준 부합 / 코드 복잡도 계산 / 자료 흐름 분석

<br>
<br>

### D. 테스트 케이스
  - 특정 요구사항에 준수하는 지를 확인하기 위해 개발된 입력값, 실행 조건, 예상된 결과의 집합

#### 1. 테스트 케이스 필요 항목
  - **공통 작성 항목 요소**
    - **테스트 단계명, 작성자, 승인자, 작성 일자, 문서 버전**
      - 단위/통합/시스템/인수 테스트 등의 테스트 단계와 테스트 케이스 작성자, 승인자 등을 작성
    - **대상 시스템** : 애플리케이션 개발 서버 또는 개발 시스템명 등을 작성
    - **변경 여부** : 테스트 케이스 변경 여부 및 변경 사유 등을 작성
    - **테스트 범위** : 테스트 대상 애플리케이션의 기능별 테스트 범위 및 업무별 테스트 범위 식별
    - **테스트조직** : 테스트 케이스 작성 및 테스트 수행을 담당할 조직 식별
  - **개별 테스트 케이스 항목 요소**
    - **테스트 ID** : 테스트 케이스를 고유하게 식별하기 위한 ID 작성
    - **테스트 목적** : 테스트 시 고려해야 할 중점 사항이나 테스트 케이스의 목적 작성
    - **테스트할 기능** : 애플리케이션의 테스트할 기능을 간략하게 작성
    - **테스트 데이터** : 테스트 실행 시 입력할 테이터를 작성
    - **예상 결과** : 테스트 실행 후 기대되는 결과 데이터를 작성
    - **테스트 환경** : 테스트 시 사용할 물리적, 논리적 테스트 환경, 사용할 데이터, 결과 기록 서버 등의 내용 작성
    - **테스트 조건** : 테스트 간의 종속성, 테스트 수행 전에 실행되어야 할 고려사항 등 작성
    - **성공/실패 기준** : 테스트를 거친 애플리케이션 기능의 성공과 실패를 판단하는 조건을 명확하게 작성

<br>
<br>

### E. 테스트 오라클
  - 테스트의 결과가 참인지 거짓인지를 판단하기 위해 사전에 정의된 참값을 입력하여 비교하는 기법
  - **테스트 오라클 종류**
    - **참 오라클** : 모든 입력값에 대해 기대하는 결과를 생성함으로써 발생된 오류를 모두 검출할 수 있는 오라클
    - **샘플링 오라클** : 특정한 몇 개의 입력값에 대해서만 기대하는 결과를 제공해 주는 오라클
    - **휴리스틱 오라클** : 샘플링 오라클을 개선한 오라클, 특정 입력값에 대해 올바른 결과를 제공하고, 나머지에 대해서는 휴리스틱(추정)으로 처리하는 오라클
    - **일관성 검사 오라클** : 애플리케이션 변경이 있을 때, 수행 전과 후의 결괏값이 동일한지 확인하는 오라클

<br>
<br>
<br>

## ii. 애플리케이션 테스트 시나리오 작성

### A. 테스트 레벨
  - 함께 편성되고 관리되는 테스트 활동의 그룹
  - 서로 독립적

#### 1. 테스트 레벨 종류
  - **단위 테스트** 
    - 사용자 요구사항에 대한 단위 모듀르 서브루틴 등을 테스트하는 단계
    - 모듈이나 컴포넌트에 초점을 두고 구조 기반 테스트 위주로 수행
  - **통합 테스트** 
    - 단위 테스트를 통과한 모듈 사이의 인터페이스, 통합된 컴포넌트 간의 상호 작용을 검증하는 테스트 단계
    - 소프트웨어 각 모듈 간의 인터페이스 관련 오류 및 결함 찾기
    - 설계 단계에서 제시된 것과 동일한 구조와 기능으로 구현되었는지 확인
  - **시스템 테스트** 
    - 통합된 단위 시스템의 기능이 시스템에서 정상적으로 수행되는지를 검증하는 테스트 단계
  - **인수 테스트** 
    - 계약상의 요구사항이 만족되었는지를 확인하기 위한 테스트 단계
    - **알파 테스트** : 선택된 사용자가 개발자 환경에서 통제된 상태로 개발자와 함께 수행하는 인수 테스트
    - **베타 테스트** : 실제 환경에서 일정 수의 사용자에게 대상 소프트웨어를 사용하게 하고 피드백을 받은 인수 테스트
  - **V 모델에서** 아래에서 위로 *단위 → 통합 → 시스템 → 인수* 순

<br>
<br>

### B. 테스트 시나리오
  - 여러 테스트 케이스의 집합, 동작 순서를 기술한 문서이며 절차를 명세한 문서

***

<br>
<br>
<br>
<br>

# C2. 애플리케이션 통합 테스트

<br>

## i. 애플리케이션 테스트 수행

### A. 단위 테스트
  - 개별적인 모듈 또는 컴포넌트를 테스트

#### 1. 단위 테스트 수행 도구
  - **테스트 드라이버** 
    - 모듈 테스트 수행 후의 결과를 도출하는 시험용 모듈
    - 필요 테스트를 인자를 통해 넘겨주고, 테스트 완료 후 그 결과값을 받는 역할을 하는 가상의 모듈
    - 상향식 통합 테스트
  - **테스트 스텁** 
    - 일시적으로 필요한 조건만을 가지고 임시로 제공되는 시험용 모듈
    - 상위 모듈에 의해 호출되는 하위 모듈의 역할
    - 하향식 통합 테스트

<br>
<br>

### B. 통합 테스트
  - 소프트웨어 각 모듈 간의 인터페이스 관련 오류 및 결함을 찾아내기 위한 체계적인 테스트 기법

#### 1. 하향식 통합
  - 메인으로 부터 아래 방향으로 제어 경로를 따라 이동하면서 통합 테스트
  - 하위 모듈과 최하위 모듈은 깊이-우선 또는 너비-우선 방식을 통합됨

#### 2. 상향식 통합
  - 최하위 레벨의 모듈 또는 컴포넌트로부터 위쪽 방향으로 제어 경로를 따라 이동하면서 테스트

#### 3. 샌드위치 통합
  - 상향식 통합 테스트와 하향식 통합 테스트를 결합
  - 스텁 + 드라이버 = 비용 높음

<br>
<br>

### C. 테스트 자동화 도구
  - 반복적인 테스트 작업을 스크립트 형태로 구현, 시간 단축, 비용 최소화

#### 1. 테스트 자동화 도구 유형
  - **정적 분석 도구** : 애플리케이션을 실행하지 않고 분석하는 도구
  - **테스트 실행 도구** : 작성된 스크립트를 실행하고, 각 스크립트마다 특정 데이터와 테스트 수행 방법 포함
  - **성능 테스트 도구** : 처리량, 응답 시간, 경과 시간, 자원 사용률에 대해 가상의 사용자를 생성하고 테스트
  - **테스트 통제 도구** : 테스트 계획 및 관리를 위한 테스트 관리 도구

<br>

#### 2. 테스트 하네스
  - 컴포넌트 및 모듈을 테스트하는 환경의 일부분, 테스트를 지원하기 위한 코드와 데이터를 말함
  - **테스트 하네스 구성요소**
    - **테스트 드라이버** : 테스트 대상 하위 모듈 호출, 파라미터 전달, 모듈 테스트 수행 후의 결과를 도출하는 상향식 테스트에 필요
    - **테스트 스텁** : 제어 모듈이 호출하는 타 모듈의 기능을 단순히 수행하는 도구, 하향식 테스트에 필요
    - **테스트 슈트** : 테스트 대상 컴포넌트나 모듈, 시스템에 사용되는 테스트 케이스의 집합
    - **테스트 케이스** : 입력값, 실행 조건, 기대 결과 등의 집합
    - **테스트 시나리오** : 테스트 되어야 할 기능 및 특징, 테스트가 필요한 상황을 작성한 문서
    - **테스트 스크립트** : 자동화된 테스트 실행 절차에 대한 명세
    - **목 오브젝트** : 사용자의 행위를 조건부로 사전에 입력해 두면, 그 상황에 예정된 행위를 수행하는 객체

<br>
<br>
<br>

## ii. 애플리케이션 개선 조치사항 작성

### A. 테스트 커버리지
  - 주어진 테스트 케이스에 의해 수행되는 소프트웨어의 테스트 범위를 측정하는 테스트 품질 측정 기준

<br>

### B. 테스트 커버리지 유형
  - **기능 기반 커버리지** : 테스트 대상의 전체 기능을 모수로 설정, 실제 테스트가 수행된 기능의 수를 측정
  - **라인 커버리지** : 전체 소스 코드의 라인 수를 모수로 테스트 시나리오가 수행한 소스 코드의 라인 수를 측정
  - **코드 커버리지** : 소스 코드의 구문, 조건, 결정 등의 구조 코드 자체가 얼마나 테스트 되었는지를 측정

***

<br>
<br>
<br>
<br>

# C3. 애플리케이션 성능 개선

<br>

## i. 애플리케이션 성능 분석

### A. 애플리케이션 성능 점검 개요

#### 1. 애플리케이션 성능 측정 지표
  - **처리량(Throughput)** : 주어진 시간에 처리할 수 있는 트랜잭션의 수
  - **응답 시간** : 사용자 입력이 끝난 후 애플리케이션의 응답 출력이 개시될 때까지의 시간
  - **경과 시간(Turnaround)** : 사용자가 요구를 입력한 시점부터 트랜잭션을 처리 후 결과 출력이 완료할 때까지 걸리는 시간
  - **자원 사용률** : 트랜잭션을 처리하는 동안 사용하는 CPU 사용량

<br>
<br>
<br>

## ii. 애플리케이션 성능 개선

### A. 소스 코드 최적화의 이해

#### 1. 배드 코드
  - 다른 개발자가 로직을 이해하기 어렵게 작성된 코드
  - **배드 코드 사례**
    - **Alien Code** : 아주 오래되거나 참고문서 또는 개발자가 없어 유지보수가 어려운 코드
    - **Spaghetti Code** : 소스 코드가 복잡하게 얽힌 코드
    - **알 수 없는 변수명** : 변수나 메서드에 대한 이름 정의를 알 수 없는 코드
    - **로직 중복** : 동일한 처리 로직이 중복된 코드

<br>
<br>

### B. 소스 코드 품질분석
  - **소스 코드 품질분석 도구**
    - **정적 분석 도구**
      - **pmd** : 자바 및 타 언어 소스 코드에 대한 버그, 데드코드 분석
      - **cppcheck** : C/C++ 코드에 대한 메모리 누수, 오버플로우 등 문제 분석
      - **SonarQube** : 소스 코드 품질 통합 플랫폼
      - **checkstyle** : 자바 코드에 대한 코딩 표준 검사 도구
      - **ccm** : 다양한 언어의 코드 복잡도 분석 도구
      - **cobertura** : jcoverage 기반의 테스트 커버리지 측정 도구
    - **동적 분석 도구**
      - **Avalanche** : Valgrind 프레임워크 및 STP 기반 소프트웨어 에러 및 취약점 동적 분석 도구
      - **Valgrind** : 자동화된 메모리 및 스레드 결함 발견 분석 도구

<br>
<br>

### C. 애플리케이션 성능 개선 방안
  - **소스 코드 최적화 기법 적용**
  - **아키텍처 조정을 통한 성능 개선**
  - **프로그램 호출 순서 조정 적용**
  - **소스 코드 품질분석 도구 활용**
  - **리팩토링을 통한 성능 개선**
    - 유시보수 생산성 향상을 목적으로 기능 변경 없이 복잡한 소스 코드를 수정, 보안하여 가용성 및 가독성을 높이는 기법

***

<br>
<br>
<br>

***

이번 과목 역시 타 과목에 비해 비교적 중요한 과목이다.

주로 1~2문제가 출제되는 데, 범위가 넓어 외울 게 많다.

그렇기 때문에 최대한 지엽적인 내용은 배제하였으며, 만약 나오면 틀린다는 생각으로 암기 해야할 것이다.

이전 데이터베이스 과목에서도 코멘트로 작성했지만, 이 외에도 중요한 개념과 암기해야할 내용이 산더미처럼 있기 때문에
전략적으로 다가가는 게 좋을 것 같다.

지엽적인 내용을 배제하였다는 의미는 예를 들어 후반부에 나오는 단답형 문제로 내기 애매한 부분이나,
화이트박스 테스트, 블랙박스 테스트의 세부내용이다.

정확히는 각 테스트의 동작 방식을 숙지하고 응용하여 푸는 방식의 문제를 넘겨버리겠다고 판단한 것이다.

그래도 주로 단답식으로 내기 어려운 부분을 제외했다.

일단 전체 내용이 중요하지만 그래도 굳이 거른다면

소프트웨어 테스트의 기본 원칙   
소프트웨어 테스트 산출물   
소프트웨어 테스트 유형    
테스트 기법에 따른 분류    
테스트 목적에 따른 분류    
정적테스트-리뷰    

테스트 오라클   
테스트 레벨 종류     
단위 테스트 수행 도구     
테스트 하네스 구성요소       
테스트 커버리지 유형      

정도가 될 것이다.

최대한 암기해서 챙겨갈 수 있는 것은 챙겨가되 지엽적인 개념이라 암기가 어렵다면 과감히 버리는 편이 나을 것 같다.

또한 마지막 과목인 제품 소프트웨어 패키징 파트는 지금까지 출제된 적이 없기 때문에 아예 버릴까 생각중이다.     
문제는 출제 안된 부분에서 주로 문제가 출제된다는 불안감이 있기 때문에 고민 중이긴한데,
또 과목 시점에서 보면 주로 출제되는 과목 내에서 대부분 출제되기 때문에 유기해도 될 것 같다.

일단 가장 중요한 파트인 응용 SW 기초 기술 활용 과목을 정리하면서 생각해보자.
