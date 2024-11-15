---
title: "SQLD / 2 과목 - SQL 기본 및 활용"
author: Jeremiah Lee
date: 2024-11-05
categories: [ SQLD, 개념 ]
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
[돌멩이네](https://blog.naver.com/ekf1121_) 참조

학습 목적으로 직접 타이핑해서 옮김.     
문제 시 삭제.

2 과목의 경우 간단히 정리하고 최대한 문제 풀이 위주로 진행

<br>
<br>
<br>

# 2 과목

## SQL 기본

### 관계형 데이터베이스 개요
  - 효율적인 데이터 관리와 데이터 손상을 피하고 복구를 위한 시스템 (DBMS)

#### 명령어 종류
  - **DML**
    - SELECT / INSERT / UPDATE / DELETE
  - **DDL**
    - CREATE / ALTER / DROP / RENAME
  - **DCL**
    - GRANT / REVOKE
  - **TCL**
    - COMMIT / ROLLBACK
  - **일반 집합 연산자**
    - UNION / INTERSECTION / DIFFERENCE / PRODUCT(CROSS JOIN)
  - **순수 관계 연산자**
    - SELECT / PROJECT / JOIN / CHARACTER(s) / VARCHAR(s) / NUMERIC / DATETIME

<br>
<br>

### SELECT 문

```
SELECT [ALL/DISTICT/(*)] 칼럼명1 [(as) ALIAS], 칼럼명2 ...
FROM 테이블명;
```

#### ALIAS (별칭)
  - 칼럼명 바로 뒤에 위치
  - 칼럼명과 ALIAS 사이에 AS 키워스 사용 가능
  - 이중 인용부호(큰 따옴표)는 ALIAS가 공백, 특수문자를 포함하는 경우나 대소문자 구분이 필요할 때 사용

#### 합성 연산자 (합침)
  - 수직바 \|| (Oracle)
  - 플러스 + (SQL server)
  - CONCAT (string1, string2)

```
SELECT first_name || last_name AS full_name
FROM employees;
```

#### 쿼리 실행 순서
  - FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY

<br>
<br>

### 함수

#### 함수 종류
  - **문자형 함수**
    - CONCAT / UPPER / LOWER / SUBSTR / LENGTH / TRIM / REPLACE / INSTR / LEFT / RIGHT / MID 등
  - **숫자형 함수**
    - ABS / POWER / ROUND / CEIL (올림) / FLOOR (내림) / TRUNC(숫자,[m]) (숫자를 m 자리에서 반올림, m 생략 시 디폴트 0) / MOD / SIGN(양수, 음수, 0인지 구별) / SQRT / EXP (지수 값 리턴) / LOG 등
  - **날짜형 함수**
    - DATEADD / DATEDIFF / YEAR / MONTH / DAY / HOUR / MINUTE / SECOND / DATEPART / GETDATE 등
  - **변환형 함수**
    - CAST / CONVERT / TO_CHAR / TO_NUMBER / TO_DATE / NUMTOYMINTERVAL (시간, 날짜 interval을 date 타입으로) / TO_TIMESTAMP 등

#### 단일행 NULL 관련 함수 종류
  - NVL(표현식1, 표현식2) : 표현식 1 이 NULL 이면 표현식 2 출력
  - NVL2(표현식1, 표현식2, 표현식3) : 표현식1 이 NULL이면 표현식3, 아니면 표현식2 출력
  - NULLIF(표현식1, 표현식2) : 표현식1==표현식2 면 NULL 리턴, 표현식1<>표현식2 면 표현식1 리턴
  - COALESCE(표현식1, 표현식2) : 임의의 개수 표현식 중 NULL이 아닌 표현식 들을 리턴 (병합)

<br>
<br>

### WHERE 절
  - 조건식
  - 칼럼명 비교연산자 문자,숫자,표현식 비교칼럼명(JOIN 사용시) - 순

#### 연산자 종류
  - **비교**
    - =
    - \> 
    - \>= 
    - < 
    - <=
  - **SQL**
    - BETWEEN a AND b
    - IN (list)
    - LIKE '비교문자열'
    - IS NULL
  - **논리**
    - AND
    - OR
    - NOT
  - **부정 비교**
    - !=
    - ^=
    - \<>
    - NOT=
  - **부정 SQL**
    - NOT BETWEEN a AND b
    - NOT IN (list)
    - IS NOT NULL

#### 참고
  - 오라클에 '' 입력하면 NULL로 입력되어 조회하려면 IS NULL 조건으로 조회해야 함
  - SQL에서는 ''로 저장 및 조회 가능

#### 조건
  - 검색 CASE 표현식 : 개별 조건 확인하고 반환
  - 단순 CASE 표현식 : 표현식 값 기준, 여러 조건을 확인
  - DECODE : 여러 조건 비교하고 일치하는 조건의 결과 반환

<br>
<br>

### GROUP BY, HAVING 절
  - 일반적으로 집계 함수는 GROUP BY 절과 같이 사용되지만 테이블 전체가 하나의 그룹이 되는 경우에는 GROUP BY 절 없이 단독으로 사용 가능함
  - GROUP BY 절을 통해 소그룹 별 기준을 정한 후, SELECT 절에 집계 함수를 사용함
  - 집계 함수의 통계 정보는 NULL을 제외하고 수행됨
  - SELECT 절과 달리 ALIAS 사용 불가
  - HAVING 절은 GROUP BY 절의 기준 항목이나 소그룹의 집계 함수를 사용한 조건을 표시함
  - GROUP BY 절에 의한 소그룹 별로 만들어진 집계 데이터 중, HAVING 절에서 제한 조건을 두어 만족하는 내용만 출력함
  - HAVING 절은 일반적으로 GROUP BY 절 뒤에 위치하지만 GROUP BY 가 없어도 사용 가능

#### 집계 함수
  - **WHERE 절에는 집계함수 사용 불가**
  - COUNT(*)
  - COUNT
  - SUM
  - AVG
  - MAX
  - MIN
  - STDDEV (표준 편차)
  - CARIANCE/VAR (재무 함수)
  
<br>
<br>

### ORDER BY 절
  - 기본적인 정렬 순서는 ASC
  - **오라클에서 NULL은 최댓값, SQL에서는 최솟값**
  - SELECT 절에서 오직 한 개만 사용 가능

<br>
<br>

### 조인
  - 두 개의 테이블을 서로 묶어서 하나의 결과를 리턴
  - 일반적으로 조인은 PK와 FK의 연관성에 의해 성립됨 (특정 경우 논리적인 값들의 연관만으로도 가능)
  - DBMS 옵티마이저는 FROM 절에 나열된 데이터 들을 항상 2개로 묶어서 처리함
  - **EQUI JOIN** : 두 테이블에서 공통적으로 존재하는 컬럼의 값이 일치하는 행을 연결해서 결과를 리턴
  - **NON-EQUI JOIN** : JOIN 조건을 '=' 연산자 이외의 비교 연산자를 사용하는 것
  - EQUI JOIN 은 조인에 관여하는 테이블 들의 값이 정확하게 일치할 때 '=' 이 사용됨, 이외의 경우는 NON-EQUI JOIN임

#### 표준 조인
  - INNER JOIN : 디폴트, 쉼표 혹은 조건절로 수행, 두 테이블 모두 지정한 열의 데이터 리턴
  - NATURAL JOIN : 오라클, 두 테이블의 동일한 컬럼명을 갖는 컬럼 조인
  - USING 조건절 / ON 조건절 : 오라클, 자연 조인에서 동일한 컬럼명이 둘 이상인 경우 사용
  - CROSS JOIN : 카타시안 조합, 두 테이블의 모든 행을 조인
  - OUTER JOIN : 두 테이블 중 한 쪽만 있어도 리턴

![](/assets/img/Certificates/SQLD/outer_join.png)

출처 : [혼공](https://hongong.hanbit.co.kr/sql-%ea%b8%b0%eb%b3%b8-%eb%ac%b8%eb%b2%95-joininner-outer-cross-self-join/)

<br>
<br>
<br>

## SQL 활용

### 서브 쿼리
  - 다른 쿼리 내부에 포함된 SELECT 문
  - **연관 서브쿼리** : 서브 쿼리가 메인 쿼리 컬럼을 가짐
  - **비연관 서브쿼리** : 메인 쿼리에 값을 제공하기 위한 목적으로 사용
  - **단일 행 서브쿼리** : 실행 결과가 항상 1건 이하, 단일 행 비교 연산자(=, <, <=, >, =>, <>)와 사용
  - **다중 행 서브쿼리** : 실행 결과가 여러 건인 서브쿼리, 다중 행 비교 연산자와 함께 사용
    - IN : 결과에 값이 포함되는 지 확인 (OR 조건)
    - ANY : 결과 중 하나라도 조건을 만족하는 지 확인
    - ALL : 모든 값이 조건을 만족하는 지 확인
    - EXISTS : 결과가 존재하는 지 확인
  - **다중 컬럼 서브쿼리** : 여러 컬럼 반환, 메인 쿼리 조건절에 따라 여러 컬럼 동시 비교 가능
  - **스칼라 서브쿼리** : SELECT 절에서 사용, 한 행, 한 컬럼만 리턴하는 서브쿼리
  - **인라인 뷰** (동적 뷰) : FROM 절에서 사용, 서브쿼리를 임시 테이블 처럼 사용
    - 뷰 사용 장점
      - 독립성 : 테이블 구조가 변경되어도 뷰를 사용하는 응용 프로그램을 변경하지 않아도 됨
      - 편리성 : 복잡한 질의를 단순하게 작성 가능
      - 보안성 : 숨기고 싶은 정보를 제외하고 구성 가능
  - 서브쿼리는 HAVING, ORDER BY 절 등에서도 사용 가능

<br>
<br>

### 집합 연산자
  - UNION : 중복 제거
  - UNION ALL : 결과 전부 합침
  - INTERSECT : 중복 제거 교집합 리턴
  - EXCEPT : 중복 제거 차집합 리턴

<br>
<br>

### 그룹 함수
  - NULL 빼고 집계됨, 결과값 없는 행 출력 안함
  - 표현식
    - ROLL UP(1, 2) : 1과 2 별 소계, 1별 소계, 총 합계 리턴 (계층 구조, 순서 바뀌면 결과 값 바뀜)
    - CUBE(1, 2) : 1과 2 별 소계, 1 별 소계, 2 별 소계, 총 합계 리턴 (순서 무관)
    - GROUPING SETS(1, 2) : 1별  소계, 2 별 소계 (순서 무관)

```
SELECT 부서, 직급, SUM(급여)
FROM 직원
GROUP BY ROLLUP(부서, 직급);
-> 각 부서와 직급별 급여의 합계를 보여주고, 각 부서의 총 급여와 전체 급여의 합계도 포함

SELECT 부서, 직급, SUM(급여)
FROM 직원
GROUP BY CUBE(부서, 직급);
-> 각 부서와 직급별 급여의 합계를 보여주고, 각 부서, 각 직급별 총 급여 및 전체 총 급여도 포함

SELECT 부서, 직급, SUM(급여)
FROM 직원
GROUP BY GROUPING SETS (
    (부서, 직급),
    (부서),
    ()
);
-> 각 부서와 직급별 급여 합계, 각 부서의 총 급여, 전체 총 급여를 포함
```

<br>
<br>

### 윈도우 함수
  - 행과 행 간의 관계를 정의
  - 순위, 합계, 평균, 행 위치 등 조작
  - 결과에 대한 처리로, 결과 건수에 영향을 미치지 않음

#### 윈도우 함수 종류
  - 순위
    - ROW_NUMBER / RANK / DENSE_RANK
  - 집계
    - SUM / AVG / MAX / MIN / COUNT
  - 행 순서
    - FIRST_VALUE / LAST_VALUE / LAG / LEAD
  - 비율
    - PERCENT_RANK (행 순서별 백분율) / CUME_DIST (건수 누적 백분율) / NTILE (전체 건수를 주어진 인자로 N 등분)

#### 윈도잉 절
  - SELECT 윈도우함수 (A) OVER (PARTITION BY 컬럼 ORDER BY 컬럼 윈도잉절) FROM 테이블
  - BETWEEN a AND b : 프레임 범위 지정
  - UNBOUNDED PRECEDING/FOLLOWING : 프레임 시작/끝을 현재 윈도우 그룹의 처음/마지막 행으로 설정
  - N PRECEDING/FOLLOWING : 현재 행을 기준으로 지정된 수 만큼 이전/이후의 행을 리턴
  - CURRENT ROW : 현재 행을 기준으로 윈도우 프레임 설정

<br>
<br>

### Top N 쿼리
  - 조건에 맞는 최상위 데이터를 N개 혹은 최하위 데이터를 N개 조회하는 쿼리
  - ORDER BY : 데이터 정렬
  - LIMIT : 정렬된 결과에서 상위 N개 행 선택
  - FETCH : 결과 집합에서 상위 N개 행 선택
  - TOP(n) WITH TIES : 값이 동일한 경우 함께 출력

<br>
<br>

### 계층형 질의와 셀프 조인

#### 계층형 질의
  - SQL 에서 테이블 내에 계층 구조를 표현할 때 사용하는 기법
  - 함수
    - CONNECT BY : 트리 형태의 구조로 쿼리 수행
    - START WITH : 계층 구조 전개의 시작 위치(최상위 행) 지정
    - CONNECT_BY ROOT/ISLEAF : 최상위/하위 계층 값 으로 쿼리 수행
    - SYS_CONNECT_BY_PATH : 계층 구조로 형성된 데이터 조회
    - ORDER BY SIBLINGS BY : 형제 노드 사이에서 정렬
  - SQL Server에서 계층형 질의문은 CTE(Common Table EXPRESSION)를 재귀호출 함으로써 계층 구조를 전개함
    - CTE : 공통 테이블 식, SELECT 문을 미리 정의해 이름을 붙인 후, 이어지는 쿼리에서 테이블처럼 사용하는 기능
  - 앵커 멤버를 실행하여 기본 결과의 집합을 만들고 이후 재귀 멤버를 지속적으로 실행함
    - 앵커 멤버 : 재귀 CTE의 첫 번째 호출, 집합 연산자로 연결된 하나 이상 쿼리 정의로 구성, 다른 CTE를 참조하지 않는 경우 앵커 멤버로 간주
  - 오라클의 계층형 질의문에서 WHERE 절은 모든 전개를 진행한 후 필터 조건으로서 조건을 만족하는 데이터만 추출하는 데 활용됨
  - PRIOR 키워드는 CONNECT BY 절 뿐만 아니라 SELECT, WHERE 절에서도 사용 가능

#### 셀프 조인
  - 동일 테이블 사이의 조인
  - 식별을 위해 반드시 테이블 ALIAS 사용

```
셀프 조인 예시

SELECT ALIAS명1.컬럼명, ALIAS명2.컬럼명 ... 
FROM 테이블 ALIAS명1, 테이블 ALIAS명2
WHERE ALIAS명1.컬럼명2 = ALIAS명2.컬럼명1;
```

<br>
<br>
<br>

## 관리 구문
  - 제약 조건 종류 : PRIMARY KEY / UNIQUE KEY (NULL 허용, 주식별자 중 하나) / NOT NULL / CHECK / FOREIGN KEY

```
기본키 할당

ALTER TABLE 테이블명 ADD CONSTRAINT
constraint_name PRIMARY KEY (컬럼명1, 컬럼명2)
```

#### DELETE(MODIFY) ACTION
  - Cascade : 상위 삭제 시 자식 동시 삭제
  - SET NULL / SET Default : NULL 값 처리 / 기본 값 처리
  - Restrict : 자식 테이블에 PK 값 없는 경우에만 상위 삭제 가능
  - No Action : 참조무결성 위반하는 삭제나 수정 액션 불가

#### INSERT ACTION
  - Automatic : Master PK 자동 생성 후 Child 입력
  - SET NULL / Default : PK 없으면 NULL 값 처리 / 기본값 처리
  - Dependent : Master 테이블에 PK가 존재할 때만 Child 입력 허용
  - No Action : 참조무결성 위반 액션 불가

<br>
<br>

### DML
  - SELECT / INSERT / DELETE / UPDATE / MERGE

```
[삽입]
INSERT INTO 테이블명 컬럼명 VALUES(리스트);

[수정]
UPDATE 테이블명 SET 컬럼명=데이터 WHERE 조건;
UPDATE 테이블명 SET (컬럼명=데이터, 컬럼명=데이터, 컬럼명=데이터) WHERE 조건;

[삭제]
DELETE FROM 테이블명 WHERE 조건;

[병합]
MERGE INTO 타겟테이블명
USING 비교테이블명
ON 조건;
-> 조건에 따라 타겟을 비교와 병합
```

<br>
<br>

### DDL
  - CREATE / ALTER / DROP / TRUNCATE / RENAME

```
ALTER TABLE 테이블명 ADD 컬럼명 데이터타입 {DEFAULT} {제약조건};
ALTER TABLE 테이블명 ADD CONSTRAINT 제약속성명 제약조건 {컬럼명};

[컬럼 수정 ORACLE = MODIFY]
ALTER TABLE 테이블명 MODIFY 컬럼명 데이터타입 {DEFAULT} {제약조건};
-> 동시 여러 개 불가

[컬럼 수정 SQL = ALTER COLUMN]
ALTER TABLE 테이블명 ALTER COLUMN 컬럼명 데이터타입 {DEFAULT} {제약조건};
-> 동시 여러 개 불가

[테이블 삭제]
DROP TABLE 테이블명;
DROP TABLE 테이블명 CASCADE CONSTRAINT;
-> 해당 테이블에 FK가 존재해서 다른 엔티티를 참조 중이면 DROP 불가능
-> CASCADE CONSTRAINT = 참조 제약조건을 모두 삭제하면서 동시에 삭제

[컬럼 삭제]
ALTER TABLE 테이블명 DROP COLUMN 컬럼명;

[테이블 값 초기화]
TRUNCATE TABLE 테이블명;

[테이블 이름 변경]
RENAME TABLE 변경전이름 TO 변경후이름;

[컬럼 이름 변경]
ALTER TABLE 테이블명 RENAME COLUMN 기존컬럼명 TO 변경컬럼명;
```

<br>
<br>

### DCL
  - GRANT / REVOKE

```
[권한 추가]
GRANT CREATE TABLE TO 사용자명;
GRANT SELETE ON 테이블명 TO 사용자명;
GRANT INSERT ON 테이블명 TO 사용자명;
-> 모든 권한일 때 = ALL

[권한 취소]
REVOKE 권한명 FROM 사용자명;

[ROLE 권한 부여]
CREATE ROLE 역할명;
GRANT CREATE SESSION, CREATE USER, CREATE TABLE TO 역할명;
GRANT 역할명 TO 사용자명;

```

<br>
<br>

### TCL
  - COMMIT / ROLLBACK
  - 트랜잭션 특성 ACID
    - 원자성 : 모두 실행 OR 다 안됨
    - 일관성 : 이전 이후 데이터 동일
    - 고립성 : 연산 중 영향 X
    - 지속성 : 성공 시 영구 저장

```
CREATE TABLE SAMPLE1 (COL1 NUMBER, COL2 NUMBER);
INSERT INTO SAMPLE1 VALUES(10,10);
CREATE TABLE SAMPLE2 (COL1 NUMBER, COL2 NUMBER);
INSERT INTO SAMPLE2 VALUES(10,30);
ROLLBACK; --> 여기서 롤백 발동
INSERT INTO SAMPLE2 VALUES(20,40);
COMMIT;

-> 이 경우 CREATE 는 DDL, AUTO 커밋이 이루어져서 저장되고
별도의 ROLLBACK 포인트 없기 때문에 SAMPLE2 테이블 만든 뒤로 이동 후
SAMPLE2 테이블에 10, 30 삽입만 롤백됨

-> 결과
SAMPLE1 = (10, 10)
SAMPLE2 = (20, 40)
```

<br>
<br>

### DB 키 종류
  - 기본키 : 엔티티 대표 키 (NULL 불가)
  - 후보키 : 유일성과 최소성 만족
  - 슈퍼키 : 유일성 만족
  - 대체키 : 기본키 제외 나머지
  - 외래키 : 여러 테이블의 기본 키 필드, 참조 무결성 확인 위해 사용 (NULL 가능)
  - 고유키 : 고유한 값 보장 (NULL 1개만 가능)

### 연산자 우선 순위
  - 괄호 - NOT - 비교 연산자 및 SQL 연산자 - AND - OR

### DELETE / DROP / TRUNCATE 차이
  - DELETE
    - 데이터 일부 또는 전체 삭제
    - 롤백 가능
    - UNDO 데이터 생성 -> 느림
  - DROP - AUTO 커밋
    - 데이터와 구조를 동시 삭제
    - 즉시 반영
    - 롤백 불가능
  - TRUNCATE - AUTO 커밋
    - 데이터만 초기화, 구조는 그대로
    - 즉시 반영
    - 롤백 불가능
    - UNDO 데이터 생성 X -> 빠름
