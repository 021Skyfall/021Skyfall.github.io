---
title: "프로젝트:샐로그 / 성능 테스트 - MySQL Exporter 추가 / 분석"
author: Jeremiah Lee
date: 2024-07-16
categories: [ 프로젝트, 샐로그, MySQL Exporter, 분석 ]
tags: [프로젝트, 샐로그, MySQL Exporter, 분석]
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

저번 포스트에서 언급했듯이 지금까지의 매트릭 중에는 DB 관련, 쿼리 관련 매트릭이 없었다.

그렇기 때문에 GC의 문제점 이외에도 별개의 문제가 존재할 가능성을 확인할 수 없었는데, 이 기회에 MySQL Exporter를 설치하고 별도로 DB 매트릭을 수집함으로써
DB 구조나 쿼리에 문제가 있는지 확인할 수 있게 되었다.

이외에도 쿼리 로그를 출력하여 분석할 수 있으므로 어떤 개선을 거쳐야하는 지 보다 정확하게 분석할 수 있다.

<br>

이번 포스트에서는 DB 관련 매트릭 수집, 시각화 후 결과와 해당 문제를 분석하여 적합한 해결법을 도출해내는 과정을 작성한다.

<br>
<br>
<br>

### MySQL Exporter

프로메테우스에서 기본으로 제공하지 않는 매트릭을 수집하기 위해서는 다양한 Exporter가 필요하다.

이 Exporter를 연결해서 특정 매트릭을 수집할 수 있는데, 지금은 MySQL 통신 매트릭을 수집해야하기 때문에
MySQL Exporter를 설치하고 연결해주면 된다.

<br>

설치는 Windows 10 환경에서 진행되었다.

<br>

#### MySQL Exporter 설치

우선 이 Exporter를 설치하기 위해서 프로메테우스가 제공하는 깃헙 레포로 접근해야한다.    

[프로메테우스-MySQL_exporter 깃헙 레포지터리](https://github.com/prometheus/mysqld_exporter/releases/tag/v0.15.1)     

여기서 아래의 mysqld_exporter-0.15.1.windows-amd64.zip 를 다운 받으면 된다.

<br>

#### MySQL Exporter 설정

압축을 해제하면 실행 파일과 라이센스 등이 있다.

이 실행 파일을 통해 MySQL Exporter 를 실행하여 매트릭을 수집하는 것인데,
우선은 ini 파일로 관련 설정을 작성해주어야 한다.

해당 파일은

```
[client]
user=exporter
password={mysql-비밀번호}
host=localhost
port=3306
```

이런 식으로 작성하면 되며 매트릭을 수집할 mysql 의 계정과 패스워드, 필요하다면 호스트명과 포트를 작성하면 된다.

<br>

#### MySQL Exporter ini 파일 작성 시 주의

다음으로 넘어가기 전에 주의해야할 것이 있다.

MySQL 계정의 비밀번호가 특수문자인 경우가 있는데, 
이 경우 ini 파일에 MySQL 비밀번호로 특수문자를 입력하고 실행하면 계정으로 접근이 되지 않는다.

Exporter 가 Mysql로 접근하기 위해 비밀번호를 읽어들일 때 특수문자는 인식이 제대로 되지 않는 것 같다.

계속

```
"Error 1045 (28000): Access denied for user 'exporter'@'localhost' (using password: YES)"
```

이런 에러가 발생해서 계정을 다시 만들어보고, 비밀번호를 바꿔보고 MySQL 에서 로그인 해보고 등등 여러 시도를 해봤는데 안되다가
혹시 몰라 특수문자를 빼고 다시 실행해보니 잘 되었다.

고로 MySQL Exporter와 연결할 MySQL 계정의 비밀번호에는 특수문자를 사용하면 안된다.

<br>

#### Prometheus 설정

다음으로는 이 Exporter 와 연결하기 위해 프로메테우스의 설정을 추가해야 한다.

우선 프로메테우스의 설정 파일인 yml 파일에 접근해서 아래의 설정을 추가한다.

```
scrape_configs:

  ...
      
    # MySQL Exporter 설정 추가
  - job_name: 'mysql'
    static_configs:
      - targets: ['localhost:9104']  # MySQL Exporter의 주소와 포트
```

여기서 9104 포트는 MySQL Exporter의 기본 포트이다.

<br>

#### MySQL Exporter 실행

이제 MySQL Exporter 를 실행하면 프로메테우스와 연결이 되어 mysql 관련 매트릭을 수집할 수 있다.

윈도우 기준으로는 cmd를 켜서 

```
mysqld_exporter --config.my-cnf=mysqld_exporter.ini
```

이 명령어를 통해 작성한 ini 파일을 토대로 exporter를 실행시킬 수 있다.

참고로 cmd로 실행시키기 위해 exporter 위치를 환경변수에 추가 해주어야 한다.

<br>

실행하고 나면 

![](/assets/img/projects/salog/monitoring_mysql/mysqld_exporter_prometheus.png)

이와 같이 프로메테우스의 target으로 잡혀있다.

<br>
<br>
<br>

### 프로메테우스로 확인해야할 db 지표

이제 프로메테우스로 mysql 관련 매트릭을 수집할 수 있다.

프로젝트 개선을 위해 확인할 사항과 이유는 다음과 같다.

#### 1. Slow queries
  - mysql_global_status_slow_queries
  - 느린 쿼리가 많으면 DB의 처리 성능이 떨어질 수 있다.
  - 이 쿼리를 사용하기 위해서는 MySQL에서 슬로우 쿼리 로그를 설정 해야한다.

#### 2. 쿼리 성능
  - mysql_global_status_questions
  - 쿼리 처리 수를 모니터링하고 갑작스러운 부하를 파악할 수 있다.
  - 요청이 급증하는 상황에서 DB의 처리 성능을 추적할 수 있다.

<br>

#### MySQL 슬로우 쿼리 로그 활성화

우선 시각화를 위해 새로운 패널을 생성하기 전에 슬로우 쿼리를 위해 mysql에서 슬로우 쿼리 로그를 활성화해야 한다.

가장 먼저 mysql에 설정 파일을 적용해야한다.

mysql이 설치된 디렉토리로 이동해 my.ini 파일을 생성하고
아래의 내용을 삽입하면 된다.

```
[mysqld]
slow_query_log = 1 # 슬로우 쿼리 로그 활성화
slow_query_log_file = C:\ProgramData\MySQL\MySQL Server 8.0\mysql-slow.log # 로그 파일 생성 경로
long_query_time = 1 # ~초 이상 걸리는 쿼리 기록
```

이와 같이 설정한 후 mysql 을 실행하여 

```
SHOW VARIABLES LIKE 'slow_query_log';
```

이 쿼리를 입력하면

```
mysql> SHOW VARIABLES LIKE 'slow_query_log';
+----------------+-------+
| Variable_name  | Value |
+----------------+-------+
| slow_query_log | ON    |
+----------------+-------+
1 row in set, 1 warning (0.00 sec)
```

이와 같이 슬로우 쿼리 로그가 활성화 되었는 지 확인할 수 있다.

<br>

이제 이 설정을 통해 mysql 에서 1초 이상 걸리는 쿼리를 기록하고, mysql exporter 가 해당 쿼리를 기록, 수집하여 프로메테우스에서 확인이 가능해졌다.

<br>
<br>
<br>

### 문제해결 - 슬로우 쿼리 설정

위와 같이 슬로우 쿼리를 설정하고서 그라파나 패널을 수정하던 중 문제를 발견했다.

![](/assets/img/projects/salog/monitoring_mysql/slow_query_problem.png)

이렇게 슬로우 쿼리 수를 패널로 생성하여 확인했는데 0만 출력되었다.

1초로 설정해놨기 때문에 응답하는 데 2초 넘게 걸리는 요청에 대한 쿼리가 1초가 넘을리가 없다고 생각이 들어 이상하다고 느꼈다.

그래서 mysql로 돌아가

```
mysql> SHOW VARIABLES LIKE 'long_query_time';
```

이 쿼리를 날려보니

```
+-----------------+-----------+
| Variable_name   | Value     |
+-----------------+-----------+
| long_query_time | 10.000000 |
+-----------------+-----------+
1 row in set, 1 warning (0.00 sec)
```

내가 설정하지 않은 10초가 설정되어 있었다.

그러면 내가 삽입한 설정 파일을 읽어들이는 것이 아니라 다른 것을 읽고 있다는 건데,

```
mysql> SHOW VARIABLES LIKE 'pid_file';
```

아니나 다를까 이 쿼리를 날려 현재 읽고 있는 설정 파일을 확인 했더니

```
+---------------+--------------------------------------------------------+
| Variable_name | Value                                                  |
+---------------+--------------------------------------------------------+
| pid_file      | C:\ProgramData\MySQL\MySQL Server 8.0\Data\Jupiter.pid |
+---------------+--------------------------------------------------------+
1 row in set, 1 warning (0.00 sec)
```

영 엉뚱한 설정 파일을 읽고 있었다.

그래서 해당 디렉토리로 접근해보니 my.ini 파일이 있었고
내가 ProgramData로 접근했어야 했는데 ProgramFiles 로 접근해서 설정 파일을 못 찾았다는 것을 깨달았다.

그래서 해당 파일로 다시 설정하고 이전 파일을 삭제했다...

<br>

그 다음 윈도우 서비스 services.msc 로 접근해서 mysql80을 재시작하고

```
mysql> SHOW VARIABLES LIKE 'long_query_time';
+-----------------+----------+
| Variable_name   | Value    |
+-----------------+----------+
| long_query_time | 1.000000 |
+-----------------+----------+
1 row in set, 1 warning (0.01 sec)
```

슬로우 쿼리 시간 변경을 확인했으며

```
mysql> select sleep(2);
+----------+
| sleep(2) |
+----------+
|        0 |
+----------+
1 row in set (2.00 sec)
```

이와 같이 일부러 2초 걸리는 쿼리를 입력했을 때

```
# Query_time: 1.999633  Lock_time: 0.000000 Rows_sent: 1  Rows_examined: 1
SET timestamp=1726476064;
select sleep(2);
```

로그가 정상적으로 저장되는 것도 확인했다.

이로써 해당 문제를 해결했다.

<br>
<br>
<br>

### 모니터링 결과와 분석

새로운 패널을 생성하는 과정은 [이전 포스트](https://021skyfall.github.io/posts/salog_project_test10/)에서 확인하면 된다.

바로 계속된 부하 테스트와 동일한 환경, 동일한 조건으로 진행해 본 결과를 보자.

![](/assets/img/projects/salog/monitoring_mysql/mysql_salog_monitoring_result.png)

자주 봤던 그라파나 모니터링이지만 MySQL Exporter로 수집한 매트릭을 시각화한 두 가지 패널이 추가 됐다.

쿼리에 문제가 있는 지 파악하기 위해 전체 쿼리 수와 슬로우 쿼리 수를 가져왔으며, 둘의 비율을 따질까 하다가 그냥 분리해 두었다.

<br>

우선, 전체 쿼리 수에 비해 슬로우 쿼리 수가 현저히 낮은 것을 보아 쿼리나 DB 자체에는 문제가 없는 것으로 판단된다.

그리고, 요청이 몰렸던 시간에 눈에 띄는 것은 JVM 힙 메모리 사용량의 변동폭이 큰 것과 요청 지연 시간이 한 번 씩 크게 튀는 것,
그리고 CPU 사용량의 증가폭이다.

한 가지 알 수 있는 것은 요청 지연 시간이 크게 늘어날 때는 CPU 사용량과 JVM 힙 메모리 사용량이 낮은데 요청 지연 시간이 떨어지면서 CPU 사용량과 힙 메모리 사용량이 
크게 증가한다.

이를 나름대로 분석해본 결과

1. 초기 요청이 폭증하면서 시스템이 처리하는 것 이상으로 요청이 들어온다.
2. 이로 인해 병목 현상이 발생하고 요청 지연 시간이 증가한다.
3. 요청 처리 중 메모리 사용량이 증가하면서 GC가 빈번히 실행된다.
4. 이 과정에서 CPU 자원을 할당 받아 GC가 수행된다.
5. GC가 완료될 때마다 메모리가 확보되고, 요청 지연 시간이 감소한다.

로 결론이 난다.

1,2 번으로 인해 메모리가 부족해지고, 3번이 진행된다.

이는, 요청 폭증 극초반 이후부터 CPU와 힙 메모리 사용량의 변화에서 확인할 수 있다.

GC가 완료될 때마다 요청 지연 시간이 감소하는 것을 확인할 수 있었다.

마지막으로 메모리가 부족하지 않아 GC가 실행되지 않고, 약간의 병목현상 이후 요청 처리가 완료된다.

<br>

이제 이를 기반으로 개선을 진행해보고 다시 동일한 조건과 환경에서 테스트를 진행해 볼 것이다.

<br>
<br>
<br>

### 개선 계획

결과적으로 슬로우 쿼리는 많이 발생하지 않았기 때문에 쿼리 자체에 문제가 있다기 보다는 요청이 들어올 때 병목 현상이 발생하는 것으로 보인다.

이 경우에 가장 효과적이고 간단한 방법은 메모리 할당 크기를 늘리거나 스케일업 하는 것인데,
문제는 현재 AWS 무료 플랜을 사용 중이기 때문에 상당히 상황이 제한적이다.

그러면 논리적으로 어느 정도 개선을 해야하는데,
그러기 위해 내가 생각한 전략은 세 가지이다.

##### 1. GC 튜닝
  - GC 알고리즘을 변경하여 메모리 관리의 효율성을 향상 시킨다.
  - GC의 빈도를 조절하여 메모리를 확보한다.

##### 2. DB 인덱싱
  - 결과적으로 쿼리 자체에 문제가 없는 것 같아도 where 절이 붙은 쿼리이기 때문에 요청 처리 시간을 단축 시킬 수 있을 것이라 판단 했다.

##### 3. 캐싱 전략
  - 주로 날짜 별 수입을 조회하는 것이기 때문에 이 데이터를 캐싱하여 응답 속도를 높인다.
  - Redis를 활용한다.

이는 보편적으로 애플리케이션의 성능을 향상 시키기 위한 전략 들이며
위 전략 들을 통해 나도 샐로그의 요청 지연 시간을 줄여보고자 한다.

<br>

순서대로 진행한 뒤, 각 결과를 확인하고 어떤 전략이 효과적이었는 지 다음 포스트에 작성한다.

<br>
<br>
<br>

### 후기

지속적으로 모니터링과 부하 테스트를 진행해봤는데, 개선에 있어서는 어떤 것이 정답이라고 확정하기가 힘들다.

조회니까 대충 쿼리가 문제겠지? 라고 생각했는데, 오히려 슬로우 쿼리가 발생하지 않는 걸 보니 그건 또 아니고

모니터링 해본 결과로 GC의 빈번한 실행 때문이라고 생각했는데
오히려 GC 실행되어야 요청 지연 시간이 줄어든다.

그래도 앱의 성능을 끌어올리기 위해 주로 사용되는 전략을 적용해보고 결과를 확인해봐야 느낌이 올 것 같아서 진행해보려 한다.

<br>

대부분 문제는 사실 스케일업하면 해결되기야 할테지만 최대한 적은 비용으로 최대한의 성능을 끌어올릴 수 있어야 한다고 생각한다.
