---
title: "SQLD / 모의고사 정리 1"
author: Jeremiah Lee
date: 2024-11-15
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

#### 다음 결과 2개의 테이블을 어떤 JOIN으로 진행한 것인가?	

![](/assets/img/Certificates/SQLD/SQLD_MOCKTEST_1.png)

1. Natural Join
2. Right Outer Join
3. Left Outer Join
4. Full Outer Join

##### 정답 : 4
  - EMPNO와 DEPTNO 각각 서로 NULL 값이 있음.

<br>

#### 엔티티 간 1:1, 1:M과 같이 관계의 기수성을 나타내는 것을 무엇이라 하는가?

1. 관계 차수
2. 관계명
3. 관계선택성
4. 관계정의

##### 정답 : 1
  - 엔티티 간 1:1, 1:M 등 관계 참여 인스턴스의 수를 지칭하는 것은 관계 차수

<br>

#### 3차 정규화에 대한 설명으로 옳은 것을 고르시오.

1. 속성 간 종속성을 가지면 안 된다.
2. 모든 속성은 반드시 기본 키 전부에 종속되어야 한다.
3. 모든 속성은 반드시 하나의 값을 가져야 한다.
4. 다수의 주식별자를 분리시킨다.

##### 정답 : 1
  - 식별자를 제외한 일반 속성 간 종속 제거 = 3차 정규화
  - 참고 (도부이 결다조)
    - 1차 정규화 : 원자 값 아닌 도메인 분해
    - 2차 정규화 : 부분 함수 종속 제거
    - 3차 정규화 : 이행 함수 종속 제거 (일반 속성 간 종속 제거)
    - BCNF : 결정자가 후보키가 아닌 것 제거
    - 4차 정규화 : 다치 종속 제거
    - 5차 정규화 : 조인 종속 제거

<br>

#### 다음 설명에 해당하는 모델링 관점은 무엇인가?

```
업무가 어떤 데이터와 관련이 있는지 또는 데이터 간의 관계는 무엇인지에 대해서 모델링하는 관점
```

1. 프로세스 관점
2. 데이터와 프로세스의 상관 관점
3. 데이터와 데이터 간의 상관 관점
4. 데이터 관점

##### 정답 : 4
  - 데이터 모델링의 세 가지 관점
    - 데이터 관점 (관계무엇) : 업무가 어떤 데이터와 관련이 있는지 또는 데이터 간의 관계는 무엇인지에 대해서 모델링 하는 방법 (What, Data)
    - 프로세스 관점 (실제어떻게) : 업무가 실제로 하고 있는 일은 무엇인지 또는 무엇을 해야 하는 지를 모델링 하는 방법 (How, Process)
    - 데이터와 프로세스의 상관 관점 : 업무가 처리하는 일의 방법에 따라 데이터는 어떻게 영향을 받고 있는 지 모델링 하는 방법 (Interaction)

<br>

#### 아래의 결괏값을 보고 SQL문의 빈칸에 들어갈 수 있는 내용을 고르시오.

![](/assets/img/Certificates/SQLD/SQLD_MOCKTEST_2.png)

1. ROLLUP(DEPTNO, JOB)
2. GROUPING SETS(DEPTNO, JOB)
3. DEPTNO, JOB
4. CUBE(DEPTNO, JOB)

##### 정답 : 1
  - DEPTNO별 합계, DEPTNO, JOB별 합계, 전체 합계가 조회되므로 빈칸에는 그룹 함수 중 ROLLUP이 와야함
  - ROLLUP(1, 2) : 1 합계, 1 + 2 합계, 총 합계 리턴 (계층 구조, 순서 상관)
  - GROUPING SETS(1, 2) : 1 합계, 2 합계, 1 + 2 합계, 총 합계 리턴 (순서 무관)
  - CUBE(1, 2) : 1 합계, 2 합계 (순서 무관)

<br>

#### 다음 SQL문의 실행 결과는 무엇인가?

```
SELECT COALESCE(NULL, '2', '1') FROM DUAL;
```

1. 1
2. 2
3. 3
4. NULL

##### 정답 : 2
  - COALESCE 함수는 NULL이 아닌 첫 번째 값을 리턴하는 함수, 쿼리에서 1 번째는 NULL 이기 때문에 그 다음 2가 옴

<br>

#### 다음 중에서 집합 연산자의 종류에 해당되지 않는 것을 고르시오.
  - UNION ALL
  - UNION
  - PROJECT
  - EXCEPT

##### 정답 : 3
  - 집합 연산자 종류 : UNION / UNION ALL / EXCEPT / INTERSECTION / DIFFERENCE / PRODUCT
  - PROJECT 는 순수 관계 연산자

<br>

#### 숫자형 함수 적용과 그 결괏값이 올바르지 않은 것은?

1. ABS(-30) = 30
2. SIGN(-50) = -1
3. MOD(7,3) = 2
4. CEIL = 39

##### 정답 : 3
  - MOD(7,3)은 7을 3으로 나누었을 때의 나머지, 결괏값은 1
  - ABS는 n의 절댓값 리턴
  - SIGN은 n의 부호를 반환 -> ex) 양수일 경우 1, 음수일 경우 -1, 0 = 0

<br>

#### 다음 SQL문에 대한 설명으로 올바르지 않은 것은?

```
SELECT ENAME, SAL,
NTILE(4) OVER (ORDER BY SAL DESC) as DATA
FROM EMP;
```

1. DATA 필드가 가질 수 있는 값의 범위는 0~3까지다.
2. SAL이 큰 순을 조회된다.
3. SAL의 값에 따라서 데이터를 4등분으로 분류해서 DATA 필드로 조회된다.
4. SAL의 마지막 행은 급여가 가장 작은 사람이다.

##### 정답 : 1
  - NTILE(n) 윈도우 함수는 데이터를 n값으로 n등분하는 함수이다.
  - 위에서 n값은 4이기 때문에 1~4 4등분

<br>

#### 엔티티에 대한 개념 중 엔티티 정의의 공통점 3가지가 아닌 것은?

1. 데이터베이스 내에서 변별 가능한 객체이다.
2. 엔티티는 사람, 장소, 물건, 사건, 개념 등의 명사에 해당된다.
3. 저장되기 위한 어떤 것이다.
4. 업무상 관리가 필요한 관심사에 해당된다.

##### 정답 : 1

<br>

#### 사원 테이블에 사원번호는 기본키로 설정되어 있다. SQL문으로 사원번호 1번을 검색하는데 사원 테이블에는 하나의 ROW만 저장되어 있다. 이때 유리한 스캔 방식을 무엇으로 판단되는가?

1. Unique Index Scan
2. Non-Unique Index Scan
3. Index Full Scan
4. Table Full Scan

##### 정답 : 4
  - 하나의 데이터(행)을 읽기 위해서는 인덱스를 굳이 사용하지 않고 테이블을 FULL SCAN 하는 것이 효율적임.
  - 즉, 문제에서 검색되는 행이 1건이므로 굳이 인덱스를 읽지 않고 바로 테이블을 전체 검색해야함

<br>

#### 다음의 SQL문에 대한 설명으로 올바르지 않은 것은?

```
SELECT JOB, ENAME, SAL,
RANK(
) OVER (ORDER BU SAL DESC) ALL_RANK,
RANK(
) OVER (PARTITION BY JOB ORDER BY SAL DESC) JOB_RANK
FROM EMP;
```

1. SAL 컬럼은 급여가 큰 순으로 조회된다.
2. JOB 별로 SAL이 큰 등수가 조회된다.
3. RANK() 함수를 사용했으므로 급여가 동일한 사람이 있다면, 조회 순서에 따라서 1등과 2등으로 표시된다.
4. PARTITION 문을 사용해서 해당 파티션 내에서 순위를 계산한다.

##### 정답 : 3
  - RANK() 함수는 동일한 순위에 대해 같은 등수로 리턴함

<br>

#### 다음 개념에 해당하는 관계는 무엇인가?

```
부모 엔티티로부터 속성을 받았지만, 자식 엔티티의 주식별자로 사용하지 않고 일반적인 속성으로만 사용한다.
```

1. 식별자 관계
2. 비식별자 관계
3. 일반 속성 관계
4. 외부 식별 관계

##### 정답 : 2
  - 부모 엔티티로 부터 속성을 받았지만 자식 엔티티의 주식별자로 사용하지 않고 일반적인 속성으로만 사용하는 것은 비식별자 관계에 대한 설명
  - 식별자 관계 = 부모 엔티티의 주식별자가 자식 엔티티의 주식별자로 포함됨
  - 일반 속성 관계 = 서로 독립적인 엔티티 간 연결, 단순 속성 연관

<br>

#### 분산 데이터베이스의 특징 중 저장 장소 명시가 불필요하다는 특성은 무엇인가?

1. 지역 사상 투명성
2. 위치 투명성
3. 병행 투명성
4. 분할 투명성

##### 정답 : 2
  - 데이터 저장 장소 명시 불필요, 위치 정보 시스템 카탈로그에 유지

<br>

#### 다음의 SQL문에 대한 설명으로 올바른 것은?

```
SELECT 'A', 1 FROM DUAL
UNION ALL
SELECT 1, 'A' FROM DUAL;
```

1. 위의 SQL문 실행 결과는 A 1, 1 A가 조회된다.
2. UNION ALL을 사용해서 합집합을 만들고 중복을 제거한다.
3. 위의 SQL문은 실행되지 않는다.
4. 실행 결과로 아무것도 출력되지 않는다.

##### 정답 : 3
  - 각 열 데이터 타입 불일치로 실행 안됨

<br>

#### Sort Merge 방식의 조인이 Nested Loop 방식 조인보다 효율적으로 판단되는 것을 고르시오.

1. 기본키와 외래키 관계에서 외래키에 인덱스가 없을 때
2. 사용자가 발주한 주문에 대해서 체결 정보 한 건을 확인할 때
3. 병렬로 테이블을 읽고 SORT_AREA_SIZE가 작은 경우
4. Random Access가 자주 발생하지 않을 때

##### 정답 : 1
  - Sort Merge 방식의 조인은 두 개의 테이블을 정렬 한 후에 병합함.
  - 병합이 완료되면 한 번의 Full Scan으로 데이터를 검색함
  - 따라서 기본키와 외래키 관계에서 외래키에 인덱스가 없을 때 옵티마이저가 Sort Merge 방식의 조인으로 동작하게 함

<br>

#### 다음 중 분산 데이터베이스의 투명성(Transparency)에 속하지 않는 것은?

1. 분할 투명성
2. 병행 투명성
3. 중복 투명성
4. 병렬 투명성

##### 정답 : 4
  - 분산 데이터베이스의 투명성의 종류에는 분할 투명성, 위치 투명성, 지역 투명성, 중복 투명성, 병행 투명성, 장애 투명성이 있다.
  - 분위지중병장

<br>

#### 테이블 R과 S가 다음과 같을 때, 아래 SQL문 (SELECT 구문)의 실행 결과로 옳은 것은?

![](/assets/img/Certificates/SQLD/SQLD_MOCKTEST_3.png)

1. 4
2. 5
3. 9
4. 20

##### 정답 : 4
  - R과 S에 조인 조건이 따로 없어서 카테시안 곱이 발생함, 4 * 5 (cross join 과 동일한 효과)

<br>

#### 우선순위를 계산하는 윈도우 함수에서 동일한 우선순위가 나와도 고유의 값을 부여하기 위한 방법으로 올바른 것은 무엇인가?

1. SELECT RANK() OVER (PARTITION BY DEPTNO ORDER BY SAL DESC) DEPT_RANK;
2. SELECT DENSE_RANK() OVER (PARTITION BY DEPTNO ORDER BY SQL DESC) DEPT_RANK;
3. SELECT ROW_NUMBER() OVER (PARTITION BY DEPTNO ORDER BY SAL DESC) DEPT_RANK;
4. SELECT UNIQUE_RANK() OVER (PARTITION BY DEPTNO ORDER BY SAL DESC) DEPT_RANK;

##### 정답 : 3
  - ROW_NUMBER() 는 동일한 우선순위가 나올 때 고유 값 부여
  - DENSE_RANK() 는 동일한 값을 가진 행에 대해 같은 순위 부여
  - UNIQUE_RANK() 는 동일 순위 일때 중복 없는 순위 부여
