---
title: "SQLD / 모의고사 정리 3"
author: Jeremiah Lee
date: 2024-11-13
categories: [ SQLD, 모의고사 ]
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

영진닷컴 이기적 CBT 모의고사 참조

<br>
<br>
<br>

#### 다음 SQL문의 실행 결과로 올바른 것을 고르시오.

![](/assets/img/Certificates/SQLD/SQLD_MOCKTEST_4.png)

1. 10, 20
2. 10, 20, 30
3. 10, 20, 30, 40
4. 10, 20, 30, 40, 50

##### 정답 : 2
  - ALL 연산자는 서브쿼리 값 모두가 조건에 만족하면 true 리턴

<br>

#### 실행 계획에 대한 설명으로 적절하지 않은 것은?

1. 실행 계획은 SQL문의 처리를 위한 절차와 방법이 표현된다.
2. 실행 계획이 다르면 결과도 달라질 수 있다.
3. 실행 계획은 액세스 기법, 조인 순서, 조인 방법 등으로 구성된다.
4. 최적화 정보는 실행 계획의 단계별 예상 비용을 표시한 것이다.

##### 정답 : 2
  - 동일 SQL문에 대해 실행 계획이 다르다고 결과가 달라지지 않음
  - 단, 실행 계획 차이로 성능은 달라질 수 있음

<br>

#### JOIN의 종류에 대한 설명으로 틀린 것은 무엇인가?

1. NON-EQUI JOIN은 등가 조건이 성립되지 않은 테이블에 JOIN을 걸어주는 방법이다.
2. EQUI JOIN은 반드시 기본키, 외래키 관계에 의해서만 성립된다.
3. OUTER JOIN은 JOIN 조건을 만족하지 않는 데이터도 볼 수 있는 JOIN 방법이다.
4. SELF JOIN은 하나의 테이블을 논리적으로 분리시켜 EQUI JOIN을 이용하는 방법이다.

##### 정답 : 2
  - EQUI JOIN은 반드시 기본키, 외래키 관계에 의해서만 성립되는 것은 아님
  - 조인 컬럼이 1:1로 매핑이 가능하면 사용 가능
  - **EQUI JOIN** : 두 테이블에서 공통적으로 존재하는 컬럼의 값이 일치하는 행을 연결해서 결과 리턴
  - **NON-EQUI JOIN** : JOIN 조건을 '=' 연산자 이외의 비교 연산자를 사용하는 것

<br>

#### 다음 설명 중 올바르지 않은 것은?

1. SQL Server는 null 값을 인덱스 맨 뒤에 저장한다.
2. Oracle에서 인덱스 구성 컬럼 중 하나라도 null이 아닌 레코드는 인덱스에 저장한다.
3. SQL Server는 인덱스 구성 컬럼이 모두 null인 레코드도 인덱스에 저장한다.
4. Oracle에서 인덱스 구성 컬럼이 모두 null인 레코드는 인덱스에 저장하지 않는다.

##### 정답 : 1
  - SQL Server는 null 값을 인덱스 맨 앞에 저장하고, Oracle은 맨 뒤에 저장함

<br>

#### ANSI/ISO 표준 SQL에서 두 테이블 간에 동일한 컬럼 이름을 가지는 것을 모두 출력하는 조인 방식은 무엇인가?

1. Inner Join
2. Cross Join
3. Natural Join
4. Using

##### 정답 : 3
  - Using : 공통된 컬럼 이름을 기준으로 조인
  - 차이 : Natural은 동일 이름 자동 감지, Using은 사용자가 조인할 컬럼 명시적 지정

<br>

#### 다음 모델의 배송 엔티티에서 고객의 정보를 찾을 때, 성능 향상과 SQL 문장을 단순화하는 가장 적절한 반정규화 방법은 무엇인가? (단, 주문목록 엔티티에서는 고객의 주식별자를 상속받기를 원하지 않음, 배송 엔티티에서는 고객 엔티티의 모든 속성을 참조하기를 원함)

1. 고객과 배송 엔티티의 관계를 추가(1:M)하는 관계 반정규화
2. 배송과 고객의 엔티티를 통합하는 반정규화
3. 배송 엔티티와 주문목록 엔티티 관계를 식별자 관계로 수정
4. 고객의 모든 정보를 모두 배송 엔티티의 속성으로 반정규화

##### 정답 : 1
  - 고객 엔티티의 모든 속성을 참조하기를 원할 때 가장 효율성이 좋은 반정규화 기법은 관계를 중복하는(관계의 반정규화) 방법이며 이를 적용하면 두 테이블의 조인 경로를 단축하게 되고 SQL 문장을 단순하게 구성할 수 있다.


