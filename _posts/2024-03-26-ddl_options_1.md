---
title: "스프링 부트 설정 : ddl-auto"
author: Jeremiah Lee
date: 2024-03-26
categories: [ 스프링 부트, 설정 ]
tags: [스프링 부트, 설정, 개념]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/solo_study_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

### ***개요***

이번 "샐로그" 프로젝트 뿐만 아니라 이전 "라잇팅" 프로젝트도 마찬가지로 스프링 부트 설정 중 ddl-auto를 사용했다.

이 ddl-auto 옵션은 개발 시, 배포 시 각 상황에 따라 속성값이 달라야하며, 편리하지만 위험한 기능이기 때문에 각 속성에 대해 잘 알아두어야 한다.

"위험"도 그냥 위험한게 아니라 정말 위험하기 때문에 꼭 알고가자.

<br>
<br>
<br>

### ***ddl-auto란***

이 ddl-auto는 JPA의 구현체 중 하나인 하이버네이트의 기능이다.

여기서 DDL은 Data Definition Language로 데이터 정의어이다.

즉, 데이터 정의어를 자동으로 작성해주는 기능인 것이다.

<br>

즉 jpa의 하이버네이트 기능 중 하나 이기 때문에 아래와 같은 위치에 설정해야한다.

```yml
spring:
  jpa:
    hibernate:
      ddl-auto: create-drop
```

앞서 말했듯이 이 옵션의 경우, 아주 위험한 상황을 초래할 수 있는데, 그 이유는 데이터 정의어를 작성해주는 기능이기 때문이다.

데이터 정의어에는 CREATE, ALTER, DROP, TRUNCATE가 있는데 이 명령어만 봐도 무시무시하다.

자동적으로 삭제하거나 초기화될 수 있다는 의미이다.

그렇기 때문에 속성의 종류를 잘 알아 둬야한다.

<br>
<br>
<br>

### ***ddl-auto 속성***

ddl-auto에 적용할 수 있는 속성은 다음 5가지가 있다.
1. create
2. create-drop
3. update
4. validate
5. none (default 속성)

이 각 속성이 무슨 역할을 하는지 잘 알아보고 사용하자.

<br>

##### **create**

create는 엔티티로 등록된 클래스와 매핑되는 테이블을 자동으로 생성한다.

이 때, 기존에 해당 클래스와 매핑되는 테이블이 존재한다면 DROP 후 생성한다.

<br>

##### **create-drop**

create-drop은 create와 비슷하게 엔티티로 등록된 클래스와 매핑되는 테이블이 존재한다면 기존 테이블을 삭제하고 자동으로 테이블을 생성한다.

여기서 추가로 애플리케이션이 종료될 때 모든 테이블을 DROP 한다.

<br>

##### **update**

update는 엔티티로 등록된 클래스와 매핑되는 테이블이 없으면 새로 생성하는 것은 create와 동일하지만, 기존 테이블이 존재한다면 create와 create-drop 과 달리 DROP 하지 않고
변경사항을 적용한다.

예를 들어, 컬럼등을 변경한다.

하지만, 일반적으로 생각하는 update와 다르게 모든 변경사항을 반영하는 것은 아니다.

기존의 컬럼 속성은 건드리지 않고 새로운 컬럼이 추가되는 것만 반영한다.

즉, 어떤 클래스의 컬럼이 String 속성인데, 이를 Int로 변경해도 반영되지 않는다는 것이다.

<br>

##### **validate**

validate의 경우 다른 속성들과 달리 DDL을 작성하여 테이블을 생성하거나 수정, 삭제하지 않고 엔티티 클래스와 테이블이 정상적으로 매핑되는지만 검사한다.

만약 검사 후 테이블이 존재하지 않거나, 테이블에 엔티티의 필드에 매핑되는 컬럼이 존재하지 않는다면 예외가 발생하여 애플리케이션이 종료된다.

이 때 발생하는 예외는 "SchemaManagementException"이다.

validate도 update와 비슷하게 클래스의 필드가 매핑되는 테이블에 모두 존재만 한다면, 테이블에 컬럼을 추가하더라도 아무 일이 일어나지 않는다.

<br>

##### **none (default 속성)**

none은 따로 작업을 하지 않고 위의 4가지 경우를 제외한 모든 경우에 해당한다.

이는 기본 옵션으로, 스프링 부트에서는 none이라고 명시하거나 아예 ddl-auto 속성을 명시하지 않아야 적용된다.

이 none을 적용하면 말 그대로 아무일도 일어나지 않는다.

<br>
<br>
<br>

### ***각 상황에 따른 ddl-auto 속성값***

ddl-auto의 속성값에는 어떤 것이 있는지에 대해 알아보았다면, 어느 순간에 어떤 속성을 적용해야하는 지도 알아야한다.

<br>

##### **1. 개발 초기 단계 또는 로컬에서 테스트 시**

이 경우에는 create, create-drop, update 속성을 적용할 수 있다.

이 단계에서는 엔티티에 매핑되는 테이블이 시시각각 변경될 수 있으며, 테스트를 위해 DB 데이터를 초기화해야할 수 있기 때문이다.

<br>

##### **2. 테스트 서버에서**

이 경우에는 update 또는 validate 속성을 적용할 수 있다.

이 단계에서는 말 그대로 "테스트"이기 때문에 어느 정도 완성된 환경에서 진행하는 단계이다.

즉, 테이블이 정확히 생성되었는지 validate로 확인해볼 수 있으며, 약간의 변경사항이 있다면 update를 적용하여 수정할 수 있다.

<br>

##### **3. 스테이징 및 운영 서버**

이 경우에는 validate 또는 none 속성을 적용할 수 있다.

이 단계에서는 어떠한 일이 있더라도 테이블이 변경되어서는 안된다.

서비스를 진행 중인 상황이기 때문에 변경사항이 있으면 어떤 사이드이펙트가 생길 지 모르고, 배포가 끊길 수 있다.

<br>
<br>
<br>

### ***ddl-auto 사용 시 취급주의***

ddl-auto가 무엇인지, 어떤 속성값이 있는지에 대해 알아보았다.

솔직히 속성값에 대한 내용만 읽어봐도 이 편리한 기능이 왜 위험한지 눈치챘을 것이다.

아래는 유튜브 개발바닥 채널에서 말하는 ddl-auto의 위험한 점이다. 참고하자.

<iframe width="560" height="315" src="https://www.youtube.com/embed/SWZcrdmmLEU?si=PQqoa4G0Ilp6YYlC" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<br>

또 주의해야할 점은, 스프링 부트 프로젝트 진행 시 스프링 부트의 다중 profile을 사용하여 환경에 따라 다른 설정을 적용할 때,
해당 profile에서 설정하지 않은 속성은 공통 속성에서 가져온다는 점이다.

위 동영상의 뒷부분에 나오는데, 로컬 개발 시 ddl-auto를 create로 설정했는데, 운영용 profile에서는 옵션을 별도로 설정 해놓지 않았기 때문에
로컬 profile의 ddl-auto 옵션이 적용되어 공통 속성의 create가 적용되었다는 내용이다.

이 경우 서비스 도중 모든 데이터가 날아가기 때문에 정말정말정말 주의해야한다.

또한 이러한 경우에 대비하여 DB를 백업 해놓는 것이 아주아주아주 중요하다.

<br>
<br>
<br>

***

이번에는 프로젝트 진행 시 자주 적용한 ddl-auto 옵션에 대해 알아보았다.

이전까지는 그냥 이렇구나~하고 사용했다면 이번에는 위험한 사항과 세부 내용을 꼼꼼히 체크하고 사용할 수 있다.

앞서 말했듯이 ddl-auto 옵션은 특히 스프링 부트에서 취급에 주의해야한다.
