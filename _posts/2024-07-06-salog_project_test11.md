---
title: "프로젝트:샐로그 / 성능 테스트 - 테스트 환경에서 MySQL 연결 후 부하 테스트"
author: Jeremiah Lee
date: 2024-07-06
categories: [ 프로젝트, 샐로그, 성능 테스트 ]
tags: [프로젝트, 샐로그, 성능 테스트, JMeter, MySQL]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Project_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

### 개요

지난 번에 이어 이번 포스트는 로컬 환경에서 테스트 브랜치를 분리하여 DB를 로컬 MySQL로 연결, 부하테스트를 진행해 본 결과를 작성한다.

기본적으로 Application.yaml 파일에서 기존 H2와의 연결을 끊고 MySQL을 연결하였으며 곧바로 JMeter를 활용하여 부하 테스트를 동일하게 진행해보았다.

순서대로 MySQL과 어떻게 연결을 했는지, 결과는 어땠는 지를 적었다.

<br>
<br>
<br>

### MySQL과 연결

앞서 말한대로 개발 브랜치나 메인 브랜치가 아닌 테스트 브랜치를 따로 생성하여 부하 테스트를 진행했다.

처음에는 배포 환경에 직접 부하 테스트를 진행하고, 문제점을 찾아 개선 작업을 하려고 했는데,
다시 생각해보니 별로 좋지 않은 선택이라는 것을 깨달았다.

우선, 배포 환경은 지속적으로 서비스를 이용할 수 있도록 두어야 한다.

부하 테스트를 진행한다면 지금은 없다하더라도 서버 이용률 폭증으로 사용자들의 불편을 야기할 수 있기 때문에 배포 서버를 직접적으로 테스트하는 것은 좋지 않다고 생각했기 때문이다.

또한, 테스트를 진행하고 문제점을 찾아냈다고 하더라도 개선 작업을 위해서는 서버를 직접적으로 건드려야 한다.

이 과정 중에 예상치 못한 에러가 발생한다면 문제를 찾는 데 오랜 시간이 걸릴 수 있기 때문에 서버가 불안정해지거나 다운될 수 있는 염려가 있다.

<br>

고로 테스트 서버를 분리하여 부하를 주고, 문제점을 찾아, 분석, 개선하는 것이 가장 좋다고 생각했다.

<br>

기본적으로 로컬에서 H2 DB와의 연결을 끊고 MySQL과 연결하는 것은 크게 어렵지 않다.

```yaml
spring:
#  h2:
#    console:
#      enabled: true
#      path: /h2
#  datasource:
#    url: jdbc:h2:mem:test
#  sql:
#    init:
#      data-locations: classpath*:db/h2/
#      mode: always
```

이와 같이 H2 관련 설정을 주석 처리하여 꺼주고,

```yaml
  datasource:
    url: jdbc:mysql://localhost:3306/salogdb
    username: ${MYSQL_ID}
    password: ${MYSQL_PASSWORD}
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    database-platform: org.hibernate.dialect.MySQL8Dialect # mysql8버전
```

데이터 소스를 MySQL로 지정해주면 된다.

<br>

여기서 한 가지 주의할 점은 연결할 MySQL 데이터베이스에 salogdb라는 DB가 먼저 존재해야 저 url을 통해 연결된다.

그리고,

```yaml
  jpa:
    database-platform: org.hibernate.dialect.MySQL8Dialect # mysql8버전
    hibernate:
      ddl-auto: create-drop  # 스키마 자동 생성 (드랍-생성) -> 테스트 환경에서만 적용할 것
```

원활한 테스트 작업을 위해 db에서 실행 시 테이블 생성, 종료 시 삭제할 수 있도록 ddl-auto 옵션을 create-drop으로 변경했다.

이러한 작업은 항상 하던 작업이기 때문에 별다른 특이사항은 없었다.

<br>
<br>
<br>

### MySQL과 연결 후 부하 테스트

앞선 내용을 거쳐서 이제 로컬에서 샐로그(테스트 브랜치)를 실행시키면 DB가 MySQL과 연결되어 실행된다.

이제 이전에 사용했던 1 천명의 회원 가입, 로그인, 10만번의 조회 템플릿을 적용시켜 동일하게 부하 테스트를 진행하면 된다.

DB의 요청에 대한 반응속도, 처리속도가 어떤 가에 따라 사용자의 경험(UX)가 달라진다고 생각하는데,
그 중에서 가장 활용도가 높은 조회 시에 크게 작용한다고 생각한다.

물론 어떤 웹 사이트에서 게시글 작성 등 생성 쿼리 시, 오래 걸린다면 그 만큼 불편함을 느낄 수 있지만,
내가 수십년 간 웹사이트를 이용해온 결과, 보통은 생성보다 조회를 더 많이한다고 느꼈다.

물론 내가 정답이라는 것은 아니기 때문에 무조건은 없다.

<br>

앞서 말한대로 이전 부하 테스트 시와 동일한 테스트 플랜을 사용했다.

![](/assets/img/projects/salog/stress_test_MySQL/test_plans.png)

마찬가지로 수입 조회(일/월) 10만 번의 요청을 보내보았다.

![](/assets/img/projects/salog/stress_test_MySQL/income_get.png)

<br>

이전에는 1.7 ~ 2.3초 범위 내로 결과가 도출되었는데, 이번에는 어떨까?

![](/assets/img/projects/salog/stress_test_MySQL/daily_income_get_result.png)
![](/assets/img/projects/salog/stress_test_MySQL/monthly_income_get_result.png)

H2와 큰 차이를 보이지 않는다.

<br>

사실 당연한 일이다. DB 성능 보다는 어떤 식으로 조회하도록 코드가 짜여 있는가, DB 내의 논리 구조는 어떤가에 따라 이 속도가 차이날 것이기 때문이다.

그렇기 때문에 쿼리를 최적화 하거나, 데이터베이스의 구조를 변경하는 등의 개선 작업을 거쳐야 좀 더 빠르게 개선할 수 있을 것이다.

<br>
<br>
<br>

### 결과와 개선 계획

배포 환경은 도커를 활용해서 외부의 영향을 받지 않고 컨테이너 내부에서 실행 되도록 구성되어 있기 때문에 완전히 동일한 환경에서 진행된 테스트는 아니다.

하지만 로컬에서 개선 작업을 거치게 되면 쿼리 최적화나 DB 튜닝을 진행할 것이기 때문에 충분히 배포되어 있는 서버에서도 개선이 이루어질 것이라 생각한다.

이제 다음 포스트에서는 JMeter를 활용하여 부하를 줌과 동시에 프로메테우스, 그라파나를 활용해 모니터링을 거친 후 쿼리 성능을 분석하여 개선 작업을 진행할 예정이다.

<br>
<br>
<br>

### 후기

이번 포스트는 딱히 큰 내용은 없다.

이제 다음으로 모니터링을 적용한 다음 분석을 진행해보자.
