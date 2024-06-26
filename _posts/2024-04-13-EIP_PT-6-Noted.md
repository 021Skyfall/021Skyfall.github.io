---
title: "정보처리기사 실기 / Vii - SQL 응용 ☆☆"
author: Jeremiah Lee
date: 2024-04-13
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

# C1. 데이터베이스 기본

<br>

## i. 트랜잭션

### A. 트랜잭션(Transaction)

#### 1. 트랜잭션의 개념
  - 인가받지 않은 사용자로부터 데이터를 보장하기 위해 DBMS가 가져야 하는 특성
  - 데이터베이스 시스템에서 하나의 논리적 기능을 정상적으로 수행하기 위한 작업의 기본 단위

<br>

#### 2. 트랜잭션의 특성
  - **원자성(Atomicity)**
    - 트랜잭션 연산이 모두 정상 성공하거나 모두 취소되어야함
    - All or Nothing
  - **일관성(Consistency)**
    - 트랜잭션 수행 전과 트랜잭션 수행 후의 상태가 같아야함
  - **고립성(Isolation)**
    - 동시에 실행되는 트랜잭션들이 서로 영향을 미치지 않아야함
  - **영속성(Durability)**
    - 성공 한 트랜잭션의 결과는 영구적으로 DB에 저장되어야함

<br>

#### 3. 트랜잭션의 상태 변화
  - **활동 상태(Active)** : 초기 상태, 트랜잭션이 실행 중
  - **부분 완료 상태(Partially Committed)** : 마지막 명령문이 실행된 후
  - **완료 상태(Committed)** : 트랜잭션이 성공적으로 완료된 후
  - **실패 상태(Failed)** : 정상적인 실행이 더 이상 진행될 수 없을 때
  - **철회 상태(Aborted)** : 트랜잭션이 취소되고 데이터베이스가 트랜잭션 시작 전 상태로 환원

<br>

#### 4. 트랜잭션 제어(TCL; Transaction Control Language)
  - **커밋(Commit)** : 트랜잭션 확정
  - **롤백(Rollback)** : 트랜잭션 취소
  - **체크포인트(Checkpoint)** : 저장 시기 설정

<br>

#### 5. 병행 제어(Consistency 주요 기법)
  - 다수 사용자 환경에서 여러 트랜잭션을 수행할 때, 데이터베이스 일관성 유지를 위해 상호 작용을 제어하는 기법
  - **병행 제어 미보장 시 문제점**
    - **갱신 손실** : 먼저 실행된 트랜잭션의 결과를 나중에 실행된 트랜잭션이 덮어쓸 때 발생하는 오류
    - **현황 파악오류** : 트랜잭션의 중간 수행 결과를 다른 트랜잭션이 참조하여 발생하는 오류
    - **모순성** : 두 트랜잭션이 동시에 실행되어 데이터베이스의 일관성이 결여되는 오류
    - **연쇄복귀** : 복수의 트랜잭션이 데이터 공유 시 특정 트랜잭션이 처리를 취소할 경우 트랜잭션이 처리한 곳의 부분을 취소하지 못하는 오류
  - **병행 제어 기법 종류**
    - **로킹(Locking)** : 하나의 트랜잭션을 실행하는 동안 특정 데이터 항목에 대해 다른 트랜잭션이 동시에 접근하지 못하도록 **상호배제(Mutual Exclusion)** 기능을 제공 (순차적 진행을 보장하는 직렬화 기법)
    - **낙관적 검증(Optimistic Validation)** : 트랜잭션이 어떠한 검증도 수행하지 않고 일단 수행하고, 종료 시 검증을 수행하여 데이터베이스에 반영
    - **타임 스탬프 순서(Time Stamp Ordering)** : 트랜잭션 실행 시작 전에 타임 스탬프를 부여하여 시간에 따라 작업을 수행
    - **다중버전 동시성 제어(MVCC; Multi Version Concurrency Control)** : 트랜잭션의 타임스탬프와 접근하려는 데이터의 타임스탬프를 비교하여 직렬가능성이 보장되는 적절한 버전을 선택하여 접근하도록 함

<br>

#### 6. 데이터베이스 고립화 수준(Isolation 주요 기법)
  - 다른 트랜잭션이 현재의 데이터에 대한 무결성을 해치지 않기 위해 잠금을 설정하는 정도
  - **Read Uncommitted**
    - 한 트랜잭션에서 연산 중인 데이터를 다른 트랜잭션이 읽는 것을 허용
  - **Read Committed**
    - 한 트랜잭션에서 연산을 수행할 때 연산이 완료될 때까지 연산 대상 데이터에 대한 읽기 제한
  - **Repeatable Read**
    - 선행 트랜잭션이 특정 데이터를 읽을 때, 트랜잭션 종료 시까지 해당 데이터에 대한 갱신/삭제 제한
  - **Serializable Read**
    - 선행 트랜잭션이 특정 데이터 영역을 순차적으로 읽을 때, 해당 데이터 영역 전체에 대한 접근 제한

<br>

#### 7. 회복 기법(Durability 주요 기법)
  - 트랜잭션을 수행하는 도중 장애로 인해 손상된 데이터베이스를 손상되기 이전의 상태로 복구시키는 작업
  - **REDO** : DB가 비정상적으로 종료되었을 때 로그를 분석 후 시작, 완료 기록이 있는 트랜잭션들의 작업을 재작업함
  - **UNDO** : DB가 비정상적으로 종료되었을 때 로그를 분석 후 시작은 있지만 완료 기록이 없는 트랜잭션들의 변경 내용을 모두 취소함
  - **회복 기법 종류**
    - **로그 기반 회복 기법**
      - **지연 갱신 회복 기법(Deferred Update)** : 트랜잭션이 완료되기 전까지 DB에 기록하지 않음
      - **즉각 갱신 회복 기법(Immediate Update)** : 트랜잭션 수행 중 갱신 결과를 바로 DB에 반영
    - **체크 포인트 회복 기법(Checkpoint Recovery)** : 장애 발생 시 검사점 이후에 처리된 트랜잭션에 대해서만 장애 발생 이전의 상태로 복원
    - **그림자 페이징 회복 기법(Shadow Paging Recovery)** : 트랜잭션 수행 시 복제본을 생성하여 DB 장애 시 이를 이용해 복구

<br>
<br>

### B. 데이터 정의어(DDL; Data Define Language)
  - 테이블 같은 데이터 구조를 정의하는 데 사용되는 명령어
  - 생성, 변경, 삭제, 명칭 변경

#### 1. DDL의 대상
  - **도메인** : 하나의 속성이 가질 수 있는 원자값들의 집합
  - **스키마**
    - 데이터베이스의 구조, 제약조건 등의 정보를 담고 있는 기본적인 구조
    - **외부 스키마(External Schema)**
      - 서브 스키마, 사용자나 개발자의 관점에서 필요로 하는 데이터베이스의 논리 구조
    - **개념 스키마(Conceptual Schema)**
      - 데이터베이스의 전체적인 논리 구조
    - **내부 스키마(Internal Schema)**
      - 물리적 저장 장치의 관점에서 보는 데이터베이스 구조
  - **테이블** 
    - 릴레이션 혹은 엔티티
    - 데이터를 저장하는 항목인 필드들로 구성된 데이터의 집합체
    - 하나의 DB 내에 여러 개 가능
  - **뷰** 
    - 하나 이상의 물리 테이블에서 유도되는 가상 테이블, 조인 없이 단순 질의 가능
    - **특징**
      - **논리적 데이터 독립성 제공** : DB에 영향 없이 원하는 형태로 데이터 접근 가능
      - **데이터 조작 연산 간소화** : 원하는 형태의 논리적 구조를 형성 후 조작 연산 간소화
      - **보안 기능 제공** : 특정 필드를 선택해 뷰를 생성할 경우 이외의 필드 조회 및 접근 불가
      - **뷰 변경 불가** : ALTER 문 사용 불가
    - CREATE나 DROP 명령만 사용 가능 / INDEX 생성 불가
  - **인덱스**
    - 검색 연산의 최적화를 위해 DB 내 값에 대한 주소 정보로 구성된 데이터 구조
    - <키 값, 주소> 형태의 자료구조
    - **인덱스 종류**
      - **순서 인덱스** : B-Tree 알고리즘 활용, 정렬된 순서로 생성됨
      - **해시 인덱스** : 해시 함수에 의해 직접 데이터에 키 값으로 접근
      - **비트맵 인덱스** : 각 컬럼에 적은 개수 값이 저장된 경우 선택
      - **함수기반 인덱스** : 수식이나 함수를 적용하여 만듬
      - **단일 인덱스** : 하나의 컬럼으로만 구성
      - **결합 인덱스** : 두 개 이상의 컬럼으로 구성
      - **클러스터드 인덱스** : 기본 키 기준으로 레코드를 묶어서 저장

<br>

#### 2. 데이터베이스 파일 구조
  - **순차 방법** : 레코드들의 물리적 순서가 논리적 순서와 같게 순차적으로 저장
  - **인덱스 방법** : 인덱스가 가리키는 주소를 따라 원하는 레코드에 접근
  - **해싱 방법** : 키값을 해시 함수에 대입시켜 계산 결과를 주소로 사용

<br>

#### 3. DDL 명령어
  - **CRATE** : 생성
  - **ALTER** : 수정
  - **DROP** : 테이블 삭제
  - **TRUNCATE** : 내용 삭제

<br>

#### 4. TABLE 관련 DDL

##### a. **CREATE TABLE**
  - 테이블 생성

```SQL
// 기본
CREATE TABLE 테이블명
(
  컬럼명 데이터 타입 [제약 조건],
  ...
);

// 예시
CREATE TABLE 테이블명
(
  컬럼명 데이터타입 PRIMARY KEY,
  컬럼명 데이터타입 FOREIGN KEY REFERENCES 참조테이블(기본키),
  컬럼명 데이터타입 UNIQUE,
  컬럼명 데이터타입 NOT NULL,
  컬럼명 데이터타입 CHECK(조건식),
  컬럼명 데이터타입 DEFAULT 값
);
```

  - **CRATE TABLE 제약 조건**
    - **PRIMARY KEY** : 유일하게 테이블의 각 행을 식별하는 기본 키 정의
    - **FOREIGN KEY** : 참조 대상을 테이블로 명시하여 외래 키 정의
    - **UNIQUE** : 테이블 내에서 얻은 유일한 값을 갖도록 함
    - **NOT NULL** : NULL 비허용
    - **CHECK** : 개발자가 정의하는 제약 조건
    - **DEFAULT** : INSERT 시 해당 컬럼 값을 넣지 않는 경우 기본값

##### b. **ALTER TABLE**
  - 테이블 컬럼 수정

```SQL
1. 컬럼 추가 시
ALTER TABLE 테이블명 ADD 컬럼명 데이터타입 제약조건;

2. 컬럼 수정 시
ALTER TABLE 테이블명 MODIFY 컬럼명 데이터타입 제약조건;

3. 컬럼 삭제 시
ALTER TABLE 테이블명 DROP 컬럼명 데이터타입 제약조건;
```

##### c. **DROP TABLE**
  - 테이블 삭제

```SQL
DROP TABLE 테이블명 [CASCADE | RESTRICT];
```

  - CASCADE와 RESTRICT는 외래 키 관련 옵션
  - **CASCADE** : 참조하는 테이블까지 연쇄 삭제
  - **RESTRICT** : 삭제할 테이블을 참조 중이면 제거하지 않음

##### d. **TRUNCATE TABLE**
  - 테이블 내의 데이터 삭제

```SQL
TRUNCATE TABLE 테이블명;
```

<br>

#### 5. VIEW 관련 DDL

##### a. **CREATE VIEW**
  - 뷰 생성

```SQL
CREATE VIEW 뷰이름 AS 
조회쿼리;
```

  - 이 때 조회쿼리에서 UNION 이나 ORDER BY 절을 사용할 수 없음

##### b. **CREATE OR REPLACE VIEW**
  - 뷰 교체

```SQL
CREATE OR REPLACE VIEW 뷰이름 AS
조회쿼리;
```

##### c. **DROP VIEW**
  - 뷰 삭제

```SQL
DROP VIEW 뷰이름;
```

<br>

#### 6. INDEX 관련 DDL

##### a. **CREATE INDEX**
  - 인덱스 생성

```SQL
CREATE UNIQUE INDEX 인덱스명 ON 테이블명(컬럼명1, 컬럼명2 ...);
```

  - 컬럼에 중복 값 허용하지 않는 UNIQUE는 생략 가능

##### b. **ALTER INDEX**
  - 인덱스 수정

```SQL
ALTER [UNIQUE] INDEX 인덱스명 ON 테이블명(컬럼명1, 컬럼명2, ...);
```

  - UNIQUE 생략 가능

##### c. **DROP INDEX**
  - 인덱스 삭제

```SQL
DROP INDEX 인덱스명;
```

<br>
<br>

### C. DML(Data Manipulation Language)
  - 데이터베이스에 저장된 자료들을 입력, 수정, 삭제, 조회하는 언어

#### 1. DML 명령어
  - **SELECT** 컬럼명 FROM 테이블명 : 조회
  - **INSERT** INTO 테이블명 VALUES(값) : 삽입
  - **UPDATE** 테이블명 SET 컬럼명='' : 수정
  - **DELETE** FROM 테이블명 : 삭제

<br>

#### 2. SELECT 명령어
  - 데이터 내용 조회

```SQL
SELECT [ALL | DISTINCT] 속성명1, 속성명2 ...
  FROM 테이블명1, ...
[WHERE 조건]
[GROUP BY 속성명1, ...]
[HAVING 그룹조건]
[ORDER BY 속성 [ASC | DESC]];
```

##### a. WHERE 절
  - **비교**
    - **=** : 값이 같은 경우
    - **<>**, **!=** : 값이 다른 경우
    - **<**, **<=**, **>=**, **>** : 비교 연산
  - **범위**
    - 컬럼 **BETWEEN** 값1 AND 값2 : 값1 보다 크거나 같고, 값2 보다 작거나 같은 값 조회
  - **집합**
    - 컬럼 **IN** (값1, 값2, ...) : 컬럼이 IN 안에 포함된 경우
    - 컬럼 **NOT IN** (값1, 값2, ...) : 컬럼이 IN 안에 포함되어 있지 않은 경우
  - **패턴**
    - 컬럼 **LIKE** 패턴 : 컬럼이 패턴에 포함된 경우
      - **패턴**
        - **%** : 0개 이상의 문자열과 일치 ('정보%' → "정보"로 시작하는 문자열)
        - **[ ]** : 1개의 문자와 일치 ('[AB]%' → "A"나 "B"로 시작하는 문자열)
        - **[^]** : 1개의 문자와 불일치
        - **_** : 특정 위치의 1개의 문자와 일치
  - **NULL**
    - 컬럼 **IS NULL** : 컬럼이 NULL인 경우
    - 컬럼 **IS NOT NULL** : 컬럼이 NULL이 아닌 경우
  - **복합조건**
    - 조건1 **AND** 조건2
    - 조건1 **OR** 조건2
    - **NOT** 조건 : 조건에 해당하지 않는 경우

##### b. GROUP BY 절
  - 속성값을 그룹으로 분류

##### c. HAVING 절
  - GROUP BY에 의해 분류한 후 그룹에 대한 조건 지정
  - GROUP BY의 WHERE 절

##### d. ORDER BY 절
  - 속성값을 정렬

##### e. JOIN
  - 두 개 이상의 테이블을 연결하여 데이터 검색
  - INNER, OUTER 생략 가능
  - **내부 조인(INNER JOIN)** 
    - 공통 존재의 컬럼의 값이 같은 경우 추출 **(≈교집합)**
  - **외부 조인(OUTER JOIN)**
    - **왼쪽 외부 조인(LEFT OUTER JOIN)** : 왼쪽 테이블의 모든 데이터와 오른쪽 테이블의 동일 데이터 추출
    - **오른쪽 외부 조인(RIGHT OUTER JOIN)** : 오른쪽 테이블의 모든 데이터와 왼쪽 테이블의 동일 데이터 추출
    - **완전 외부 조인(FULL OUTER JOIN)** : 양쪽 모든 데이터 추출 **(≈합집합)**
  - **교차 조인(CROSS JOIN)** : 조인 조건이 없는 모든 데이터 조합 추출
  - **셀프 조인(SELF JOIN)** : 자기 자신에게 별칭을 지정한 후 다시 조인

```SQL
SELECT A.컬럼1, A.컬럼2, ...,
        B.컬럼1, B.컬럼2, ...
FROM 테이블1 A JOIN 테이블2 B
ON 조인조건
[WHERE 검색조건];
```

##### f. 서브쿼리
  - SQL 문 안에 포함된 또 다른 SQL 문
  - **FROM 절 서브쿼리**
    - 인라인 뷰
    - FROM 절 안
  - **WHERE 절 서브쿼리**
    - 중첩 서브쿼리
    - WHERE 절 안

```SQL
// 인라인 뷰 예시
SELECT 컬럼명
FROM 테이블명 
(SELECT 컬럼명 FROM 테이블명 [WHERE 조건])
[WHERE 조건]

// 중첩 서브쿼리 예시
SELECT 컬럼명
FROM 테이블명
WHERE 컬럼명 IN 
(SELECT 컬럼명 FROM 테이블명 [WHERE 조건])
```

##### g. 집합 연산자
  - 테이블을 집합으로 보고 두 테이블 연산에 집합 연산자를 사용
  - **UNION** : 중복 행이 제거된 쿼리 결과 반환 (≈중복 제거 합집합)
  - **UNION ALL** : 중복 행 포함된 쿼리 결과 반환 (≈중복 포함 합집합)
  - **INTERSECT** : 두 쿼리 결과에 공통 존재 결과 반환 (≈교집합)
  - **MINUS** : 첫 쿼리에 있고 두 번째 쿼리에 없는 결과 반환

```SQL
// 집합 연산자 예시
SELECT 컬럼명
FROM 테이블명
WHERE 조건
UNION | UNION ALL | INTERSECT | MINUS
SELECT 컬럼명
FROM 테이블명
WHERE 조건;
```

<br>

#### 3. INSERT 명령어
  - 데이터 내용 삽입

```SQL
INSERT INTO 테이블명(속성명1, ...)
VALUES(데이터1, ...);

// 예시
INSERT INTO 학생(학번, 성명)
VALUES (6655, '홍길동');
```

<br>

#### 4. UPDATE 명령어
  - 데이터 내용 변경

```SQL
UPDATE 테이블명
SET 속성명 = 데이터, ...
WHERE 조건;

// 예시
UPDATE 학생
SET 주소 = '서울'
WHERE 이름 = '홍길동';
```

<br>

#### 5. DELETE 명령어
  - 데이터 내용 삭제

```SQL
DELETE FROM 테이블명
WHERE 조건;
```

<br>
<br>

### D. DCL(Data Control Language)
  - 보안, 무결성 유지, 병행 제어, 회복을 위한 관리자 제어용 언어
  - **GRANT** : 사용자 권한 부여
  - **REVOKE** : 사용자 권한 취소

```SQL
// GRANT 문법
GRANT 권한 ON 테이블 TO 사용자명;

// 예시
GRANT UPDATE ON 학생 TO 홍길동;

// REVOKE 문법
REVOKE 권한 ON 테이블 FROM 사용자명;

// 예시
REVOKE UPDATE ON 학생 FROM 홍길동;
```

***

<br>
<br>
<br>
<br>

# C2. 응용 SQL 작성하기

<br>

## i. 집계성 SQL 작성

### A. 데이터 분석 함수의 개념
  - 총합, 평균 등의 데이터 분석을 위해서 복수 행 기준의 데이터를 모아 처리하는 것을 목적으로 하는 다중 행 함수
  - 복수 행을 그룹별로 모아서 그룹당 단일 계산 결과를 반환
  - 비교연산자(<, >, =, <>) 사용 가능
  - **다중 행 연산자**
    - 서브 쿼리의 결과가 여러 개의 튜플을 반환하는 다중 행 서브쿼리에서 사용되는 연산자
    - **IN** : 리턴되는 값 중 조건에 해당하는 값이 있으면 참 반환
    - **ANY** : 서브쿼리에 의해 리턴되는 각 값과 조건을 비교 후 하나 이상 만족하면 참 반환
    - **ALL** : 서브쿼리에 의해 리턴되는 모든 값과 조건 값을 비교하여 모든 값을 만족하면 참 반환
    - **EXIST** : 메인 쿼리의 비교 조건이 서브쿼리의 결과 중에서 만족하는 값이 하나라도 존재하면 참 반환

```SQL
// IN 예시
SELECT 컬럼명1, 컬럼명2, ...
FROM 테이블명
WHERE 컬럼명1
IN (SELECT 컬럼명 FROM 테이블명);
// 딱 집어서 조건 만족 시 리턴

// ANY 예시
SELECT 컬럼명1, 컬럼명2, ...
FROM 테이블명
WHERE 컬럼명1 > ANY (SELECT 컬럼명 FROM 테이블명 WHERE 조건);
// 조건 행 중 하나라도 만족하면 리턴

// ALL 예시
SELECT 컬럼명1, 컬럼명2, ...
FROM 테이블명
WHERE 컬럼명 < ALL (SELECT 컬럼명 FROM 테이블명 WHERE 조건);
// 조건 행을 모두 만족해야 리턴

// EXISTS 예시
SELECT 컬럼명1, 컬럼명2, ...
FROM 테이블명
WHERE EXISTS (SELECT 1 FROM 테이블 FROM 조건);
// 여기서 1은 TRUE 값을 반환해야한다는 의미로 관례적인 표현
// EXISTS 연산자는 TRUE냐 FALSE냐가 중요하지 리턴되는 값은 중요치 않음
```

<br>
<br>

### B. 데이터 분석 함수의 종류
  - 데이터 튜플 간의 상호 연관 및 계산 분석을 위한 함수
  - **집계 함수** : 여러 행 또는 테이블 전체 행으로부터 하나의 결괏값을 반환하는 함수
  - **그룹 함수** : 소그룹 간의 소계 및 중계 등의 중간 합계 분석 데이터를 산출하는 함수
  - **윈도 함수** : 데이터베이스를 사용한 온라인 분석 처리 용도로 사용하기 위해서 표준 SQL에 추가된 기능

<br>
<br>

### C. 집계 함수
  - 여러 행 또는 테이블 전체 행으로부터 하나의 결괏값을 반환하는 함수
  - NULL 값 제외 후 산출

#### 1. 집계 함수 구문

```SQL
SELECT 컬럼1, 컬럼2, ..., 집계함수
FROM 테이블명
[WHERE 조건]
GROUP BY 컬럼1, 컬럼2, ...
[HAVING 조건식(집계함수 포함)]
```

<br>

#### 2. 집계 함수 종류
  - **COUNT** : 개수 반환
  - **SUM** : 합계 리턴
  - **AVG** : 평균 리턴
  - **MAX** : 최댓값 리턴
  - **MIN** : 최솟값 리턴
  - **STDDEV** : 표준편차 리턴
  - **VARIANCE** : 분산 리턴

```
// COUNT 예시
SELECT COUNT(*)
FROM 테이블명
[WHERE 조건];

// SUM, AVG 예시
SELECT SUM(컬럼명), AVG(컬럼명)
FROM 테이블명
[WHERE 조건];

// MAX, MIN 예시
SELECT MAX(컬럼명), MIN(컬럼명)
FROM 테이블명
[WHERE 조건];

// STDDEV, VARIANCE 예시
SELECT STDDEV(컬럼명), VARIANCE(컬럼명)
FROM 테이블명
[WHERE 조건];
```

<br>
<br>

### D. 그룹 함수
  - 테이블의 전체 행을 하나 이상의 컬럼을 기준으로 컬럼 값에 따라 그룹화하여 그룹별로 결과를 출력

#### 1. 그룹 함수의 유형
  - **ROLLUP** : 소계(소그룹 합계) 등 중간 집계 값 산출
  - **CUBE** : 결합 가능한 모든 값에 대해 다차원 집계를 생성
  - **GROUPING SETS** : 집계 대상 컬럼들에 대한 개별 집계, 컬럼 간 순서 무관

```SQL
// ROLLUP 예시
SELECT 컬럼명1, 컬럼명2, 집계함수(컬럼명3), ...
FROM 테이블명
GROUP BY ROLLUP(컬럼명1, 컬럼명2);

// CUBE 예시
SELECT 컬럼명1, .., 집계함수
FROM 테이블명
[WHERE 조건]
GROUP BY [컬럼명1, ...] CUBE(컬럼명a, ...)
[HAVING 조건식]
[ORDER BY 컬럼명];

// GROUPING SETS 예시
SELECT 컬럼명1, ..., 집계함수
FROM 테이블명
[WHERE 조건]
GROUP BY [컬럼명1, ...]
GROUPING SETS(컬럼명1, ...)
[HAVING 조건식]
[ORDER BY 컬럼명];
```

<br>
<br>

### E. 윈도 함수
  - DB를 사용한 온라인 분석 처리 용도로 사용하기 위해 표준 SQL에 추가된 함수
  - OLAP 함수 : 의사결정 지원 시스템

#### 1. 윈도 함수 구문

```SQL
SELECT 함수명(파라미터)
OVER
([PARTITION BY 컬럼1, ...]
[ORDER BY 컬럼A, ...])
FROM 테이블명;
```

<br>

#### 2. 순위 함수
  - **RANK** : 특정 컬럼에 대한 순위를 리턴
  - **DENSE_RANK** : 동일 순위에 동일 순위 / 2위 2위 2위 3위 3위 4위 ...
  - **ROW_NUMBER** : 동일 순위 무관 / 2위 3위 4위 5위 6위 7위 ...

```SQL
SELECT 컬럼명1, 컬럼명2, ...
RANK() OVER (ORDER BY 컬럼명 DESC) 결과컬럼명,
DENSE_RANK() OVER (ORDER BY 컬럼명 DESC) 결과컬럼명,
ROW_NUMBER() OVER (ORDER BY 컬럼명 DESC) 결과컬럼명
FROM 테이블;
```

***

<br>
<br>
<br>
<br>

# C3. SQL 활용 및 최적화

<br>

## i. 절차형 SQL

### A. 절차형 SQL
  - 일반적인 개발 언어처럼 SQL 언어에서도 절차 지향적인 프로그램이 가능하도록 하는 트랜잭션 언어

<br>

### B. 절차형 SQL 종류
  - **프로시저** : 일련의 쿼리들을 마치 하나의 함수처럼 실행하기 위한 쿼리 집합
  - **사용자 정의 함수** : SQL 수행 결과를 단일 값으로 반환할 수 있는 절차형 SQL
  - **트리거** : 삽입, 갱신, 삭제 등의 이벤트 발생 시 관련 작업 자동 수행 절차형 SQL

<br>
<br>
<br>

## ii. SQL 최적화

### A. 쿼리 성능 개선
  - DB에서 프로시저에 있는 SQL 실행 계획을 분석, 수정을 통해 최소의 시간으로 원하는 결과를 얻도록 프로시저를 수정하는 작업

<br>

### B. 옵티마이저
  - SQL을 가장 빠르고 효율적으로 수행할 최적의 처리경로를 생성해주는 DBMS 내부 핵심엔진
  - **규칙기반 옵티마이저(PBO)** : 통계 정보 없는 상태에서 사전 등록 규칙에 따라 질의 실행 계획 선택
  - **비용기반 옵티마이저(CBO)** : 통계 정보로부터 모든 접근 경로를 고려한 질의 실행 계획 선택

***

<br>
<br>
<br>

***

문제의 비중이 제일 높은 과목 중 하나인 SQL 파트이다.

진짜 문법 외우면서 정리하는데 토할 뻔 했다.    
사실 문제를 계속 풀어보면서 직접 SQL 구문을 작성해봐야 더 느낌이 올 것이다.

기본적인 DDL/DML/DCL 개념, SQL 구문, 집계함수 사용법 등 알고는 있지만 다시 한 번 깔끔하게 정리한 기분이다.

사실 SQL 파트는 거의 모~든 내용이 중요하기 때문에 굳이 추려낼 건 없다고 생각하지만
정말 핵심만 따지면 DDL, DML, DCL, 집계함수 정도가 되지 않을까싶다.

그래도 시험에 나왔거나 한 부분도 있고, 출제할만한 건덕지가 많기 때문에 굳~이 굳이 추려보자면

트랜잭션 개념     
트랜잭션 특성     
트랜잭션 상태 변화     
트랜잭션 제어     
병행제어 기법 종류     
DB 고립화 수준     
영속성 회복 기법     
DDL의 대상     
DB 파일 구조     
DDL 명령어     
DML 명령어     
DCL 명령어     

다중 행 연산자     
집계함수     

절차형 SQL 종류     
옵티마이저     

이렇게 일듯하다.   
사실상 전부 다...

진짜진짜 정 안되겠으면 DDL, DML, DCL 문법과 집계함수 응용만 파면 된다.    
즉, SQL 문을 반복적으로 직접 작성해서 지엽적인 개념 다 버리더라도 SQL 문은 작성할 줄 알아야한다는 거다.

그래도 나왔던 문제는 안나오는 시험이기 때문에 사실상 전부 외워야된다...
