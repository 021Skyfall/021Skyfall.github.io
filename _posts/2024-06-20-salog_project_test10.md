---
title: "프로젝트:샐로그 / 모니터링 - 프로메테우스와 그라파나 적용"
author: Jeremiah Lee
date: 2024-06-20
categories: [ 프로젝트, 샐로그, 모니터링 ]
tags: [프로젝트, 샐로그, 모니터링, 프로메테우스]
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

[지난 번](https://021skyfall.github.io/posts/SW_test_4/)에 이어서 프로메테우스와 그라파나에 대한 내용을 알아냈으니 이번에는 적용해볼 차례다.

<br>

우선 생각보다 사용 자체는 복잡하지 않고 단순히 파일을 다운로드 받고 사용만 하면 되었다.

물론 몇 가지 설정 파일에 들어갈 내용이 있기는 하지만 그리 복잡하지는 않았다.

<br>

한 가지 유의할 점은 모든 작업은 Windows 환경(GUI)에서 진행했으며, 실제 배포 환경이 아닌 로컬(8080포트)이다.

<br>
<br>
<br>

### 전체적인 흐름

기본적으로
프로메테우스를 적용하고 샐로그 애플리케이션에 의존성을 추가하여 메트릭을 수집할 수 있도록 yml 파일을 수정하고
프로메테우스를 작동시켜서 설정한 간격으로 메트릭을 수집시킨다.

그러면 특정 API로 메트릭이 수집되고, 프로메테우스에서 지정된 API로 접근하면 프로메테우스 대쉬보드가 열린다.

해당 대쉬보드로 메트릭이 정상적으로 수집되고 있는 지 체크할 수 있고 쿼리(promQL)를 사용해 원하는 정보를 열람할 수 있다.

<br>

메트릭을 수집하여 모니터링하고자 하는 애플리케이션과의 연결이 up 상태라면 정상적인 것으로 판단하고
간단한 쿼리를 몇 개 사용해보면 된다.

이후 정상적으로 동작 중이라면 다음은 시각화 도구를 사용해 볼 차례인 것이다.

<br>

시각화 도구는 그라파나를 사용했으며 마찬가지로 그라파나를 실행하여 메트릭을 수집 중인 프로메테우스와 연결하면
대쉬보드를 생성하여 원하는 패널로 수집 중인 메트릭을 그래프화 할 수 있다.

주요 지표를 시각적으로 보기 편하게 설정하고 나면 모니터링 준비는 완료된다.

<br>

이제 프로메테우스, 그라파나를 어떻게 적용했는지 순서대로 살펴보자.

<br>
<br>
<br>

### 프로메테우스 적용

앞서 말한 대로 전체적인 흐름이 이어지는데, 추후 또 다른 애플리케이션이나 배포 상황인 샐로그에 적용할 수 있기 때문에
프로메테우스와 그라파나가 각각 어떻게 연결되었는지 확인해보자.

단순히 "사용"해본 것이고 복잡한 설정을 거치지 않았으며, 주로 HTTP 요청 관련, CPU 사용량, 디스크 I/O, 메모리 사용량 등을 확인했으며 이후 시각화했다.

<br>

이제 순서대로 적용 방법을 알아보자.

#### 1. 프로메테우스 설치

프로메테우스를 시작하기 위해서는 당연히 프로메테우스를 설치해야 한다.

그런데 별도로 설치 파일로 설치하지 않아도 zip 파일을 받아 압축 해제한 뒤 prometheus.exe 파일을 실행하기만 하면 된다.

설치는 [프로메테우스 공식 홈페이지 (다운로드)](https://prometheus.io/download/)에서 환경에 맞는 파일을 받아서 진행하면 된다.

<br>
<br>

#### 2. 프로메테우스 설정(yml) 파일 수정

압축 해제를 하고 나면 

![](/assets/img/projects/salog/monitoring/prometheus_directory.png)

위와 같은 폴더와 파일들이 있는데, 여기서 바로 exe 파일을 실행하면 안되고 설정 파일인 yml 파일을 텍스트 에디터로 열어 내용을 수정해야한다.

<br>

기본적으로 여타 yml 파일이 그렇듯 각 설정은 뎁스로 구분된다.

이 설정 파일에서 메트릭을 수집하는 간격을 설정할 수 있으며 alertmanger의 타겟 포트나 메트릭을 수집할 타겟 포트 등을 설정해서 실행 중인 애플리케이션과 연결할 수 있다.

```yml
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"
    metrics_path: '/actuator/prometheus'

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.
    
    scrape_interval: 5s  # 이 잡에 대한 스크레이프 간격 (기본값을 덮어씀)

    static_configs:
      - targets: ["localhost:8080"] # 모니터링할 애플리케이션의 주소와 포트
```

위와 같이 작업 이름과 메트릭을 수집할 경로, 타겟의 주소와 포트를 설정하고 저장하면 된다.

<br>
<br>

#### 3. 웹 애플리케이션 의존성과 설정(yml) 파일 수정

프로메테우스 yml 파일을 수정했다고 해서 끝난 것이 아니다.

우리의 웹 애플리케이션에서도 별도의 의존성을 추가하고 설정을 수정해야만 프로메테우스 yml에서 지정한 경로로 메트릭을 수집할 수 있다.

<br>

메트릭을 수집할 수 있도록 설정하기 위해 의존성 추가부터 살펴보자.

```
dependencies {
...
	// 모니터링 (프로메테우스)
	implementation 'io.micrometer:micrometer-registry-prometheus'
	implementation 'org.springframework.boot:spring-boot-starter-actuator'
}
```

이 두가지를 추가하면 된다. 

micrometer는 애플리케이션의 메트릭을 수집하고 관리하는 메트릭 수집기이다.

micrometer에서 제공하는 프로메테우스 레지스트리 구현제이다.

그 다음의 Spring Boot Actuator는 애플리케이션의 상태를 모니터링하고 관리할 수 잇는 기능을 제공한다.

이 의존성을 추가하면 애플리케이션의 다양한 정보(헬스 체크, 메트릭, 환경 설정 등)를 HTTP 엔드포인트를 통해 확인할 수 있다.

<br>

처음에는 액추에이터에 관련된 내용을 몰라 좀 헤맸는데

아래의 블로그에서 방법을 찾아 해결했다.

[Spring Boot Actuator, Prometheus, Grafana를 사용한 스프링부트 모니터링 환경 구축](https://hudi.blog/spring-boot-actuator-prometheus-grafana-set-up/)

<br>

정리하자면 프로메테우스에서 사용되는 메트릭을 수집할 때 Micrometer 의존성이 필요하고,
메트릭 수집 엔드포인트를 actuator로 노출 시켜 프로메테우스가 메트릭을 수집할 수 있도록 하는 것이다.

<br>

다음으로는 애플리케이션의 설정 파일을 수정해야하는데,

```yml
# 모니터링
management:
  endpoints:
    web:
      exposure:
        include: prometheus
  metrics:
    export:
      prometheus:
        enabled: true
```

뎁스 그대로 해당 내용을 추가하면 된다.

윗 부분은 prometheus 엔드포인트를 웹 상에 노출시킨다는 의미이고, 아래는 프로메테우스 메트릭 데이터 exporter를 활성화 시킨다는 설정이다.

추가적으로

```yml
...
  metrics:
    export:
      prometheus:
        enabled: true
        step: 1m
```

아래의 step 옵션으로 특정 시간 단위로 메트릭을 내보낼 수 있게 설정할 수 있다.

<br>
<br>

### 4. 프로메테우스 실행 / 대쉬보드

이제 프로메테우스를 실행해보자.

프롬프트가 하나 열리고 메트릭 수집이 시작되며, 애플리케이션에서는 메트릭 수집 엔트포인트로의 요청이 로그에 남기 시작한다.

사실 뭔가 문제가 있어 메트릭 수집 엔드포인트로 접근되지 않는다면 애플리케이션 로그에 남긴하지만 더 확실하게 확인할 수 있는 방법은 프로메테우스 대쉬보드에 접근해서 확인해보는 방법이다.

<br>

프로메테우스 대쉬보드의 기본 포트는 9090이기 때문에 http://localhost:9090/ 으로 접근하면 프로메테우스 대쉬보드가 노출된다.

![](/assets/img/projects/salog/monitoring/prometheus_dashboard.png)

위와 같은 대쉬보드가 있는데, 우선 메트릭 수집 엔드포인트와 연결이 되어서 메트릭이 수집 중인지 확인하고자 한다면    
상단의 Status 탭에서 Targets 로 접근하면 타겟과 연결되어 있는 지 확인할 수 있다.

![](/assets/img/projects/salog/monitoring/prometheus_dashboard_targets.png)

위와 같은 화면에서 연결이 실패했다면 Status가 DOWN 상태이고 성공했다면 UP 상태이다.

현재는 UP 인 상태로 메트릭을 수집하고 있는 것이다.

수집 중인 메트릭에 관련된 자세한 내용이 궁금하다면 엔드포인트로 접근해보면 된다.

<br>
<br>

### 5. 프로메테우스 쿼리 (promQL)

프로메테우스는 기본적으로 promQL 이라고 하는 독자적인 쿼리 언어를 사용하여 수집 중인 메트릭에 대해 쿼리해 볼 수 있다.

실제로 간단한 쿼리 몇 가지를 사용해 보았다.

```
sum(http_server_requests_seconds_count{uri="/members/signup"}) : 특정 경로에 대한 HTTP 요청 수를 확인
rate(http_server_requests_seconds_sum{uri="/members/signup"}[5m]) / rate(http_server_requests_seconds_count{uri="/members/signup"}[5m]) : 특정 경로에 대한 HTTP 요청의 평균 응답 시간을 확인
sum by (status) (http_server_requests_seconds_count) : 상태 코드별로 HTTP 요청 수를 확인
rate(http_server_requests_seconds_count[5m]) : 최근 5분 동안의 HTTP 요청 수를 확인
jvm_memory_used_bytes : JVM 메모리 사용량을 확인
system_cpu_usage : 현재 시스템 CPU 사용률
```

주로 요청과 cpu, 메모리 등에 대한 쿼리를 사용해보았으며 정상적으로 접근이 되었다.

블로그를 작성하는 현시점에는 JMeter로 요청을 아직 보내지 않아서 쿼리 결과가 없기 때문에 사용한 쿼리가 무엇인지만 작성했다.

<br>
<br>
<br>

### 그라파나 적용

이제 프로메테우스를 연결하여 수집된 메트릭에 대한 쿼리도 해보았으나 단순히 텍스트로는 보기 어렵다는 것을 알았다.

그렇기 때문에 직관적으로 모니터링 내용을 확인할 수 있도록 이번에는 그라파나를 적용해보자.

<br>
<br>

#### 1. 그라파나 설치

우선 각 환경에 맞게 그라파나를 다운로드 해보자.

다운로드는 [그라파나 공식 홈페이지 - 다운로드](https://grafana.com/grafana/download?platform=windows)에서 할 수 있다.

인스톨러도 있기는 한데 나는 바이너리를 다운 받아 사용했다.

<br>

가장 최신 버전인 11.1.0 버전을 받았으며 내용물은 다음과 같다.

![](/assets/img/projects/salog/monitoring/grafana_directory.png)

여기서 bin에 접근한 다음 grafana-server.exe 파일을 실행하면 그라파나가 실행된다.

<br>
<br>

#### 2. 그라파나 접속

그라파나가 열려있는 기본 포트는 3000번 포트이다.

실행된 프롬프트에서 확인할 수 있다.

<br>

처음 접속하면 로그인을 먼저 거치게끔 되어있는데

기본 id와 pw는 둘다 "admin" 이다.

이후 pw를 변경할 수도 있고 아님 skip 할 수도 있다.

이후부터는 실행하면 자동 로그인된다.

<br>

![](/assets/img/projects/salog/monitoring/grafana_page.png)

접속하고 나면 위와 같은 페이지가 반겨준다.

<br>
<br>

#### 3. 그라파나 - 프로메테우스 연결

이제 현재 열려있는 프로메테우스와 연결을 해야 이후 작업을 시작할 수 있다.

좌측 상단에 단추를 누르면 아래와 같은 탭이 열린다.

![](/assets/img/projects/salog/monitoring/grafana_tabspng.png)

여기서 Connections -> Data sources에 접근하면 시각화할 데이터 소스를 설정할 수 있다.

![](/assets/img/projects/salog/monitoring/grafana_datasources.png)

위 화면에서 우측 상단에 + Add new data source로 접근하면 원하는 소스를 추가할 수 있으며
시계열 데이터 뿐만 아니라 여러가지 종류를 지원한다.

현재는 프로메테우스와 연결해야하기 때문에 프로메테우스를 선택한다.

![](/assets/img/projects/salog/monitoring/grafana_datasources2.png)

선택하면 위와 같은 화면이 펼쳐지는데, 여기서 이름은 임의로 하고 Connection 파트에서 프로메테우스의 주소를 넣어주면 된다.

유의할 것은 메트릭이 수집되고 있는 엔드포인트가 아닌 프로메테우스의 자체 엔트포인트이다.

현재는 저기 나와있는대로 기본 주소인 localhost:9090 으로 연결했다.

이후 추가적인 설정은 넘어가고 최하단의 Save & test 단추를 누르면 연결을 테스트 해본 다음 저장된다.

<br>
<br>

#### 4. 그라파나 대쉬보드

이제 메트릭을 수집하고 있는 프로메테우스와 연결이 끝났으니
다음은 본격적인 시각화 차례이다.

아까의

![](/assets/img/projects/salog/monitoring/grafana_tabspng.png)

탭에서 대쉬보드로 접근해보자.

![](/assets/img/projects/salog/monitoring/grafana_dashboards.png)

그러면 이와같은 화면이 뜨는데, 현재 salog 대쉬보드는 내가 만들어논 것이고 기본적으로 우상단의 new 탭에서 하나 제작하면 된다.

처음에는 빈 화면이 보이는데, 상단의 add 탭을 클릭해서 Visualization으로 접근하면

![](/assets/img/projects/salog/monitoring/grafana_new_pannels.png)

이와 같이 패널을 생성하는 창이 보일 것이다.

이 패널은 프로메테우스를 사용한다는 가정하에 기본적으로 promQL을 사용하여 쿼리를 날린 결과를 시각적으로 표현해주는데,

![](/assets/img/projects/salog/monitoring/grafana_pannels_query.png)

여기서 해당 내용을 설정할 수 있다.

데이터소스는 당연히 아까 생성한 프로메테우스를 지정해주고, 탭을 열어 Metric 항목에 쿼리를 작성하면 된다.

어떤 쿼리를 사용해야할 지 잘 모르겠으면

해당 탭을 누르면

![](/assets/img/projects/salog/monitoring/grafana_pannels_query2.png)

이렇게 Metrics explorer 에 접근할 수 있고 열어보면

![](/assets/img/projects/salog/monitoring/grafana_pannels_query3.png)

수집 중인 모든 메트릭을 확인 할 수 있다.

여기서 어떤 수집 도구를 사용하느냐, 혹은 액추에이터를 사용하느냐 등에 따라 메트릭의 이름이 다를 수 있기 때문에 주의해야 한다.

<br>

이런 식으로 원하는 쿼리를 활용해 패널들을 만들고 나면

![](/assets/img/projects/salog/monitoring/grafana_dashboards2.png)

이렇게 내가 원하는 지표를 시각화할 수 있게 된다.

현재는 "대규모 트래픽 테스트" 컨셉에 맞춰 유의미한 지표를 설정해 두었으며 이외에도 여러 메트릭을 활용해 시각화 할 수 있다.

또한, 아까의 패널 설정 페이지에서 우측의 스타일 시트를 활용해 원하는 모양으로 제작도 가능하다.

<br>
<br>
<br>

### 추후 계획

이제 프로메테우스 모니터링 도구를 적용하여 그라파나를 활용해 시각화도 해보았다.

위의 최종 대쉬보드는 JMeter로 몇 가지 요청을 다수 보내본 결과가 담겨있다.

현재는 H2 데이터베이스를 활용해 테스트 해본 것이고, MySQL 데이터베이스를 활용해 2차전을 할까 생각했다.

<br>

다음으로는 조회 기능에 초점을 맞춰 로컬에서 MySQL DB를 적용하고, 부하 테스트를 진행해보고 개선해보려 한다.

배포 환경에서 직접적으로 테스트, 모니터링, 개선 작업을 진행하려 했는데, 실 서비스 중인 애플리케이션에 부하를 가하거나 한다면 문제가 생길 여지가 있어
로컬 환경에서 배포 환경과 같은 Mysql db 를 적용하고, 개선 작업을 진행하는 것이 맞다고 생각한다.

테스트 브랜치를 별도로 분리하여 MySQL DB와 연결한 다음 부하를 주고 개선 작업을 해볼 예정이며,
추후 개선된 사항을 배포 환경에 적용할 계획이다.

<br>
<br>
<br>

***

### 후기

모니터링 툴들을 적용하고 실사용해보는 시간이 좀 길어졌는데
생각보다 엄청 어렵진 않아서 다행이었다.

현재 마지막 모니터링 사진을 보면 요청 지연 시간이 2~3초 대인 것들이 대부분 조회 쪽에서 걸린 시간이기 때문에
개선 작업을 조회에 초점을 맞춰 진행해보려 한다.
