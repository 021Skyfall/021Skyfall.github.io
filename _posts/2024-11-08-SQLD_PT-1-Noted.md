---
title: "SQLD / 1 과목 - 데이터 모델링의 이해"
author: Jeremiah Lee
date: 2024-11-08
categories: [ SQLD ]
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

[대발 허브](https://daebal.tistory.com/18) 참조

학습 목적으로 직접 타이핑해서 옮김.     
문제 시 삭제.

<br>
<br>
<br>

# 1과목

## 데이터 모델링의 이해

### 데이터 모델링 이란?
  - 데이터 모델링은 '현실 세계'를 단순화하여 표현하는 기법

<br>
<br>

### 데이터 모델링 특징 및 목적

#### 특징
  - **추상화** : 현실 세계, 개념을 일정한 형식으로 '간략하게' 표현
  - **단순화** : 현실 세계를 '정해진 표기법'으로 단순하고 쉽게 표현, 핵심에 집중하고 불필요한 요소 제거
  - **명확화** : 불분명함을 제거하고, 정확하게 현상을 기술

#### 목적
  - 단순히 DB, 시스템 만을 구축하기 위한 것이 아닌 업무 설명, 분석, 형상화 목적도 있음
  - 분석된 모델을 실제 DB로 생성하여 개발 및 데이터 관리에도 사용

<br>
<br>

### 데이터 모델링 유의점 및 3가지 관점과 중요 3요소

#### 유의점
  - **중복(Duplication)** : 같은 데이터가 엔티티에 중복 저장되면 안됨
  - **비유연성(Inflexibility)** : 애플리케이션의 '사소한 변경'에도 데이터 모델이 수시로 변경되면 안됨 / 데이터 모델과 프로세스 분리해서 유연성 향상
  - **비일관성(Inconsistency)** : 중복이 없는 경우에도 비일관성 발생 가능성 있음 / 데이터 간 연관 관계 명확하게 정의

#### 관점
  - **데이터 관점(What, Data)** : 어떤 데이터들이 업무와 얽혀있는지
  - **프로세스 관점(How, Process)** : 업무가 실제로 처리하고 있는 일이 무엇인지
  - **데이터와 프로세스의 상관 관점(Data vs Process, Interaction)** : 프로세스 흐름에 따라 데이터가 어떤 영향을 받는지

#### 중요 요소
  - **Things** : 대상 (Entity)
  - **Attribute** : 속성
  - **Relationships** : 관계

<br>
<br>

### 모델링의 3가지 단계

#### 1. **개념적** 데이터 모델링
  - **'전사적'으로 수행**, 업무 중심적이고 포괄적인 수준의 모델링 (**추상화 레벨 가장 높음**)

#### 2. **논리적** 데이터 모델링
  - Key, 속성, 관계 들을 표현하는 단계 -> 정규화 활동이 이루어지는 단계
  - 논리 데이터 모델을 대상으로 정규화 하는 것

#### 3. **물리적** 데이터 모델링
  - 실제 DB를 구현할 수 있도록 **성능, 가용성 등 물리적 요소를 고려**하는 단계

<br>
<br>

### 데이터 스키마 단계에 따른 독립성

#### 스키마란?
  - 테이블이 어떤 구성으로 되어있는 지, 어떤 정보를 가지고 있는 지에 대한 기본적인 테이블의 구조를 정의한 것

#### 데이터 스키마의 구조
  - USER
    - **외부** 스키마 : **각 사용자**가 보는 스키마 정의 및 표현
    - **개념** 스키마 : **모든 사용자**가 보는 데이터 정의 및 표현 & 관계를 정의하는 단계
    - **내부** 스키마 : 물리적인 저장 구조를 나타내는 단계 -> **저장 구조, 칼럼, 인덱스 정의**
  - DB
    - **논리적 독립성** : 개념 스키마가 변경 되어도 외부 스키마는 영향 없음 / 외부 --- 개념
    - **물리적 독립성** : 내부 스키마가 변경되어도 개념/외부 스키마는 영향 없음 / 외부, 개념 --- 내부

<br>
<br>

### ERD 작성 순서
1. 엔티티 도출
2. 엔티티 배치
3. 엔티티 관계 설정
4. 관계명 기입
5. 관계 참여도 기입
6. 관계 필수/선택 여부 기입

#### IE/Crow's Foot (까마귀발) 표기법

![](/assets/img/Certificates/SQLD/ie_crowfoot.png)
출처 : https://ppomelo.tistory.com/51

![](/assets/img/Certificates/SQLD/crowfoot_ex.png)
출처 : https://ppomelo.tistory.com/51

- Entity는 네부분의 모서리가 둥근 형태인 Soft-Box로 표현
- Entity 이름은 Box 내부 상단에 표시
- Attribute 중 필수로 값을 입력하며 식별자인 속성은 # (Mandatory)를 표시
- Attribute 중 필수로 값을 입력해야 하는 속성은 * (Mandatory)를 표시
- Attribute 중 선택적인 입력을 해야 하는 속성은 o (Optional)를 표시

<br>
<br>

### 엔티티란? 엔티티의 특징
  - 업무에서 쓰이는 데이터들을 용도 별로 분류한 데이터의 그룹 = 엔티티

#### 엔티티의 특징
  - **업무에서 쓰이는 정보**여야 함
  - **식별자**가 있어야 함
  - **2개 이상의 인스턴스**를 가져야 함
  - **반드시 속성**을 가져야 함 -> 이 때 **하나의 인스턴스는 2개 이상의 속성**을 가짐
    - 즉, **하나의 엔티티는 2개 이상의 속성**을 가짐
  - 다른 **엔티티와 1개 이상의 관계**를 가짐

<br>
<br>

### 엔티티 분류 방법과 그에 따른 종류

#### **유형, 무형에 따른 분류** (개사유)
  - **개념** 엔티티 : 모델링 대상이 **형태 없음** / ex) 부서, 학과
  - **사건** 엔티티 : 모델링 대상이 **행위로 인해 발생**하는 것 / ex) 주문
  - **유형** 엔티티 : 모델링 대상이 **물리적인 형태가 존재** / ex) 상품, 회원

#### **발생 시점에 따른 분류** (기행중)
  - **기본** 엔티티 : 모델링 대상이 업무에 대해 **원래 존재하는 요소** -> 독립적, 자식 엔티티를 가실 수 있음 / ex) 상품, 회원, 부서
  - **행위** 엔티티 : **2개 이상의 엔티티**로 부터 파생 / ex) 주문 내역
  - **중심** 엔티티 : 모델링 대상의 **업무 과정 중 하나, 기본 엔티티로 부터 파생, 행위 엔티티 생성** / ex) 주문, 매출, 계약

<br>
<br>

### 엔티티 명명 주의점
  - 업무에서 실제 쓰이는 용어 사용
  - **한글 약어 사용 X**, 영어 **대문자**로 표시
  - **단수 명사로 표현**, 띄어쓰기 X
  - 의미상 중복 X (주문, 결제 엔티티는 중복 가능)
  - 명확하게 표현

<br>
<br>

### 속성이란, 속성의 특징
  - 엔티티의 특징을 나타내는 최소 데이터 단위 = 속성 (원자)

#### 속성의 특징
  - 더 이상 쪼개지지 않는 레벨
  - 업무에서 필요로 하는 항목
  - 엔티티를 설명, 인스턴스를 설명
  - 하나의 속성은 하나의 속성 값만 가짐 -> 여러 개 가지면 1차 정규화
  - 일반 속성은 정해진 주 식별자에 함수적 종속성을 가져야함
    - 완전 함수적 종속이 아닌 부분 종속이면 2차 정규화
      - ex) PK가 2개의 속성으로 이루어져 있는데, {속성1, 속성2}에서 속성 2에만 종속성을 가지면 2차 정규화로 엔티티 추가 생성해서 각 엔티티 마다 완전 함수적 종속 충족 시켜줌

<br>
<br>

### 속성의 특성에 따른 분류

#### 일반적인 특성에 따른 분류
  - **기본** 속성 : 업무 프로세스(기본 틀) 분석 후 **바로 정의 가능한 속성**
  - **설계** 속성 : 업무엔 없으나, 모델링 후 고유함을 보전하기 위해 필요해져서 만들어짐 / ex) 학번, 사번 등
    - **인스턴스에 유니크함을 부여하는 속성(PK의 토대)**
  - **파생** 속성 : 데이터를 조회할 때 빠른 성능을 낼 수 있도록 원래 속성 값을 계싼하여 저장할 수 있도록 하는 속성 / ex) 평균, 재고 등
    - 파생 = **성능, 편의를 위해 새로 만든 엔티티 속성**
    - **데이터 정합성 고려 & 가급적 적게 정의**

#### 구성 방식 (각 속성 및 엔티티와의 관계)에 따른 분류
  - PK 속성 : 인스턴스의 유니크함을 부여하는 속성, 일반 속성들의 종속성을 가진 키
    - 기본키, 주 식별자 키 : # 으로 표현
  - FK 속성 : 다른 엔티티에서 가져온 속성(외래키), 다른 엔티티와의 관계를 맺게 해줌
    - 기본키에 있는 속성이 FK가 될 수 있음 / ex) #사원번호(FK)
  - 일반 속성 : PK, FK를 제외한 나머지 속성

#### 속성의 분해 가능 여부에 따른 분류
  - **단일** 속성 : 속성이 하나의 의미로 구성
  - **복합** 속성 : 여러 개의 의미로 구성 / ex) 주소 = 시+구+동
  - **다중값** 속성: 속성이 여러 값을 가짐 -> 1차 정규화 or 별도 엔티티로 생성

<br>

### 속성이 만들어낸 데이터 모델의 개념
  - 도메인 : 속성이 가질 수 있는 속성 값의 범위
  - **용어 사전** : 속성의 이름을 정확, 직관적으로 부여하기 위한 용어 사전
  - **시스템 카탈로그** 
    - 시스템 자체에 관련 있는 데이터를 가진 DB
    - 시스템 테이블로 구성 & SQL로 조회 가능
    - 여기 저장된 데이터 = 메타 데이터, SELECT 만 가능

<br>
<br>

### 관계란? 관계의 종류
  - 엔티티와 엔티티 사이에 속성 끼리의 연결에 의해 만들어지는 상관 관계

#### 종류
  - 존재 관계 : 모델링 된 엔티티들이 존재로서 관계를 가짐
  - 행위 관계 : 모델링 된 엔티티들이 행위에 의해 관계를 가짐

#### UML의 클래스 다이어그램에 의해 나뉘는 종류
  - **연관** 관계
    - 필수적 관계(존재적 관계, 식별자 관계) - 항상 서로 이용 (실선)
    - **멤버 변수로 선언**
  - **의존** 관계
    - 선택적 관계(비식별자 관계) - 상대 클래스 행위에 따라 이용 (점선)
    - **행위 코드 오퍼레이션에서 파라미터로 사용**

#### 관계 표기 방법(ERD)에 따른 특성 분류
  - 관계명 : 관계 이름 = 시작 엔티티 - 능동적 / 끝 엔티티 - 수동적 + **'동사'** 사용
  - 관계 차수 : 각 엔티티 끼리의 관계에 참여하는 '속성의 수' 1:1, 1:M, M:N 형식으로 구분
  - 관계 선택 사양 : **필수적 관계**(엔티티끼리 항상 관계), **선택적 관계**(행위에 의해 관계 여부가 성립)
    - ex) 한 수업 엔티티에 참여 엔티티, 과제 엔티티가 있으면 참여는 수업이 있을 때 마다 항상 관계가 성립되어서 조회가 되지만, 과제는 과제가 있는 날에만 관계를 맺고 조회가 되기 때문에 이러한 것을 구분

<br>
<br>

### 관계 체크 사항 (두 엔티티 사이 관계 정의 시 유의할 사항)
  - 두 엔티티 사이에 **관심있는 연관 규칙**이 존재하는가
  - 두 엔티티 사이에 **정보의 조합**이 발생하는 가
  - 업무 기술 시, 장표의 관계 연결을 가능하게 하는 **동사**가 있는 가
  - 업무 기술 시, 장표의 관계 연결을 가능하게 하는 **규칙이 서술**되어 있는 가

<br>
<br>

### 식별자란? 주 식별자의 특성
  - 각각의 인스턴스를 구분 가능하게 만들어주는 대표 속성을 뜻함

#### 주 식별자란? 주 식별자의 특성을 #으로 표현
  - 주 식별자는 PK에 해당 하는 속성 -> PK는 한 테이블에 여러 개 존재할 수 없음
  - **유일성** : 해당 속성이 인스턴스를 유일하게 식별할 수 있는 성질을 가졌는 지
  - **최소성** : 최소한의 속성들로만 유일성을 보장하게 하는 지
  - **불변성** : 속성 값이 변하지 않아야함
  - **존재성** : 속성 값은 NULL이 될 수 없음
  - 유일성과 최소성을 만족하는 속성은 **보조키**

<br>
<br>

### 식별자의 특성과 특정 여부에 따른 분류

#### 대표성 여부
  - 주 식별자(PK) - #으로 표현
    - **유일성, 최소성, 불변성, 존재성**을 모두 만족하는 식별자
    - PK는 여러 속성이 존재할 수 있으나, 여러 속성이 존재할 경우 나머지 일반 속성들이 해당 PK 속성들에 대해 함수적 종속성을 띄어야함
    - 그렇지 않으면 2차 정규화하여 부분 종속에 해당하는 속성들만 따로 추가 엔티티를 생성해야함
      - **주 식별자 도출 기준**
        - 해당 업무에서 자주 이용되는 속성
        - 명칭, 내역 등의 이름은 피함
        - 속성 수를 최대한 적게 구성
        - 자주 변하지 않는 값
  - **보조 식별자**
    - 인스턴스 **식별은 가능하나**, 엔티티를 **대표**하는 식별자는 아님
    - 즉, 다른 엔티티와의 참조 관계로 연결되지 않음
    - ex) 회원 엔티티에 #회원번호, *회원명, *아이디 가 있는 경우 아이디는 다른 인스턴스와 중복될 수 없기 때문에 해당 엔티티에서 인스턴스를 구분 짓게 할 수 있는 식별자 이지만 이게 엔티티를 대표하진 못함

#### 스스로 생성 되었는가에 대한 여부
  - **내부** 식별자 : 다른 엔티티 참조 없이 해당 엔티티 내부에서 스스로 생성된 식별자
  - **외부** 식별자 : 다른 엔티티에서 온 식별자 - 다른 엔티티와 연결고리 역할
    - 만약 부모 엔티티의 FK를 받아서 이를 주 식별자로 사용하게 되면 해당 자식 엔티티의 PK는 SQL 조인에서 반드시 사용되고 WHERE 절에서 사용 가능성이 높음

#### 단일 속성인지에 대한 여부 (주 식별자 구성이 여러 속성인가)
  - **단일** 식별자 : 주 식별자가 1개의 속성으로 구성
  - **복합** 식별자 : 주 식별자가 2개 이상의 속성으로 구성
    - 주 식별자가 2개 이상이면 해당 속성들의 우선 순위를 잘 매겨서 복합시킨 후 일반 속성들에게 종속시켜야 주 식별자로서 기능을 다 하게 됨

#### 대체 되었는 지 기존에 있는 지에 대한 분류
  - **원조(본질)** 식별자 : 업무에 의해 만들어지는 식별자, 가공되지 않은 원래 식별자
  - **인조(대리)** 식별자 : 인위적으로 만들어지는 식별자, 주 식별자가 복잡할 때 이를 통합함
    - ex) 주문 번호 : 대표적 인조, 대리 식별자 / 기존 주 식별자를 두고 주문 처리하다가 "주문번호"라는 단일 속성의 주 식별자로 만들면 인조, 대리 식별자가 됨

<br>
<br>

### 식별자 관계 vs 비식별자 관계

#### 식별자 관계
  - 트랜잭션에 의한 관계 - 동시에 커밋, 롤백 - 하나의 커밋 단위로 엔티티들이 묶임
  - 부모 엔티티의 식별자 속성이 **자식 엔티티의 주 식별자**가 되는 관계
    - 강한 연결 관계
    - 실선 (항시 연결)
    - 부모-자식 관계가 항시 유지
    - **SQL 문의 조인을 최소화 해줌**

#### 비식별자 관계
  - 부모 엔티티의 식별자 속성이 **자식 엔티티의 일반 속성**이 되는 관계
    - 약한 연결 관계
    - 점선 (선택적 연결)
    - 부모-자식 관계가 유지 안 될 수 있음
  - 일반 속성 값은 NULL이 들어갈 수 있기 때문에 부모 엔티티의 식별자 속성에 값이 없을 때 자식 엔티티의 속성 값(인스턴스) 생성 가능

<br>
<br>
<br>

## 데이터 모델과 SQL

### 성능 데이터 모델링의 개요

#### **성능 데이터 모델링**의 정의
  - 성능 저하의 원인 중 하나는 데이터 모델링의 근본적인 디자인이 잘못되어 있는 경우가 많음
  - 따라서 성능 데이터 모델링을 통해 성능 향상을 도모해야함
  - 성능 데이터 모델링이란?
    - 데이터 베이스 성능 향상읋 목적으로 설계 단계의 데이터 모델링 때 부터 성능과 관련된 사항이 모델링에 반영될 수 있도록 하는 것

#### 성능 데이터 모델링 수행 시점
  - 사전에 성능 모델링을 할 수록 성능 향상을 위한 비용은 적게 듬
  - 분석/설계 단계에서 성능을 고려해 데이터 모델링을 수행할 경우 재업무 비용을 최소화할 수 있음
  - 따라서 분석/설계 단계에서 처리 성능을 향상시킬 방법을 고려해야 함

#### 성능 데이터 모델링 고려사항
  - **성능 데이터 모델링 프로세스**
    - **정규화** -> 정규화가 1등
    - DB **용량 산정**
    - **트랜잭션의 유형 파악 -> 테이블 수직 분할 할 때 (반정규화)**
    - 용량과 트랜잭션의 유형에 따라 **반정규화**
    - 이력 모델 조정, PK/FK 조정, 슈퍼타입/서브타입 조정
    - 성능 관점에서 데이터 모델을 **검증**

<br>
<br>

### 정규화

#### 정규화란?
  - 엔티티를 작은 단위로 분리하는 과정
    - 큰 엔티티를 작은 엔티티들로 분리하고 관계 맺음
  - **논리 데이터 모델에서 행하는 과정**임 (개념 X, 물리 X)

#### 정규화의 특징 및 하는 이유와 개념 (장점)
  - 데이터 무결성을 위해 수행
  - 최소한의 데이터 만을 하나의 엔티티에 넣는 과정, 데이터 분해 과정
  - 데이터 **일관성 확보**
  - 데이터 **독립성 확보** -> 중복 제거
  - 데이터 **유연성 확보** -> 필요 데이터 들의 분할로 인해 유연하게 접근 가능
  - **입력, 수정, 삭제 성능은 일반적으로 향상됨**

#### 정규화 단점
  - 엔티티 갯수 증가
  - 이로 인한 관계 증가
  - 데이터 조회 시 여러 번의 조인 요구
  - **조회 성능 저하**

<br>
<br>

### 정규화 종류
  - 각 정규화를 통해 이루어지는 행위가 있는데, 이 행위를 만족하는 엔티티 구조는 제 1, 2, 3 정규형 릴레이션이라고 함

#### 제 1 정규화
  - 테이블 칼럼들이 원자성을 갖게 하기 위해 엔티티 분해
    - 하나의 인스턴스가 비슷한 속성을 여러 개 가지지 않게 하기 위해 분리하는 것

![](/assets/img/Certificates/SQLD/normalization_1.png)
![](/assets/img/Certificates/SQLD/normalization_2.png)

#### 제 2 정규화
  - 엔티티의 모든 일반 속성은 반드시 주 식별자의 모든 속성들에 '부분 종속'이 아닌 '완전 종속'을 가져야 함
  - 이 때 만약 '부분 종속'을 가지는 일반 속성이 있다면 해당 속성과 해당 속성의 결장자인 부분 종속을 이루고 있는 주 식별자의 속성을 따로 떼어내 추가적인 엔티티를 만들어 제 2 정규형을 만족하는 릴레이션을 구축하는 것
  - 또는 주 식별자의 속성이 아닌 일반 속성끼리 종속 관계를 맺어도 이에 대해 해당 일반 속성이 새로운 엔티티에서 제 2정규형을 만족하도록 엔티티를 추가적으로 만들어 줌
  - ex) 엔티티 1에서 A -> B 라면 B는 엔티티 1에서 제거하고 A는 엔티티 1에 남겨 둔 채로 엔티티 2를 만들고 B를 엔티티 2의 일반 속성으로, 엔티티 1의 일반 속성 A를 FK로 사용하여 엔티티 2의 주식별자로 엔티티를 구축하고 릴레이션을 유지하게 하는 것

#### 제 3 정규화
  - 정규화된 엔티티의 일반 속성들은 주식별자에만 함수적 종속을 가져야 함
  - 그런데 만약 주식별자의 속성들끼리 종속 관계를 가지고, 그 후에 또 일반 속성에 대해 결정자가 되던지, 일반 속성끼리 종속성을 가지는데, 이 때의 결정자가 주식별자 속성에 종속되어 있는 등의 A -> B, B -> C 와 같은 '이행적 종속'을 이루는 TABLE(엔티티) 일 때 **'이행적 종속'을 깨도록 추가적인 엔티티를 만들고 관계를 형성**해주는 것이 제 3 정규화

#### BCNF 정규화
  - **모든 결정자가 후보키가 되도록 테이블을 분해**하는 것
    - 후보키 : 식별자의 '유일성', '최소성'을 만족하는 속성 집합(or 단일 속성)

#### 제 4 정규화
  - 여러 칼럼이 하나의 칼럼을 종속 시킬 때 분해해서 **'다중값 종속성'** 제거

#### 제 5 정규화
  - **조인에 의해 새로운 종속성 발생** 시 이를 막기 위해 엔티티 재 분해

<br>
<br>

### 반정규화

#### 반정규화란? 특징과 하는 이유
  - **'정규화 된'** 데이터 모델(엔티티, 속성, 관계)에 대해 **'성능 향상'**, **'개발, 운영의 단순화'**를 위해 **데이터를 중복, 통합, 분리 하는 기법**
  - 정규화 시 엔티티 갯수 증가, 관계 증가 -> 여러 조인 요구
    - 이 경우 디스크 I/O 양이 많아지기 때문에 성능이 저하되거나 경로가 멀어서 '조인'으로 인한 성능 저하
  - 비정규화 = 정규화 X

#### 반정규화 특징
  - **조회(SELECT) 성능 향상**
  - 데이터 모델의 유션성 저하
  - 입력/수정/삭제 성능 저하

#### 반정규화 하는 경우
  - 정규화를 통해 에닡티, 관계 수가 많아져서 조회 시 '조인'으로 인한 성능 저하가 예상될 때
  - 칼럼을 계산하고 읽을 때 FK라서 여러 조인이 발생하여 성능이 저하될 때
  - 즉, **조인으로 인한 I/O 양이 많아져서 처리 성능이 저하 될 때**
  - **중복성을 증가시켜 조회 성능을 향상**시킴

#### 반정규화 안하면 발생하는 문제
  - 성능 저하된 DB 생성
  - 구축, 시험 단계에서 수정에 따른 노력 비용 발생

<br>
<br>

### 테이블을 가지고 반정규화 하는 방법 (병합, 분할, 추가)

#### 테이블 병합
  - 1:1 관계 테이블 병합
  - 1:M 관계 테이블 병합
  - 슈퍼/서브 타입 테이블 병합
    - 공통 속성과 개별 속성을 별도로 관리하는 설계 타입

#### 테이블 분할
  - 테이블 **수직 분할** (속성 분할)
    - **트랜잭션 처리 유형 파악** 필요 -> 반정규화에서 테이블 수직 분할 할 때 필요
      - 테이블 속성 개수 많을 때, 조회 성능 향상을 위해 자주 쓰이는 속성을 수직 분할, 이후 **1:1 관계를 이룸**
  - 테이블 **수평 분할** (인스턴스 분할, **파티셔닝**)
    - 물리적으로 데이터 분리

#### 테이블 추가
  - **중복** 테이블 추가
    - 동일한 테이블 구조 중복, **원격 조인 제거**
  - **통계** 테이블 추가
    - SUM, AVG 등 전용 테이블 추가
  - **이력** 테이블 추가
    - 마스터 테이블의 레코드를 긁어서 테이블 추가 생성
  - **부분** 테이블 추가
    - 이용 빈도 높은 칼럼을 복사하여 별도 테이블 생성, 물리적 디스크 I/O 줄이기 위함

<br>
<br>

### 칼럼을 통해 반정규화 하는 방법
  - 중복 칼럼 추가 -> 중복 추가는 Join 감소를 위함 (중복 테이블 추가)
    - ex) 최근 상품 가격
  - 파생 칼럼 추가 -> 파생 속성 -> 부하 줄이기
    - ex) 미리 값을 계산하여 컬럼에 보관 (SUM 등)
  - 이력 테이블 칼럼 추가
    - 대량의 이력 데이터를 처리할 때 기능성 칼럼(최근 값 여부, 시작&종료일 등)을 추가
  - PK에 의한 칼럼 추가
    - 여러 칼럼으로 이루어진 PK를 가진 테이블을 조인할 경우 단순성을 위해 인공키를 PK로 지정하고 활용
  - 응용 시스템 오작동을 위한 이전 데이터 보관 칼럼 추가
    - 이전 데이터를 임시적으로 중복하여 보관

<br>
<br>

### 관계를 통해 반정규화 하는 방법

#### 중복 관계 추가 방법
  - 여러 경로를 거쳐 조인할 수 있지만, 성능 저하를 예방하기 위해 추가적인 관계를 맺음
  - **중복 관계 추가는 데이터 무결성을 깨뜨릴 위험성이 없음**
    - 이에 무결성을 지키면서 처리 성능을 향상 시킬 수 있음

<br>
<br>

### 관계와 조인

#### 관계란?
  - 부모 엔티티의 식별자를 자식에 상속하고, **상속된 속성**을 **매핑키(조인키)** 로 활용
  - **관계의 분류**
    - 존재 관계
    - 행위 관계

#### 조인이란?
  - 데이터 중복을 피하기 위해 테이블을 정규화에 의해 분리
    - 이렇게 분리된 테이블을 동시에 출력하거나 관계가 있는 테이블을 참조하기 위해서는 테이블을 연결 함, 이러한 연결 과정을 JOIN이라 함

#### 계층형 데이터 모델
  - 하나의 엔티티 내에서 인스턴스끼리 계층 구조를 가지는 경우
    - 계층 구조를 갖는 인스턴스끼리 연결하는 조인을 **셀프 조인**이라고 함 (같은 테이블 내에서 여러 번 조인 되는 것)
    - ex) 인스턴스 A 내에 속성 B에 대한 값이 해당 엔티티 내 다른 인스턴스에 있는 값이어서 이 들 두 속성을 같이 SELECT 하고, WHERE & AND 로 조건을 적용 후 조회할 때 SELECT에 의해 하나의 에닡티 내에서 여러 번 조인이 발생

#### 상호배타적 관계
  - 하나의 부모가 2개의 자식 엔티티를 가질 때 행위 조건에 따라 두 자식 중 하나의 자식만 관계를 가질 수 있는 것

<br>
<br>

### 트랜잭션이란?

#### 트랜잭션의 특징
  - 하나의 연속적인 업무 단위를 뜻함
  - 트랜잭션에 묶인 엔티티들은 '필수적 관계'를 가짐
  - 하나의 트랜잭션에 속한 동작들은 모두 성공하거나, 모두 취소되어야 함
  - 서로 독립적으로 업무 발생 안됨, 순차적으로 함께
  - 부분 커밋 불가, 동시 커밋&롤백
  - ACID

<br>

### 본질 식별자와 인조 식별자

#### 원조(본질) 식별자
  - 업무에 의해 만들어지는 식별자 (반드시 필요함)

#### 인조(대리) 식별자
  - 원조 식별자가 PK 2개 이상인 복합 식별자 일 때, 속성들을 하나의 속성으로 묶어서 사용하면 인조 식별자임
  - 꼭 필요하진 않지만 편의성을 위해 인위적으로 만들어지는 것
    - **인조 식별자 단점**
      - **중복 데이터 발생 가능성** -> 데이터 품질 저하
      - **불필요한 인덱스 생성** -> 저장 공간 낭비 및 DML 성능 저하
      - **개발 편의성 저하**

<br>
<br>
<br>

## 참고

#### 엔티티
  - 실체, 객체
  - 업무에 필요하고 유용한 정보를 저장, 관리하고자 하는 정보
  - ex) 학생이라는 엔티티는 학번, 이름, 전공과 같은 속성을 가짐
  - **특징**
    - 영속적으로 존재하는 인스턴스의 집합
    - 반드시 속성을 가짐
    - 최소 한 개 이상의 관계를 가짐

#### 인스턴스
  - DB에 저장된 데이터 내용의 전체 집합

#### 실제 테이블 예시

![](/assets/img/Certificates/SQLD/table_example.png)

  - 엔티티 = 테이블
  - 속성 = 열
  - 인스턴스 = 행

#### 엔티티, 인스턴스, 속성, 값 간의 관계

![](/assets/img/Certificates/SQLD/table_relations.png)