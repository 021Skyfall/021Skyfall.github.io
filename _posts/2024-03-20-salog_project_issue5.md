---
title: "프로젝트:샐로그 / 이슈 5 - 수입 조회 월별/일별 핸들링 (+날짜 타입)"
author: Jeremiah Lee
date: 2024-03-20
categories: [ 프로젝트, 샐로그, 문제해결 ]
tags: [프로젝트, 샐로그, 문제해결, 서비스 ]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Project_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

### ***1. 개요***

기본적으로 어떤 서비스 로직이든 CRUD 중 CUD에 대한 구현은 크게 어렵지 않았다.

R에 관련해서는 특히 쿼리 메서드를 사용해야한다거나 혹은 조회에 어떠한 조건이 들어가게 되면 조금 까다로워진다.

이전에 페이징 처리에 대한 포스트도 했었는데, 그런 식으로 무언가를 추가하려하면 복잡해지는 경향이 있다.

<br>

이번에는 수입 관련 조회 시에 어떻게 핸들링 했는가에 대한 내용이다.

아주 어려운 문제는 없지만 이 조건에 따른 비지니스 로직을 어떻게 구현했는가가 주요 포인트이다.

<br>

수입, 지출 등 가계부에 대한 내용을 조회할 시 특히 수입, 지출에서는 월별과 일별로 조회 기준이 나뉜다.

처음에는 일별 조회만 있었는데, 대쉬보드 페이지가 아닌 가계부 페이지에서는 월별로 조회해야하기 때문에 두 가지 조회 기능이 필요한 것이다.

그러나 여기서 각각의 API로 분리해서 처리하게 되면 책임이 겹치는 듯하고, 크게 복잡하지 않은 로직을 불필요하게 분리하는 것 같아 하나로 묶어서 처리하도록 했다.

만약 여기서 추가되는 조건이 더 붙는다면 아마 나눴을 것이다.

<br>
<br>
<br>

### ***날짜에 대해 어떻게 처리해야할까***

이 날짜에 대한 처리를 어떻게 해야할까?

아주 단순하게는 DTO를 활용해 요청 시 바디로 날짜를 받아오고, 이 날짜를 다시 자바 객체로 매핑하여 처리하면 된다.

이 경우, YYYY-MM-DD의 형식으로 날짜 데이터와 호환되는 LocalDate라는 타입을 사용하면 핸들링하기 용이하다.

<br>

또한, "월별" 조회의 경우는 "일자"가 불필요하니 YearMonth 타입을 사용하면 자바의 표준 라이브러리를 사용하여 핸들링이 가능하다.

두 타입의 차이점은 
1. LocalDate = YYYY-MM-DD 형식
2. YearMonth = YYYY-MM 형식  

이다.

각 상황에 맞게 일자까지 필요한 경우 LocalDate, 월까지만 필요한 경우 YearMonth 타입을 활용하여 데이터를 핸들링하면 된다.

추가적으로 LocalDateTime 이라는 타입은 YYYY-MM-DD-HH-MM-SS-NS 으로 시간까지 처리가 가능하다.

또한 이 타입을에 대해 now() 함수를 사용하면 시스템의 현재 시각을 리턴한다.

<br>
<br>
<br>

### ***수입 조회에서의 월별/일별 분기***

위와 같은 타입을 활용하면 보다 편리하게 "시간"에 대해 핸들링할 수 있지만, 내가 구현한 API의 경우에는 약간 내용이 달랐다.

처음에는 LocalDate 타입을 활용하여 월별 조회 혹은 일별 조회를 분기하려고 했는데, 이를 위한 스위치가 필요했다.

이 스위치라는 건 수입 조회 시에 YYYY-MM-DD 형식으로 데이터를 받아올텐데, 이 정보만을 가지고 월별이냐 혹은 일별이냐를 구분해야한다는 의미이다.

여기서 한 가지 생각한게, DD가 00 일때는 월별 조회, DD가 00이 아닐 때는 일별 조회로 분기하여 핸들링하려고 했다.

그런데 여기서 문제는, LocalDate 타입에서 DD가 00인 경우 날짜 포맷 불일치 문제가 발생하는 것이었다.

즉, LocalDate 입장에서는 DD가 00인 것을 받아들이지 못하는 것이다.

그렇기 때문에 처음 바디로 받아올 때부터 DTO의 date 변수에 대한 타입이 LocalDate이기 때문에 입력조차 되지 않았다.

<br>

그래서 이를 가능하게 꾸미기 보다는 초점을 내가 정한 기준으로 두었다.

즉, YYYY-MM-00 인 경우 월별 조회 / YYYY-MM-DD 인 경우 일별 조회라는 명확한 기준을 두고서 이에 맞춰 구조를 변경했다.

<br>

앞서 개요에서 말했듯이 이를 LocalDate와 YearMonth를 받아들이고, 리턴하는 API를 분리하여 작성한다면 기능이 중복된 API가 필요하기 때문에
이런 방식을 활용한 것이다.

<br>
<br>
<br>

### ***월별/일별 조회 구현***

그러면 위 조건으로 어떻게 처리를 했는지 코드와 함께 설명하고자 한다.

<br>

기존에는 조회 DTO를 구현하여 조회 요청 시 바디에 Date를 포함하여 요청을 하도록 처리했었는데, 이에 대응하는 Dto의 변수 타입이
LocalDate였다.

이 LocalDate를 String으로 변경하고, Validation으로 YYYY-MM-DD 형식으로만 데이터를 받을 수 있게 구성했다.

이후 이 문자열 타입의 date를 서비스 객체에서 배열을 활용해 Year, Month, Day로 분리했다.

```java
int[] dates = Arrays.stream(date.split("-")).mapToInt(Integer::parseInt).toArray();
int year = dates[0];
int month = dates[1];
int day = dates[2];
```

각각 변수명과 맞춰 내용을 담아내고, 이후 월별이냐 일별이냐를 분기했다.

앞서 말한대로 월별인 경우 day = 0 이어야하고, 이외의 경우는 일별 조회로 넘어가게끔 조건을 설정했다.

```java
if (day == 0) {
    fixedIncomes = fixedIncomeRepository.findByMonth(memberId, year, month,
                    PageRequest.of(page - 1, size, Sort.by("date").descending()));
} else {
    fixedIncomes = fixedIncomeRepository.findByDate(memberId, year, month, day,
                    PageRequest.of(page - 1, size, Sort.by("date").descending()));
}
```

그 다음으로는 쿼리 메서드를 활용하여 Year, Month, Day를 기준으로 조회하게끔 구성했다.

물론 PageRequest 객체를 생성하여 페이징 처리를 포함했다.

<br>

이렇게 처리한 다음 리턴 값을 아래와 같이 응답 DTO에 매핑했다.

```java
Page<FixedIncome> fixedIncomes;

... (조회 로직)

List<FixedIncomeDto.Response> fixedIncomeList = fixedIncomes.getContent().stream()
  .map(fixedIncomeMapper::fixedIncomeToFixedIncomeResponseDto)
  .collect(Collectors.toList());

return new MultiResponseDto<>(fixedIncomeList, fixedIncomes);
```

페이징을 위한 타입을 참조하는 fixedIncomes 변수를 미리 선언하고, 조건을 분기한 다음 리턴되는 응답들을 이 변수에 담아낸다.

이후 매퍼를 사용해 List에 내용을 담고서 리턴했다.

<br>

이렇게 처리하여 월별/일별 조회를 하나의 함수에서 핸들링할 수 있게 되었다.

<br>
<br>
<br>

### ***수입 조회 시에 DTO가 필요할까?***

위와 같은 방식으로 내가 생각해낸 조건에 맞게 데이터를 가공하여 요청에 대한 응답을 리턴하고는 있지만,
이 방식이라면 굳이 DTO를 활용해 요청 시 바디에 date를 포함할 필요는 없다고 생각했다.

물론 조회 시에 복잡한 데이터가 포함되어야 한다면 DTO를 활용하는 것이 맞겠지만, 기본적으로 나는 조회 시 필요한 데이터의 경우는 URL의 파람으로 입력받아
처리해야한다고 생각한다.

"조회"라는 것이 말 그대로 데이터를 "입력"한다기 보다는 "확인"하는 것이기 때문이다.

그래서 보안이 관련되어 있거나, 길이가 아주 긴 내용이 아니라면 조회 시에는 파람으로 조건을 입력하는 게 맞다고 생각한다.

그래서 수입 조회도 구조를 변경했다.

<br>

기존에 존재하던 조회 DTO를 삭제하고, Param으로 받아오게끔 만들었다.

```java
@GetMapping("/get")
public ResponseEntity<?> getAllFixedIncomes (@RequestHeader(name = "Authorization") String token,
                                             @Positive @RequestParam int page,
                                             @Positive @RequestParam int size,
                                             @RequestParam String date) {
```

또한 LocalDate나 YearMonth와 같은 자바 타입이 아니라 단순히 문자열이기 때문에 이 방법이 간단하고 적합하다고 생각했다.

단순히 요청 시 데이터를 어떤 방식으로 받아오냐에 대한 차이기 때문에, 구현 내용에 변동사항은 없다. 

<br>
<br>
<br>

***

오랜만에 이슈에 대한 내용이다.

원래는 이런 내용을 작성하지 않으려 했는데, 크고 복잡하고 대단한 내용이 아니더라도 내가 어떤 것을 구현하면서, 구성하면서 어떻게 생각했는지가 중요한
것 같아서 이런 쉽고 간단한 내용도 앞으로는 전부 블로깅할 예정이다.

간단한 것이라도 쌓이면 스킬이 된다고 생각한다.
