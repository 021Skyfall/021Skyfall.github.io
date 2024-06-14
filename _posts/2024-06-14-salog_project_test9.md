---
title: "프로젝트:샐로그 / 성능 테스트 - JMeter 사용법과 부하 테스트"
author: Jeremiah Lee
date: 2024-06-14
categories: [ 프로젝트, 샐로그, 성능 테스트, JMeter ]
tags: [프로젝트, 샐로그, 성능 테스트, JMeter]
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

직전 성능 테스트 도구에 대한 포스트를 진행하고 바로 샐로그 프로젝트에 해당 도구를 사용해서 테스트를 진행해 보았다.

우선 부하 테스트를 진행했는데, 모니터링 없이 부하만 주어보았다.

<br>

이는 대규모 트래픽에 대한 테스트라 볼 수 있는데,
사실 이 "대규모 트래픽"이라는 것에 대한 기준이 애매하다.

어디서는 1만 건의 요청이 10초 이내에 들어오면 대규모 트래픽이라고 볼 수도 있고,
또 다른 데서는 100만 건의 요청이 들어와야 대규모 트래픽이라고 할 수도 있다.

즉, 서비스 마다 상대적인 것이기 때문에 실제 사용자가 없는 샐로그 프로젝트에서 어떤 것을 기준 잡고 진행해야할 지 애매했다.

<br>

최대한 현실적으로 생각해서 요청을 받았을 때의 안정성을 테스트했다.

<br>
<br>
<br>

### JMeter

JMeter 는 기본적으로 부하를 주는 도구일 뿐이다.

정확하게 분석을 하기 위해서는 별도로 모니터링 도구가 필요하고, 시각적으로 편하기 위해서는 또 별도의 도구가 필요하다.

각각 프로메테우스와 그라파나를 적용해 볼 예정이지만, 우선은 JMeter 를 사용해 부하를 줘보았기 때문에 이 도구에 대한 사용법 부터 알고가자.

<br>
<br>
<br>

### JMeter 설치

가장 먼저 진행해야할 것은 설치다.

[JMeter 공식 홈페이지](https://jmeter.apache.org/)에서 [설치 페이지](https://jmeter.apache.org/download_jmeter.cgi)
로 접속하여 최신 버전을 다운로드 받아서 사용하면 된다.

윈도우10 환경에서 진행되기 때문에 zip 파일을 받아서 사용하면 된다.

현재 최신 버전은 5.6.3 버전이다.

<br>

다운로드가 완료되면 zip 파일을 압축해제하여 bin 디렉토리의 JMeter.bat 을 실행시켜 사용하면 된다.

<br>
<br>
<br>

### 테스트 환경

JMeter 를 사용해 프로젝트에 부하를 주기 위해 테스트 환경을 조성해야 한다.

아무리 실질적인 데이터가 필요하더라도 실제 배포되어 있는 서버에 부하를 주면 안된다.

당연한 얘기지만 실제 배포된 애플리케이션에 갑작스런 부하를 주면 어떤 부작용이 있을 지 모르고, 사용자가 불편함을 느낄 수 있기 때문이다.

<br>

그렇기 때문에 모든 테스트는 별도의 테스트 환경에서 진행되어야 하는데, 거창하게 생각하지 않고 단순히 로컬에서 진행하면 된다.

물론 실제 환경과 유사하게 AWS EC2를 하나 더 만들어서 진행해보면 좋겠지만 이제 계정도 없고 돈도 없다.

그래서 로컬에서 개발용 프로필을 적용해 테스트를 진행했는데, 추후 모니터링 도구를 적용하고 나면 별도의 프로필을 생성하여 최대한 실제와 비슷하게 
로컬 MySQL을 연결해서 다시 한번 테스트를 진행해볼 예정이다.

이에 대한 포스트는 모니터링 도구 적용 과정에서 서술할 것이다.

<br>
<br>
<br>

### JMeter 사용법 - 세팅

JMeter를 처음 실행하면 아래와 같은 화면이 열린다.

![](/assets/img/projects/salog/jmeter_first_exec.png)

이 화면에서 좌측 네비게이터의 Test Plan을 우클릭 > Add > Threads (Users) > Thread Group 을 선택하여 
스레드 그룹을 만들고, 이 것이 각각의 테스트 시나리오가 된다.

<br>

![](/assets/img/projects/salog/jemeter_thread_groups.png)

이렇게 필요한 스레드 그룹을 만들고 나면 이제 테스트를 진행하기 위한 세팅을 진행해야한다.

각 스레드 그룹을 눌러보면

<br>

![](/assets/img/projects/salog/jmeter_thread_group_setting.png)

이런 식으로 내용이 보이는데, 에러가 발생하면 어떻게 행동할지 지정하는 선택지 이후

스레드 속성을 보면 3가지 기입해야하는 항목이 있다.

이 항목은 각각

1. Number of Threads (users) : 동시 사용자 수
2. Ramp-Up Period (seconds) : 요청을 보내는 시간
3. Loop Count : 반복 횟수

를 의미한다.

즉, 지금 처럼 1000, 15, 1이라고 설정이 되어 있으면
1000 명의 사용자가 15초 동안 1회 씩 순차적으로 요청을 보낸다고 생각하면 된다.

<br>
<br>

이렇게 시나리오 설정을 끝냈으면 다음은 실제 요청을 모의해야한다.

요청을 보내기 위해서는 샘플러를 사용해야하는데,
스레드 그룹을 우클릭하고 Add > Sampler 로 접근 가능하고
샘플러 중 API에 요청을 보내야하기 때문에 Http Request 를 생성하면 된다.

다 생성했다면 네비게이터는

<br>

![](/assets/img/projects/salog/jmeter_salog_navigator.png)

이와 같은 뎁스를 가질 것이다.

<br>

이제 요청을 모의하기 위해 Http Request의 설정을 살펴보자.

<br>

![](/assets/img/projects/salog/jmeter_http_request.png)

Http Request를 클릭하면 위와 같은 화면이 보이는데, 여기서 각 항목에 맞는 값을 입력하면 된다.

프로토콜, 서버 주소 이름, 포트 번호 를 작성했다면 그 다음 요청 종류를 선택하고 주소를 넣어주면 된다.

UTP-8 로 인코딩하는 것은 그냥 명시적을 작성했다. 없어도 잘 실행된다.

<br>

그리고 Post 요청이기 때문에 바디를 포함 시켰고, 알맞은 정보를 포함 시켜 요청을 전송하면 된다.

<br>

이렇게 하고서 실행하면 요청이 전달은 되는데 응답은 400 에러가 발생한다.

그 이유는 바디의 Content-Type 때문인데, 별도로 설정해주지 않으면 JSON 형태로 데이터를 입력해도 단순히 Text로 전달이 되어 버린다.

이를 해결하기 위해서는 요청 헤더에 명시적으로 Content-Type을 JSON이라고 명시를 해줘야하는데,
해당 설정은 HTTP Request > Config Element 로 접근하여 HTTP Header Manager을 눌러 생성한 다음 진행하면 된다.

해당 HTTP Header Manager 화면은 다음과 같다.

<br>

![](/assets/img/projects/salog/jmeter_http_header_manager.png)

여기서 이제 하단의 Add 를 눌러 name과 value에 위와 같이 각각 Content-Type, application/json 을 입력하며 된다.

<br>

이러한 설정을 거치고 나면 요청이 정상적으로 전달되고 201 Created 가 리턴된다.

<br>
<br>
<br>

### JMeter 사용법 - 데이터

위에서 Http Request 의 바디에

```json
{
  "email": "${username}",
  "password": "${password}",
  "emailAlarm": "false",
  "homeAlarm": "false"
}
```

${username} 과 같이 변수로 입력된 것을 보았을 것이다.

이는 현재 이 회원 가입 요청을 1000명이 보내야 하기 때문에 그 때마다 email이 달라야 해서 그런 것인데,
여기에 들어갈 데이터 들은 CSV 파일로 생성되어 있다.

<br>

적당히 user1 ~ user1000 까지의 데이터를 csv 파일로 만들었고,
이를 사용해 각 값을 대입하여 요청을 진행하는 것이다.

<br>

이런 목 데이터가 담긴 csv 파일을 사용하기 위해서는 
Thread Group에서 Add > CSV Data Set Config 를 생성하여 설정하면 된다.

<br>

![](/assets/img/projects/salog/jmeter_csv_config.png)

해당 설정 파일은 위와 같은 형태로 설정할 수 있는데, 
사용할 csv 파일을 지정하고 각 변수 이름을 쉼표로 구분하여 입력해주면 된다.

그 아래의

Delimiter는 CSV 파일의 구분자,    
Recycle on EOF는 CSV 파일의 끝에 도달 했을 때 다시 처음부터 읽을지 여부를 설정하는 것이고     
Stop thread on EOF는 파일 끝에 도달 했을 때 스레드를 멈출지 여부를 설정하는 것이다.

현재는 딱 1000명 분의 데이터가 입력되어 있으므로, 리사이클 옵션은 False로 Stop 스레드 옵션을 True로 두면 된다.

이 다음 실행 시키면 해당 파일에서 변수를 읽고 요청이 진행된다.

<br>

여기까지 진행했다면 좌측 네비게이터의 뎁스는 아래와 같다.

![](/assets/img/projects/salog/jmeter_depth1.png)

<br>
<br>

#### JSR223 PostProcessor

샐로그 프로젝트에서는 모든 요청 시 헤더로 액세스 토큰을 받아오고, 해당 토큰을 바탕으로 회원을 구별하여 서비스를 제공하는데,
이를 위해서는 1000명 분의 토큰이 필요하다.

그런데 이를 내가 직접 생성하거나 1000번의 로그인 요청 후 리턴되는 액세스 토큰을 1000번 기입해서 CSV 파일을 생성할 순 없다.

애초에 매 로그인 요청마다 같은 회원이라도 액세스 토큰이 달라지기 때문에 현실적으로 불가능하다.

이럴 때 사용할 수 있는 JMeter 의 기능이 JSR223 PostProcessor이다.

이 JSR223 PostProcessor를 사용하여 로그인 요청 시마다 리턴되는 토큰을 CSV 파일을 생성 한 다음 자동으로 담아 낼 수 있다.

기본적으로 테스트 시나리오를 위해 JMeter에서 제공 되는 기능인데, 여러 가지 언어를 지원한다.

<br>

요청 시 진행되는 시나리오기 때문에
HTTP Request > Add > Post Processor > JSR223 PostProcessor 를 순서대로 클릭해 생성할 수 있다.

JSR223 PostProcessor 를 생성하면 아래와 같은 화면이 보인다.

<br>

![](/assets/img/projects/salog/jmeter_JSR223_PostProcessor_default.png)

여기서 Language 옵션으로 사용할 언어를 지정하고 하단의 스크립트에 원하는대로 실행될 스크립트를 작성하면 된다.

```groovy
import java.io.FileWriter
import java.io.BufferedWriter
import java.io.IOException
import groovy.json.JsonSlurper

// 로그인 요청의 응답 데이터 가져오기
def responseBody = prev.getResponseDataAsString()

// 응답 데이터에서 accessToken 추출 (JSON 파싱)
def jsonResponse = new JsonSlurper().parseText(responseBody)
def accessToken = jsonResponse.accessToken

// accessToken을 JMeter 변수에 저장 (선택 사항)
vars.put("accessToken", accessToken)

// CSV 파일에 accessToken 저장
synchronized(this) {
    try {
        BufferedWriter writer = new BufferedWriter(new FileWriter("C:/Users/.../Desktop/샐로그 성능 테스트용 데이터/access_token.csv", true))
        writer.write(accessToken)
        writer.newLine()
        writer.close()
    } catch (IOException e) {
        log.error("Error writing token to file", e)
    }
}
```

GPT의 도움을 받아 Groovy로 작성해보았다.

위와 같이 요청 시 응답 바디에 담긴 accessToken을 추출해 csv 파일에 저장한다.

<br>

여기까지 진행했다면 이제 로그인 요청을 실행 했을 때 1000개 분량의 액세스 토큰이 csv 파일에 순차적으로 저장된다.

이를 바탕으로 다음 테스트를 진행해 볼 수 있게 되었다.

<br>

토큰 CSV 파일에서 각 토큰을 읽고 요청을 보내는 것은 

<br>

![](/assets/img/projects/salog/jmeter_csv_config_2.png)

이와 같이 csv 파일을 읽을 수 있도록 설정 한 다음

<br>

![](/assets/img/projects/salog/jmeter_header_manager_token.png)

이렇게 변수를 활용해 헤더에 포함 될 수 있게 설정하면 된다.

<br>
<br>

#### JSR223 PreProcessor

그런데 요청 URL에 파라미터가 포함되는 테스트 진행 시 이 파라미터를 최대한 변경하면서 진행해야 부하를 더 줄 수 있다고 한다.

부하를 주기 위한 테스트인 만큼 부하가 더 가해질 수 있는 가능성을 포함해야하는데, 이러한 요청 전 시나리오는 어떻게 생성해야할까?

이를 위한 JMeter 의 기능이 JSR223 PreProcessor이다.

<br>

이 JSR223 PreProcessor는 말 그대로 사전 시나리오를 생성할 수 있도록 하는 기능인데, 요청 시 파라미터를 임의로 변경할 수 있게 만들 수 있다.

<br>

![](/assets/img/projects/salog/jmeter_pre_processor.png)

이렇게 PreProcessor를 PostProcessor 와 같이 생성한 다음 스크립트를 작성하면 된다.

<br>

여기서 현재는 page 만 랜덤으로 들어가게 작성했는데,
이는 테스트 요구사항 때문에 그렇다.

page, size, date 가 필요한 조회 시의 시나리오이고 실제로 size는 10으로 고정되어서 요청이 들어오기 때문이다.

또한, date의 경우 목 데이터 때문인데, 현재 테스트 시 6월에 한정하여 데이터를 포함시켰기 때문에 월은 6월이 고정이다.

마찬가지로 일자도 동일한 방식이기 때문에 아직까지는 일자도 고정이다.

<br>

추후 더 많은 케이스를 생성하여 월과 일자에 대한 데이터가 분산되면 해당 파라미터도 랜덤으로 입력되게 할 수 있다.

<br>
<br>
<br>

### JMeter 사용법 - 리포팅

JMeter 에는 기본적으로 요청과 결과에 대한 로그를 남길 수 있는 기능이 있다.

플러그인을 사용해 좀 더 복잡하게 설정하여 모니터링할 수 있는 도구도 만들 수는 있지만, JMeter는 대부분 부하를 주는 용도로 사용한다고 한다.

제대로된 모니터링은 프로메테우스를 사용할 예정이지만 어떤 기능이 있는지 살펴보자.

<br>

우선은 각 스레드 그룹마다 리포팅 해줄 리스너를 생성해야한다.

Thread group > Add > Listener 까지 우클릭으로 살펴보면 여러 리스너가 있는데 아래와 같이 5가지를 생성했다.

<br>

![](/assets/img/projects/salog/jmeter_Listeners.png)

<br>
<br>

#### View Results Tree

View Results Tree는 각 샘플 요청에 대한 상세한 결과를 트리 구조로 보여준다.

<br>

![](/assets/img/projects/salog/jmeter_vuew_results_tree.png)

이와 같이 요청과 결과를 상세히 보여주는데, 만약 테스트가 실패한다면 여기서 어떤 것이 문제인지 체크할 수 있다.

또, 결과를 csv 파일로 생성할 수도 있다.

<br>
<br>

#### Aggregate Report

Aggregate Report는 테스트 결과의 요약 통계를 표로 보여준다.

<br>

![](/assets/img/projects/salog/jmeter_aggregate_report.png)

위와 같이 보여지며, 각 샘플러에 대한 평균, 최댓값, 최솟값, 표준 편차, 에러율 등의 통계를 제공한다.

<br>
<br>

#### Summary Report

Summary Report는 테스트 결과의 요약 통계를 콘솔 형식으로 보여준다.

Aggregate Report와 비슷해보이는데, 항목이 좀 더 적으며, 기본적인 통계를 제공한다.

<br>

![](/assets/img/projects/salog/jmeter_summary_report.png)

<br>
<br>

#### Graph Results

테스트 결과를 그래프로 시각화하여 보여준다.

<br>

![](/assets/img/projects/salog/jmeter_graph_results.png)

<br>
<br>

#### Response Time Graph

각 샘플 요청의 응답 시간을 그래프로 시각화하여 보여준다.

간격은 ms를 기준으로 조정할 수 있다.

<br>

![](/assets/img/projects/salog/jmeter_response_time_graph.png)

위와 같이 설정이 가능하다.

이후 Display Graph를 실행하면

<br>

![](/assets/img/projects/salog/jmeter_response_time_graph2.png)

이렇게 표시된다.

<br>
<br>

이 테스트 결과들은 로컬에서 샐로그를 실행해서 진행했다.

<br>
<br>
<br>

### 목표

CRUD 중 주로 문제가 되고 개선할 여지가 있는 기능은 "조회"라고 생각한다.

대부분의 애플리케이션에서 사용자는 데이터를 생성하거나 업데이트하기 보다는 조회하는 경우가 일반적으로 많다.

예를 들어, 쇼핑몰 애플리케이션의 경우 상품을 검색하고 조회하는 것이 구매보다 훨씬 더 빈번하게 일어난다.

마찬가지로 샐로그 프로젝트에서도 회원가입 보다는 자신의 수입이나 지출을 검색하고 확인하기 위해 조회하는 경우가 훨씬 많은 것이다.

그렇기 때문에 나도 부하 테스트를 조회 위주로 생각하려 한다.

우선은 1000 명의 회원이 10초 이내에 각각 100번의 조회를 한다는 시나리오를 적용해 총 10만 건의 조회 요청을 진행해 보았다.

그 결과가 아래이다.

![](/assets/img/projects/salog/jmeter_test_Result.png)

앞서 말했듯이 page 를 랜덤으로 주고 테스트를 진행해 보았고, 실행 시마다 결과가 달라 오차가 있는데

평균 응답 시간이 1.7초 ~ 2.3초 정도가 나온다.

<br>

추후 H2 뿐만 아니라 실제 배포 환경에서 사용되는 MySQL 데이터 베이스를 적용하고서 진행해볼 생각이고

모니터링 도구를 적용하여 CPU 사용량, 메모리, 디스크, 네트워크 사용량 등을 측정해서 개선 목표를 정하고 개선해볼 생각이다.

<br>
<br>
<br>

***

### 후기

다음으로는 프로메테우스를 적용하고 그라파나를 활용해 시각화 해볼 생각이다.

결과적으로는 해당 테스트 내용을 바탕으로 개선 목표를 정하고 개선을 해야하기 때문에 하직 할 것이 많다.

<br>

근데 짧은 시간에 부하를 확 줘서 그런지 인텔리제이가 자꾸 멈춘다...

그나마 다행인 것은 JMeter가 사용하기 쉽다는 것인데, 프로메테우스와 그라파나가 좀 걱정된다.
