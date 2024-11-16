---
title: "SQLD / 모의고사 정리 5"
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

#### SQL 트레이스를 수집한 결과 Row Source Operation이 다음과 같았다. 가장 우선적으로 검토할 사항으로 올바른 것은? (단, 한 달간 주문 건수는 평균 50만 건이다)

![](/assets/img/Certificates/SQLD/SQLD_MOCKTEST_5.png)

1. 고객_IDX 인덱스 컬럼 순서를 조정한다.
2. 주문_IDX 인덱스 컬럼 순서를 조정한다.
3. 고객_IDX 인덱스에 컬럼을 추가한다.
4. 주문_IDX 인덱스에 컬럼을 추가한다.

##### 정답 : 3
  - 고객_IDX 인덱스를 읽고 나서 고객 테이블을 액세스하는 횟수는 많으나 대부분 필터링 되고 있음
  - 고객에 대한 조건절로 사용된 고객등급이나 연령 컬럼이 인덱스에 포함돼 있지 않아 생기는 현상이므로 고객_IDX에 컬럼을 추가해 주어야 한다.

<br>

#### 다음 중 해시 조인(Hash Join)에 대한 설명으로 올바르지 않은 것은?

1. 해시 조인은 해시 함수를 사용해서 주소를 계산하고 조인을 수행한다.
2. 해시 조인을 할 때는 선행 테이블의 크기가 작아야 한다.
3. 해시 조인은 CPU 연산이 많이 발생한다.
4. 해시 조인은 랜덤 액세스(Random Access)로 인하여 부하가 발생한다.

##### 정답 : 4
  - 해시 조인은 해시 함수를 사용하므로 CPU를 많이 사용하지만, 랜덤 액세스는 발생하지 않음
  - 랜덤 액세스 = Nested Loop

<br>

#### 다음 주어진 테이블에서 아래와 같은 결괏값을 반환하도록 아래의 SQL문의 빈칸에 들어갈 올바른 것을 고르시오.

![](/assets/img/Certificates/SQLD/SQLD_MOCKTEST_6.png)

1. RANK()
2. NTILE()
3. ROW_NUMBER()
4. DENSE_RANK()

##### 정답 : 4
  - RANK와 DENSE_RANK 모두 동일 순위에 대해 중복 순위를 부여하지만 RANK의 경우 다음 순위를 건너뜀
  - RANK 일 때 = 1, 2, 2, 4
  - **NTILE()** : 결과 집합을 N개의 그룹으로 나누고 각 행에 해당 그룹 번호를 부여함
  - **ROW_NUMBER()** : 결과 집합의 각 행에 대해 고유한 번호를 부여함, 순서 상관

<br>

#### Case 문에서 ELSE를 생략하면 어떤 현상이 발생되는가?

1. ELSE를 생략하고 작성하면 실행 시 ELSE 조건이 참이 되며 오류가 발생한다.
2. ELSE 조건이 만족하게 되면 공집합이 리턴된다.
3. ELSE 조건을 만족하게 되면 무시된다.
4. ELSE 조건이 만족하게 되면 NULL이 된다.

##### 정답 : 4
- CASE 문에서 ELSE 조건을 생략하면 NULL 리턴

<br>

#### Hash Join 기법에 대한 설명으로 옳은 것은?

1. 조인 작업을 수행할 때 결과 행의 수가 적은 테이블을 선행 테이블로 사용하는 것이 좋다.
2. Hash Join은 해시 함수를 이용하여 조인을 수행하기 때문에 '='로 수행하는 조인인 동등 조건 이외에도 사용할 수 있다.
3. 해시 테이블을 저장할 때 메모리에 적재할 수 있는 영역의 크기보다 커지면 초과한 크기를 제외한 영역 만큼 메모리에 적재한다.
4. Hash Join은 조인 컬럼의 인덱스가 존재하지 않으면 사용할 수 없는 기법이다.

##### 정답 : 1
  - Hash Join의 특징
    - Hash Join은 조인 컬럼의 인덱스가 존재하지 않을 경우에도 사용 가능
    - Hash Join은 해시 함수를 사용하여 조인을 수행하기 때문에 '='로 수행하는 조인인 동등조건에서만 사용 가능
    - 해시 함수가 적용될 때 동일한 값은 항상 같은 값으로 해시됨을 보장함
    - Hash Join 작업을 수행하기 위해서 해시 테이블을 메모리에 생성해야 함
    - 해시 테이블을 저장할 때 메모리에 적재할 수 있는 영역의 크기보다 커지면 임시 영역(디스크)에 저장함
    - Hash Join을 수행할 때는 결과 행의 수가 적은 테이블을 선행 테이블로 사용하는 것이 좋음
    - 선행 테이블을 Build Input이라고 하며 후행 테이블은 Prove Input이라고 함

<br>

#### 다음의 SQL문 실행 결과는 무엇인가?

```
SELECT * FROM dual WHERE NULL = NULL;
```

1. NULL
2. 1
3. X
4. 공집합

##### 정답 : 4
  - NULL과 NULL을 비교할 수 없음
  - NULL값 조회는 is [NOT] NULL
  - 아무것도 출력 안되기 때문에 공집합
