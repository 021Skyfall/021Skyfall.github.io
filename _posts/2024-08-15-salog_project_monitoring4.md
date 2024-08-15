---
title: "프로젝트:샐로그 / 모니터링 - 커스텀 매트릭 추가"
author: Jeremiah Lee
date: 2024-08-15
categories: [ 프로젝트, 샐로그, 모니터링, mysql ]
tags: [프로젝트, 샐로그, 모니터링, 분석, mysql]
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

개선 작업에 대한 포스트를 작성하기 전에, 이 개선 작업을 진행하면서 발생한 이슈와 이 이슈를 핸들링하기 위해 추가한 커스텀 매트릭을 기록하기 위해 이번 포스트를 작성한다.

<br>

지금까지 좀 착각하던 것이 있었다.

2.4만번의 요청을 대량으로 발생시키면서 인텔리제이가 화면에서 모든 로그를 출력한 후 매트릭 수집 요청 로그를 보여줄 때 부터가 모든 요청에 대한 처리가 완료된 시간 인줄 알았는데,
그게 아니었다.

그러니까, 실행되고 있는 인텔리제이의 RUN 탭에서 나오는 로그가 전부 다 출력되고 나서야 요청 처리 완료. 라고 생각했는데 아니었던 것이다.

이는 단순히 요청이 늘어나면서 렉 걸렸던 것이고 실제로는 요청 시 바로 응답이 돌아갔던 것이다.

<br>

이런 생각이 들기 시작한 것은 JMeter 로그를 보고서 부터다.

JMeter 상에서는 요청 즉시 응답 로그가 출력되고 있었는데 이를 처음부터 간과했다.

병목현상이 발생함으로써 응답이 느려지는 줄 알았는데, 그래봐야 0.03초 단위고 육안으로 식별하기 어려운 것이었다.

이를 모니터링 도구를 보고 있음에도 이렇게 생각한 것은 내 큰 실수다.

당장 시각화한 패널에서도 0.03초 단위로 요청에 대해 200 응답이 돌아오고 있는데도 이렇게 생각해버렸다.

<br>

그래서 좀 더 객관화하기 위해 조회 시 200 응답을 받는 것을 확인하기 위해 응답을 카운트하는 매트릭을 추가하였다.

이 포스트는 해당 작업을 어떻게 진행했는지 작성할 것이다.

<br>
<br>
<br>

### 커스텀 매트릭 모니터링

내용에 들어가기 앞서

[Prometheus와 Grafana 기반으로 커스텀 메트릭 모니터링 고도화하기](https://www.catsriding.com/posts/custom-metrics-monitoring-in-spring-using-prometheus-and-grafana)

catsriding.com의 위 게시글을 참고했다.


<br>

이 커스텀 매트릭을 생성하기 위해서는 기본적인 actuator 의존성이 필요한데,

```
implementation 'org.springframework.boot:spring-boot-starter-actuator'
```

이 의존성은 지난 모니터링 구축 시 적용했었다.

<br>

간단하게 사용법을 먼저 살펴보면

특정 메서드의 호출 시 마다 카운트 되는 매트릭을 추가하기 위해 타겟 메서드에 @Counted 어노테이션을 추가하고
옵션으로 이름을 이.름의 형식으로 작성하면 된다.

```java
@Counted("metric.order")
```

이런 식이다.

이후 프로메테우스로 수집한 해당 매트릭을 보고 싶다면 Prom쿼리를 

```
metric_order_total
```

이런 식으로 활용할 수 있다.

<br>
<br>
<br>

### 커스텀 매트릭 고도화

위 내용을 참조하여 특정 매트릭의 모든 응답 코드에 대한 응답 총 갯수를 리턴할 수 있지만,
나는 200 응답에 대한 매트릭만 수집하기 위해서 조금 더 설정을 가미했다.

요청에 대해 200 응답이 동일한 횟수로 돌아오는지만을 확인하기 위해 아래와 같이 HttpResponseCounter 클래스를 구현했다.

```java
import io.micrometer.core.instrument.Counter;
import io.micrometer.core.instrument.MeterRegistry;
import org.springframework.stereotype.Component;

import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;

@Component
public class HttpResponseCounter {

    private final MeterRegistry meterRegistry;
    private final Map<String, Counter> counters = new ConcurrentHashMap<>();

    public HttpResponseCounter(MeterRegistry meterRegistry) {
        this.meterRegistry = meterRegistry;
    }

    public void recordResponse(String status) {
        // 상태 코드에 따라 카운터를 생성하고 증가시킴
        counters.computeIfAbsent(status, key -> Counter.builder("http_response_count")
                        .description("Count of HTTP responses by status code")
                        .tag("status", key)
                        .register(meterRegistry))
                .increment();
    }
}
```

@Component 로 빈에 등록해주고, MeterRegistry 클래스를 주입해 수집 도구를 활성화 한다.

그리고 형식에 맞게 결과 값을 counters에 담아 리턴하도록 설정했다.

처음에는 응답 코드를 200으로 지정해서 해당 응답 코드만 수집하도록 했었는데, 조금 더 유연하게 하기 위해 각 상태 코드를 키로 활용해
모든 상태 코드를 개별적으로 볼 수 있도록 구현했다.

이를 통해 http_response_count 라는 이름의 매트릭을 수집할 수 있게 되었다.

<br>

이렇게 구현된 매트릭 수집 클래스를 활용하기 위해 수집하고자 하는 타겟을 지정해야하는데,

```java
    @GetMapping
    public ResponseEntity<?> getAllIncomes (@RequestHeader(name = "Authorization") String token,
                                            @Positive @RequestParam int page,
                                            @Positive @RequestParam int size,
                                            @Valid @RequestParam(required = false) String incomeTag,
                                            @RequestParam String date) { // 날짜는 스트링으로 입력받고, 서비스에서 핸들링 (월별 조회 00 처리 시)

        tokenBlackListService.isBlackListed(token);

        MultiResponseDto<IncomeDto.Response> pages =
                incomeService.getIncomes(token, page, size, incomeTag, date);

        httpResponseCounter.recordResponse("200"); // 프로메테우스 응답 상태 코드 매트릭 수집

        return new ResponseEntity<>(pages, HttpStatus.OK);
    }
```

요청과 응답은 1차적으로 컨트롤러 레이어에서 통신을 주고 받기 때문에 위와 같이 컨트롤러 클래스에 매트릭 수집 클래스 의존성을 주입하고
타겟 메서드에서 활용하면 된다.

특정 상태 코드에 대한 매트릭을 수집하기 위해 전달인자로 200을 입력했다.

<br>

이렇게 조회 요청에 대한 응답의 총 개수를 매트릭으로 수집하고 그라파나로 시각화할 수 있도록 구성했다.

한 가지 주의할 점은 최초로 매트릭을 수집해야 그 때부터 prom쿼리로 출력이 가능하다는 것이다.

<br>

이렇게 구성하면

![](/assets/img/projects/salog/monitoring_mysql/Default_non_async.png)

이 모니터링 결과에서 볼 수 있듯이 "요청에 대한 http 응답 수"를 확인할 수 있다.

<br>
<br>
<br>

### 후기

이제 이를 바탕으로 몇 번의 테스트를 거친 결과, 개요에서도 언급했지만 기존에 생각하던 것과 달리 응답과 요청 사이에는 0.03초라는 아주 짧은 시간만이 걸리기 때문에
모든 요청에 대한 응답은 그때그때 돌아간다고 생각해야한다.

내가 왜 병목현상으로 16분이나 걸린다.고 생각했는지 지금와서 생각해보면 모르겠다...

결국 모든 지표와 로그가 응답은 바로 돌아가는 것으로 나타나는데 왜 단편적으로 인텔리제이만 보고 그런 생각을 했는지 좀 부끄럽기도 하다.

사실 이 커스텀 매트릭 수집기가 없어도 기존 오류를 바로 잡을 수 있지만 그래도 확실히 할겸 한번 시도 해보았다.

<br>

아 그럼에도 결국 http 요청 지연 시간이 최대 0.5초로 늘어난 다는 것 자체는 병목현상이 발생했기 때문이라고 생각한다.

단순히 0.5초기 때문에 현재 모니터링 결과로는 바로바로 응답이 돌아오는 것 처럼 보이지만 최대 요청 지연 시간이 늘어나는 것 자체가
요청이 쏠려 병목현상이 일어나기 때문이라고 보기 때문이다.

<br>

어쨌든 이전 오류들은 그대로 냅두고자한다. 

어떤 시행착오를 거쳐 여기까지 왔는지 확인할 수 있도록.
