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
