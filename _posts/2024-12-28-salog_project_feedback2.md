---
title: "프로젝트:샐로그 / 사용자 피드백 1 - DB 내용 수정"
author: Jeremiah Lee
date: 2024-12-28
categories: [ 프로젝트, 샐로그, 피드백 ]
tags: [프로젝트, 샐로그, 피드백]
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

샐로그에 대한 피드백을 받으면서 백엔드 개발을 현업으로 하고 계시는 지인 분께 DB 테이블을 보여드린 적이 있다.

그때 말씀하셨던 게 회원에는 createdAt 이라고 생성 시간을 별도로 저장하는데, 왜 다른 테이블에는 이 컬럼이 없냐고 물어보셨다.

지금까지는 특별하게 생각하지 않았었고, 기능들을 만드는 데에 급급하여 크게 신경 쓰지 못 했던 것 같다.

하지만 어떤 문제가 생기거나 확인이 필요할 때 시스템 로그를 살펴보는 것은 굉장히 중요한 작업이고, 이느 시점에 이러한 로그가 생성이 되었는 지 파악하는 것은
문제 파악과 해결의 첫 걸음이다.

그래서 기존 DB 설계 당시에는 회원을 제외하고는 이 createdAt을 추가 하지 않았엇는데, 이번 피드백을 받으면서 보다 상세한 로그는 중요하다고 생각이 들었고
모든 테이블에 createdAt 을 추가하게 되었다.

<br>

또한, 이 생성 시간에 대한 것은 기존에 있던 Date 컬럼과는 달리 시스템에서 이 데이터가 언제 생성되었는 지 확인할 수 있는 변경 불가한 값이다.

Date의 경우는 사용자가 수정 할 수 있는 비지니스 로직에 해당한다.

그렇기 때문에 기존 Date 컬럼을 수정하는 것이 아닌 회원 테이블에 존재하는 createdAt 컬럼을 추가하도록 DB를 수정했다.

<br>

이 외에도 보다 나은 유지보수를 위하여 기존 DB 다이어그램의 각 항목에 주석으로 어떤 것을 의미하는 지 작성하였다.

<br>
<br>
<br>

### Auditable 클래스

보통 테이블들이 공통적으로 가지고 있는 필드나 컬럼이 있을 수 있다.

공통적으로 가지고 있다는 것은 결국 중복이 된다는 의미이기 때문에 JPA에서는 이러한 중복 문제를 해소하기 위해 Audit 기능을 제공한다.

특히 모든 도메인에 대해 생성일자나 수정일자 등은 공통적으로 기록이 되어야 하기 때문에 이 Audit 기능을 활용한다.

Audit 기능은 자동으로 시스템 시간을 매핑하여 DB 테이블에 값을 저장하게 된다.

```java
import lombok.Getter;
import org.springframework.data.annotation.CreatedDate;
import org.springframework.data.jpa.domain.support.AuditingEntityListener;

import javax.persistence.Column;
import javax.persistence.EntityListeners;
import javax.persistence.MappedSuperclass;
import java.time.LocalDateTime;

@Getter
@MappedSuperclass // 이 클래스 자체는 테이블로 매핑되지 않지만 이 클래스의 필드는 하위 클래스에서 매핑됨
@EntityListeners(AuditingEntityListener.class) // auditing 기능 사용
public class Auditable {
    @CreatedDate // 엔티티가 생성되고 저장될 때 시간 자동 저장
    @Column(name = "created_at", updatable = false, columnDefinition = "TIMESTAMP")
    private final LocalDateTime createdAt = LocalDateTime.now();
    
    // 수정 시간 표시 시 @LastModifiedDate 애너테이션 추가
}
```

이와 같은 Auditable 클래스를 생성하여 JPA로 테이블을 매핑, 데이터를 저장할 때 LocalDateTime 타입의 시스템 시각을 같이 저장하게 된다.

여기서 컬럼의 명칭은 created_at으로 지정되며, 그 타입은 TIMESTAMP 타입이다.

기존에는 columnDefinition 옵션을 주지 않고 저장했었는데, 명시적으로 작성해 주었다.

자바 코드 베이스에서는 TimeStamp 타입이 아닌 LocalDateTime 타입을 활용하여 자바에서의 안정성을 유지하되, DB의 확장 가능성을 염두해 두고 
데이터베이스에서 보편적으로 사용되는 TimeStamp 타입을 명시한 것이다.

이외에, 기본적으로 수정시간도 함께 저장되면 좋다.

<br>
<br>
<br>

### 실제 사용 - member 엔티티 예시

그렇다면 위 Auditable 클래스를 어떻게 활용할 수 있을까?

```java
@NoArgsConstructor
@Getter
@Setter
@Entity
public class Member extends Auditable {
  ...
}
```

이런 식으로 상속만 받으면 된다.

위에서 @MappedSuperclass 의 주석 부분을 보면 알 수 있지만 이 애너테이션을 활용하여 상속만 받으면 하위 클래스에서 Auditable 클래스의 필드가 매핑되어
함께 저장되도록 할 수 있다.

이렇게 JPA의 Audit 기능을 활용하여 모든 엔티티에 생성일자나 수정일자를 중복 없이 저장할 수 있다.

<br>
<br>
<br>

### 결론

위와 같은 내용을 통해 기존에는 회원을 제외한 엔티티들은 Auditable 클래스를 상속 받지 않았지만 현재는 수정되어 상속 받고, 생성일자를 함께 저장하도록 변경 되었다.

이후 테이블 명세서와 다이어그램을 수정하고, 안정성을 위해 직접 배포 DB에 접근하지 않고 yml 파일을 활용하여 db 생성 전략을 validate에서 update로 변경 후
다시 validate로 수정해 놓았다.

<br>
<br>
<br>

### 후기

이 내용도 사실 꽤나 큰 실수라고 생각한다.

추후 문제가 생겼을 때 언제 터졌는 지 알아야하는 데, 이러한 내용이 없으면 언제 터졌는지 알기 어렵다.

<br>

이외에도 테이블 설계 시에 로그 테이블이라 해서 각 테이블이 저장되는 시간이나 로그 등을 저장하는 테이블을 별도로 생성한다고 한다.

이건 생각도 못 했었는데 듣고 보니 로그를 한번에 모아 볼 수 있도록 조치를 미리 취해 놓는 것이 사고에 대한 예방이라고 생각이 든다.

추후 다른 프로젝트를 하게 되면 DB 설계 시에 로그 테이블을 별도로 생성하여 연결된 엔티티의 생성시간, 수정시간, 상세한 로그를 저장하는 테이블을 별도로 생성해보도록 하자.
