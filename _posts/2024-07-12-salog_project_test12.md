---
title: "프로젝트:샐로그 / 성능 테스트 - (MySQL) 모니터링 적용 / 그라파나 비밀번호 분실"
author: Jeremiah Lee
date: 2024-07-12
categories: [ 프로젝트, 샐로그, 모니터링 ]
tags: [프로젝트, 샐로그, 성능 테스트, 모니터링, JMeter, MySQL, 그라파나]
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

이전 포스트에서는 부하 테스트만 진행해 보았고 이번에는 H2 데이터 베이스를 사용할 때와 마찬가지로 프로메테우스를 활용하여 매트릭을 수집하고
그라파나를 통해 시각화를 진행했다.

우선 말하지만 그라파나의 경우 대쉬보드를 이전에 사용하던 패널들을 그대로 가져왔기 때문에 동일하며, 프로메테우스도 별다른 설정 변경은 없었다.

<br>

한 가지 특별한 점은 로컬 3000번 포트로 그라파나 접근 시 비밀번호가 맞지 않는 문제가 있어서 이걸 해결했다.

또한, DB 관련 매트릭을 수집하고 있지 않았기 때문에 정확한 조회 속도 감소에 대한 원인 분석이 필요하여 MySQL Exporter를 설치하고 매트릭을 수집하도록 설정했는데,
이는 다음 포스트에서 원인 분석을 하면서 같이 다룰 예정이다.

<br>

내용을 보면 알겠지만 H2를 다룰 때와 큰 차이가 없었으며 짧다.

중간에 비밀번호 초기화를 재설치 없이 어떻게 진행했는지 기록했으니 추후 같은 문제 발생 시 이 포스트를 보고 해결할 수 있을 것이다.

<br>
<br>
<br>

### MySQL 연결 후 모니터링 결과

![](/assets/img/projects/salog/monitoring_mysql/mysql_salog_monitoring.png)

[이전 h2 포스트](https://021skyfall.github.io/posts/salog_project_test10/)의 데이터와 큰 차이점은 없다.

물론 동일한 환경에서 동일한 매트릭을 가져온 결과고, 단순히 DB 만 바꿨기 때문이다.

현재 수준에서는 DB의 성능 차이는 크게 영향을 미치지 않을 것이다.

<br>

다만 이번에는 원인을 분석하여 개선을 목표로 두고 있기 때문에

한 가지 데이터를 더 가져왔는데

![](/assets/img/projects/salog/monitoring_mysql/cpu_usage_win.png)

바로 윈도우 작업관리자에서의 cpu와 메모리 성능 모니터링이다.

크롬은 제외하고 샐로그 웹 애플리케이션이 실행 중인 인텔리제이를 보면 CPU 사용량이 73%에 육박하고 있으며 메모리 사용량도 그닥 낮은 수준은 아니다.

즉, 불안정하다는 의미다.

<br>

뿐만 아니라 앞선 그라파나 모니터링 결과를 보면 JVM 힙 메모리 사용량이 요청이 급증하자 수시로 날뛴다.

이는 메모리 누수가 발생했거나, GC가 자주 일어나고 있음을 나타내고 있는 것이다.

JVM 메모리 관리가 효율적이지 않다는 신호다.

<br>

특히 GC의 과도한 실행은 애플리케이션 성능에 영향을 준다.

GC는 메모리 관리의 일환으로 사용되지 않는 객체를 수집하여 메모리를 회수하는 작업을 수행하는데, 과도하게 실행되면 CPU 자원을 많이 잡아먹는다.

또한, GC가 애플리케이션의 실행을 일시 중지 시키는 "Stop-The-World" 이벤트를 발생 시킬 수 있기 때문에 애플리케이션의 응답 시간이 느려지거나 지연이 발생할 수 있다.

실제로 부하 테스트 시 JMeter는 요청을 계속 보내는데 인텔리제이 쪽에서 지연이 발생했다.

그래서 거의 2초까지 지연 시간이 발생하는 것이다.

<br>

["Stop-The-World" 이벤트 참고 블로그](https://velog.io/@limsubin/GC-stop-the-world-%EB%9E%80)

<br>

물론 이 뿐만 아니라 코드가 최적화되지 않아 연산에 시간이 오래 걸리는 것일 수도 있다.

그러나 아직까지의 지표로는 GC의 사용량이 크게 널뛰는 것을 보아 위와 같이 판단했다.

<br>

결국 배포 환경에서의 개선이 중요한 것인데, 현재는 AWS 무료 ec2를 빌려쓰고 있기 때문에 스케일-업은 꿈도 못 꾸고 최대한 논리적으로 문제를 해결해야 한다.

그러면 지금까지의 결과 값으로 봤을 때 웹 애플리케이션 성능 개선을 위해서는 GC 튜닝이 먼저라고 생각한다.

<br>

또한 "개요"에서 말한대로 DB 관련 성능 체크도 진행해보고 결정할 문제이긴 하다.

<br>
<br>
<br>

### 그라파나 비밀번호 초기화

이번 모니터링을 진행하면서 그라파나에 접근해보니 로그인 화면이 떴다.

원래 첫 admin/admin 으로 로그인하고 이후 비밀번호를 변경하지 않고 skip 했는데, 어째선지 같은 id/pw를 기입해도 로그인이 되질 않았다.

그래서 지웠다 깔아야하나 고민했는데, 대쉬보드 설정과 프로메테우스 연결 설정을 처음부터 다시 해야해서 손이 가질 않았다.

만약 추후에 팀이 함께 확인하는 대쉬보드가 있거나 아주 복잡한 설정을 거쳤는데 비밀번호 까먹었다고 바로 지웠다 깔기는 좀 그렇지 않나 싶었다.

그래서 최대한 찾아보았다.

<br>

처음에는 그라파나 설치 디렉토리로 접근해서 defaults.ini의 security 섹션의 디폴트 로그인 정보를 여러 번 바꾸면서 로그인을 시도해봤는데

여전히 로그인이 되질 않았다.

<br>

아래 블로그들을 참고하여 초기화하는 방법을 알아냈다.

[Nirsa's Learning Lab - Grafana 6.7 그라파나 admin 계정 패스워드 분실 시](https://nirsa.tistory.com/260)
[시스템 엔지니어의 세상 - Grafana 관리자 계정(Admin) 비밀번호 초기화하기](https://www.runit.cloud/2020/04/reset-grafana-admin-account-password.html)

<br>

우선 그라파나 db에 접근하여 쿼리를 사용해 admin 비밀번호를 초기화 해야하는데, 이를 위해서는 그라파나 db와 호환되는 형식의 db 프로그램이 필요했고,

그라파나는 SQLite 형식으로 저장되어 있기 때문에 SQLite3를 설치했다.

설치는 단순히 SQLite 홈페이지에 가서 설치하면 된다.

<br>

설치 후 cmd로

```
sqlite3 /path/to/grafana.db
```

위 명령어를 통해 sqlite3 기반으로 그라파나 db를 열었다.

이후 

```
UPDATE user SET password = '새로운 비밀번호' WHERE login = 'admin';
```

이와 같이 단순히 쿼리를 사용하여 비밀번호를 변경해 주면 되는데

여기서 문제는 평문으로 저장하면 안된다.

그라파나에서는 사용자 비밀번호를 해시 형태로 저장한다. 즉, 단순히 비밀번호를 문자열로 저장하는 것이 아니라 보안을 위해 해시 함수를 사용해 암호화한다.

<br>

```
update user set password = '59acf18b94d7eb0694c61e60ce44c110c7a683ac6a8f09580d626f90f4a242000746579358d77dd9e570e83fa24faa88a8a6', salt = 'F3FAxVm33R' where login = 'admin';
```

이와 같이 SHA256 알고리즘으로 생성된 해시로 비밀번호를 변경해야하며 뒤의 salt는 해시를 더욱 안전하게 만드는 추가 값이다.

위 해시를 평문으로 변환하면 "admin" 이다.

<br>

이 과정을 통해 그라파나 admin 비밀번호를 변경했고 로그인에 성공했다.

<br>
<br>
<br>

### 후기

개선을 목표로 두고 진행했기 때문인지 앱 성능 저하의 원인을 어느정도 파악할 수 있었다.

이제 다음 포스트에서는 mysql exporter를 추가하여 프로메테우스로 db 관련 매트릭을 수집하고 다시 분석을 진행해 볼 예정이다.

그 이후에는 몇 가지 전략을 정해 개선 작업을 해보고 이후 배포 환경에 적용해보자.
