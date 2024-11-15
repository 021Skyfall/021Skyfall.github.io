---
title: "SQLD / 모의고사 정리 2"
author: Jeremiah Lee
date: 2024-11-12
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

#### 다음 서브쿼리에 대한 설명 중 틀린 것을 고르시오.

1. 상호연관 서브쿼리는 처리 속도가 가장 빠르기 때문에 최대한 활용하는 것이 좋다.
2. TOP-N 서브쿼리는 INLINE VIEW의 정렬된 데이터를 ROWNUM을 이용해 결과 행 수를 제한하거나 TOP (N) 조건을 사용하는 서브쿼리이다.
3. INLINE VIEW는 FROM절에 사용되는 서브쿼리로서 실질적인 OBJECT는 아니지만, SQL 문장에서 마치 VIEW나 테이블처럼 사용되는 서브쿼리이다.
4. 다중행 연산자는 IN, ANY, ALL이 있으며 서브쿼리의 결과로 하나 이상의 데이터가 RETURN되는 서브쿼리이다.

##### 정답 1
  - 상호 연관 서브쿼리는 서브쿼리가 메인쿼리의 행 수 만큼 실행되는 쿼리로서 실행 속도가 상대적으로 떨어지는 SQL 문장임
  - 그러나 복잡한 일반 배치 프로그램을 대체할 수 있기 때문에 조건에 맞는다면 사용

<br>

#### 다음 주어진 그룹 함수와 동일한 결괏값을 반환하는 그룹 함수를 고르시오.

```
GROUP BY CUBE(DEPTNO, JOB);
```

1. GROUP BY ROLLUP(DEPTNO, JOB);
2. GROUP BY (DEPTNO, JOB, (DEPTNO, JOB), ());
3. GROUP BY DEPTNO UNION ALL GROUP BY JOB UNION ALL GROUP BY (DEPTNO, JOB);
4. GROUP BY GROUPING SETS (DEPTNO, JOB, (DEPTNO, JOB), ());

##### 정답 : 4
  - CUBE는 CUBE 함수에 제시된 컬럼에 대해 결합 가능한 모든 집계를 계산함
  - CUBE(1, 2) : 1의 집계, 2의 집계, 1과 2의 집계, 총계

<br>

#### 관계를 정의할 때 주요하게 체크해야 하는 사항과 거리가 먼 것은?

1. 두 개의 엔티티 사이에 관심 있는 연관 규칙이 존재하는가?
2. 업무기술서, 장표에 관계 연결을 가능하게 하는 명사(Noun)가 있는가?
3. 업무기술서, 장표에 관계 연결 규칙이 서술되어 있는가?
4. 두 개의 엔티티 사이에 정보의 조합이 발생되는가?

##### 정답 : 2
  - 관계를 정의할 때 주요하게 체크해야 하는 사항은 업무기술서, 장표에 관계연결을 가능하게 하는 "**동사**"가 있는가이다.

<br>

#### 다음 SQL문의 결과로 출력되는 데이터는 무엇인가?

```
SELECT NEXT_DAY(ADD_MONTHS (sysdate, 6), '월요일') FROM DUAL
```

1. 오늘 날짜로부터 6일 후 첫 번째 월요일을 출력한다.
2. 오늘 날짜로부터 6개월 후 두 번째 월요일을 출력한다.
3. 오늘 날짜로부터 6개월 후 첫 번째 월요일을 출력한다.
4. 오늘 날짜로부터 6일 후 두 번째 월요일을 출력한다.

##### 정답 : 3
  - ADD_MONTHS 함수는 6개월을 더하고 NEXT_DAY 함수는 지정된 요일의 첫 번째 날짜를 출력한다.

<br>

#### 윈도우 함수 중에서 제일 먼저 나오는 것을 0으로 하고 제일 늦게 나오는 것을 1로 해서 행 순서별 백분율을 구하는 것은?

1. FIRST_VALUE
2. LAST_VALUE
3. PERCENT_VALUE
4. CUME_DIST

##### 정답 : 3
  - FIRST_VALUE : 지정 윈도우 내에서 첫 번째 값 리턴
  - LAST_VALUE : 지정 윈도우 내에서 마지막 값 리턴 
  - CUME_DIST : 누적 분포 함수

<br>

#### 옵티마이저에 대한 설명으로 적절하지 않은 것은?

1. 옵티마이저는 질의에 대해 실행 계획을 생성한다.
2. 비용 기반 옵티마이저는 적절한 인덱스가 존재하면 반드시 인덱스를 사용한다.
3. 규칙 기반 옵티마이저에서 제일 낮은 우선 순위는 전체 테이블 스캔이다.
4. 비용 기반 옵티마이저는 비용 계산을 위해 다양한 통계 정보를 사용한다.

##### 정답 : 2
  - 비용 기반 옵티마이저는 비용을 기반으로 최적의 작업을 수행함
  - 따라서 인덱스 스캔 보다 전체 테이블 스캔이 비용이 낮다고 판단하면 적절한 인덱스가 존재해도 전체 테이블 스캔으로 SQL문을 수행함
  - 무조건 비용 따라 갈림

<br>

#### 다음 설명 중 적절한 것은 무엇인가?

1. 인덱스는 인덱스 구성 칼럼으로 항상 오름차순으로 정렬된다.
2. 비용 기반 옵티마이저는 인덱스 스캔이 항상 유리하다고 판단한다.
3. 규칙 기반 옵티마이저는 적절한 인텍스가 존재하면 항상 인덱스를 사용하려고 한다.
4. 인덱스 범위 스캔은 항상 여러 건의 결과가 반환된다.

##### 정답 : 3
  - 인덱스는 내림차순 정렬
  - 비용 기반 옵티마이저는 비용에 따라 다름
  - 규칙 기반 옵티마이서는 인덱스 우선
  - 인덱스 범위 스캔은 결과 건수만큼 반환

<br>

#### 다음과 같은 SQL 문장이 있다. 예제의 ORDER BY절과 같은 결과를 같는 구문은 어떤 것인가?

```
SELECT PLAYER_NAME 선수명, POSITION 포지션, BACK_NO 백넘버
FROM PLAYER
ORDER BY PLAYER_NAME, POSITION, BACK_NO DESC;
```

1. ORDER BY 선수명 ASC, 포지션, 3 DESC
2. ORDER BY 선수명, 2 DESC 백넘버
3. ORDER BY PLAYER_NAME, ASC, 2, 3
4. ORDER BY 1 DESC, 2, 백넘버

##### 정답 : 1
  - ORDER BY 절에서는 컬럼명 대신 SELECT 절에 기술한 컬럼의 순서 번호나 ALIAS 가능

<br>

#### Subquery의 종류 중에서 Subquery가 Mainquery의 제공자 역할을 하고 Mainquery의 값이 Subquery에 주입되지 않는 유형은 무엇인가?

1. Filter형 Subquery
2. Early Filter형 Subquery
3. Associative Subquery
4. Access Subquery

##### 정답 : 4
  - 제공자 역할 = Access 서브쿼리
  - Filter형 서브쿼리 : 특정 조건을 만족하는 데이터만 반환하여 주 쿼리 결과를 제한
  - Early Filter형 서브쿼리 : 주 쿼리가 실행되기 전에 먼저 실행되어 불필요한 데이터 처리 줄임
  - Associative 서브쿼리 : 주 쿼리와의 관계를 기반으로 데이터를 가져옴

<br>

#### 일반적으로 FROM절에 정의된 후 먼저 수행되어 SQL 문장 내에서 절차성을 주는 효과를 볼 수 있는 것은 어떤 유형의 서브쿼리 문장인가?

1. SCALA SUBQUERY
2. INLINE VIEW
3. CORRELATED SUBQUERY
4. NESTED SUBQUERY

##### 정답 : 2
  - FROM절에 정의된 서브쿼리는 INLINE VIEW 밖에 없음
  - SCALA 서브쿼리 : 단일 값(스칼라 값)을 리턴하는 서브쿼리, 결과가 항상 하나의 값이므로 주쿼리 내에 직접 사용
  - CORRELATED 서브쿼리 : 상관 서브쿼리, 주 쿼리의 각 행에 대해 서브쿼리가 실행되는 서브쿼리
  - NESTED 서브쿼리 : 중첩 서브쿼리, 다른 서브쿼리 안에 위치한 서브쿼리, 주로 WHERE절이나 SELECT절에 포함

<br>

#### 서브쿼리의 종류 중에 서브쿼리를 실행하고 한 행, 한 컬럼을 반환하는 서브쿼리를 무엇이라고 하는가?

1. Looping
2. Scala Subquery
3. Associate Subquery
4. Access Subquery

##### 정답 : 2
