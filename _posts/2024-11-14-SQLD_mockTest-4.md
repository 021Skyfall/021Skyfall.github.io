---
title: "SQLD / 모의고사 정리 4"
author: Jeremiah Lee
date: 2024-11-14
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

#### 다음의 내용 중에서 ROWNUM을 올바르게 사용하지 않은 것은?

1. SELECT ROWNUM, ENAME EMP;
2. SELECT EMPNO FROM EMP WHERE ROWNUM=1;
3. SELECT ENAME FROM EMP WHERE ROWNUM=2;
4. SELECT DEPTNO FROM EMP WHERE ROWNUM < 10;

##### 정답 : 3
  - ROWNUM은 쿼리 결과의 행 번호를 리턴함
  - 실행될 때 각 행에 대한 번호를 부여함
  - 문제의 쿼리는 2인 행을 선택하려고 하지만, ROWNUM은 쿼리가 실행되는 순서대로 부여되기 때문에 다음과 같은 이유로 실행이 불가능
    - ROWNUM은 첫 번째 행에 1, 두 번째 행에 2를 할당함, 즉, 쿼리가 실행되는 동안 ROWNUM은 1부터 시작하여 증가
    - 그런데 WHERE절에서 조건을 평가하는 시점에 ROWNUM이 2인 행은 아직 없음
    - 그래서 ROWNUM=2는 불가능
    - 이를 해결하기 위해 서브쿼리를 사용해야함

<br>

#### 다음 테이블에 대한 매출 누적을 구하는 SQL문을 작성하시오. (윈도우 함수 사용)

1. SELECT 영업사원, 판매월, SUM(매출) OVER (PARTITION BY 영업사원 ORDER BY 판매월 RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 누적매출 FROM 매출;
2. SELECT 영업사원, 판매월, SUM(매출) OVER (PARTITION BY 영업사원 ORDER BY 판매월 RANGE BETWEEN UNBOUNDED PRECEDING) 누적매출 FROM 매출;
3. SELECT 영업사원, 판매월, SUM(매출) OVER (PARTITION BY 판매월 RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 누적매출 FROM 매출;
4. SELECT 영업사원, 판매월, SUM(매출) FROM GROUP BY 영업사원, 판매월;

##### 정답 : 1
  - 영업사원 별 누적 매출이므로 "PARTITION BY 영업사원"을 사용해야함.
  - 그리고 UNBOUNDED PRECEDING과 CURRENT ROW는 시작부터 현재 행 까지를 의미함

<br>

#### 다음 설명이 나타내는 데이터 모델의 개념은 무엇인가?

```
학생이라는 엔티티가 있을 때 학점이라는 속성값의 범위는 0.0에서 4.0 사이의 실수값이며 
주소라는 속성은 길이가 20자리 이내의 문자열로 정의할 수 있다.
```

1. 시스템 카탈로그
2. 용어 사전
3. 성 사전
4. 도메인

##### 정답 : 4
  - 속성에 대한 제약사항 = 도메인
  - 시스템 카탈로그 : 시스템 자체에 관련 있는 데이터를 가진 DB, 시스템 테이블로 구성되며 SQL로 조회 가능
  - 용어 사전 : 속성의 이름을 정확하고 직관적으로 부여하기 위한 사전
  - 성 사전(attribute) : 특정 엔티티나 객체가 가지는 특성을 나타내는 사전

<br>

#### 다음 중 인덱스 튜닝에 대해 잘못 설명하고 있는 것은?

1. 인덱스를 경유한 테이블의 Random 액세스 부하가 심할 때, 클러스터 테이블이나 IOT를 활용하는 방안을 고려할 수 있다.
2. 인덱스를 경유한 테이블 액세스 횟수가 같더라도 인덱스 구성에 따라 스캔 효율이 달라진다. 따라서 인덱스 스캔 효율을 높이기 위해 인덱스 칼럼 순서를 바꿔야 할 때가 종종 있다.
3. 조건절이 Where deptno = 10 and ename = "SCOTT"일 때 인덱스를 [deptno + ename] 순으로 구성하나 [ename + deptno] 순으로 구성하나 인덱스 스캔 효율에 차이가 없다.
4. 인덱스 튜닝의 핵심 요소 중 하나는 불필요한 테이블 Random 액세스가 발생하지 않도록 하는 데에 있다. 이를 위해 인덱스 컬럼 순서를 바꿔주는 것도 큰 효과가 있다.

##### 정답 : 4
  - 인덱스 컬럼 순서를 아무리 바꿔도 테이블 Random 액세스 횟수는 줄지 않음

<br>

#### 엔티티 - 인스턴스 - 속성 - 속성값에 대한 관계 설명 중 틀린 것을 고르시오.

1. 한 개의 에닡티는 두 개 이상의 인스턴스 집합이어야 한다.
2. 하나의 속성은 하나 이상의 속성값을 가진다.
3. 한 개의 엔티티는 두 개 이상의 속성을 갖는다.
4. 엔티티 하나의 인스턴스는 다른 엔티티의 인스턴스 간 관계인 Pairing을 가진다.

##### 정답 : 2
  - 하나의 속성은 하나의 속성값을 가지며, 하나 이상의 속성값을 가지는 경우에는 정규화가 필요함
