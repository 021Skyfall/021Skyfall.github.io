---
title: "프로젝트:샐로그 / 테스트 - 수입 1 : 컨트롤러 슬라이스 테스트 (Gson 커스텀)"
author: Jeremiah Lee
date: 2024-05-21
categories: [ 프로젝트, 샐로그, 테스트 ]
tags: [프로젝트, 샐로그, 테스트]
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

이번에는 수입 파트다.

회원 쪽에서 많은 내용을 작성했고, 해당 포스트만 봐도 테스트 케이스를 어떻게 작성해야할지 알 수는 있기 때문에
수입 부분에서는 특별한 내용이 있지 않은 한 작성하지 않을 생각이다.

<br>

그렇기 때문에 굉장히 짧거나 작성하지 않을 수 있다.

<br>

그런데 수입 쪽에서 컨트롤러 슬라이스 테스트를 진행하던 중 특이사항이 있기 때문에 이번 포스트를 작성한다.

<br>
<br>
<br>

### 수입 컨트롤러 슬라이스 테스트

앞선 회원 컨트롤러 슬라이스 테스트와 마찬가지로 "통신"이 잘 되는 지에 중점을 두고 테스트 케이스를 작성했다.

이를 위해 원하는 값으로 요청을 보내고서 예상된 응답 코드가 돌아오는 지 확인하면 된다.

전체적인 흐름을 보아야하는 것 아닌가라고 생각할 수 있지만 애플리케이션 통합 테스트를 별도로 진행할 예정이고, 슬라이스 테스트는 빠르고 간결하게 동작하는 지
파악하는 테스트이기 때문에 간단하게 진행했다.

<br>

먼저 말하자면, 수입 /post 엔드포인트로의 요청을 제외하면 나머지는 회원 쪽과 거의 동일하다.

수입 post도 대부분 비슷하지만 한 가지 유의해야할 것이 있기 때문에 작성한다.

```java
    @Test
    @DisplayName("/post")
    @Order(1)
    void postIncomeTest() throws Exception {
        // Given
        LocalDate date = LocalDate.of(2024, 1, 1);
        IncomeDto.Post post = new IncomeDto.Post(100, "testName", "testMemo", date, "testTag");

        // LocalDate 커스텀 직렬화
        Gson gson = new GsonBuilder()
                .registerTypeAdapter(LocalDate.class, new LocalDateSerializer())
                .create();

        String content = gson.toJson(post);

        // When
        mockMvc.perform(
                post("/income/post")
                        .header("Authorization", "Bearer fakeToken")
                        .accept(MediaType.APPLICATION_JSON)
                        .contentType(MediaType.APPLICATION_JSON)
                        .characterEncoding("UTF-8")
                        .content(content)
                )

                // Then
                .andExpect(status().isCreated())
                .andDo(print());
    }
```

이게 수입 등록에 대한 테스트 케이스다.

중간을 보면 gson을 커스텀 직렬화 했다고 되어 있는데, 회원 쪽과 동일하게 진행하면서 문제가 발생해 정의된 내용이다.

<br>

커스텀 직렬화 부분을 제외하면 post DTO에 요청 바디를 넣고 /income/post 엔드포인트로 요청을 해보는 방식이다.

여기서 문제는 date다.

date는 타입 자체가 LocalDate 타입이기 때문에 실제로 요청 시 json 형태로 "2024-01-01"로 요청을 보내게 된다.

그러나 현재 테스트 케이스는 java로 작성이 되기 때문에 문법에 맞게 초기화 후 인스턴스화 해야 했다.

여기까지 진행한 후 테스트를 실행해 보니 응답 코드가 400이었고, 어드바이스로 정의해놓은 에러코드를 뱉어냈다.

<br>

이 이유를 찾기 위해 andDo(print()) 로 출력된 결과를 살펴봤지만, 바디가 인코딩 되지 않았다는 정보와 함께 작성한 바디가 아예 보이지 않았다.

그래서 요청 시 UTF-8 인코딩을 적용하게 되었다.

<br>

이후 다시 실행시켜보니 바디에 담긴 date가 

"date" : "2024-01-01" 형태가 아닌 "date" : {"year" : 2024, "month" : 1, "day" : 1} 이런 형태로 출력되었다.

이를 바탕으로 postDto를 json으로 직렬화 해주는 gson을 활용하여 해결할 수 있는 방법은 없는지 구글링 해보았고,    
[쏘니의 개발블로그](https://juntcom.tistory.com/97) 여기서 해결법을 찾을 수 있었다.

그렇게 커스텀 생성을 위해 정의한 코드가 위 내용이며,

```java
    private static class LocalDateSerializer implements JsonSerializer<LocalDate> {
        @Override
        public JsonElement serialize(LocalDate src, Type typeOfSrc, JsonSerializationContext context) {
            return new JsonPrimitive(src.format(DateTimeFormatter.ISO_LOCAL_DATE));
        }
    }
```

LocalDateSerializer 클래스를 활용해 날짜에 대한 포맷을 커스텀 했다.

<br>

이를 통해 문제를 해결할 수 있었고, 테스트는 정상적으로 통과했다.

<br>
<br>
<br>

***

### 후기

사실 처음에는 바디 인코딩을 어떤 식으로 해야할지에 대해 찾느라 헤맸고, gson을 어떻게 수정할 수 있을 지에 대해서도 많이 헤맸다.

이외에도 요청 uri를 잘못 입력해서 거의 1시간 가량을 헤맸다.

<br>

그래도 이런 시행착오를 통해 gson을 커스텀해 원하는 형태로 직렬화 할 수 있다는 것을 알게 되었고, 테스트 시 한결 편하게 수행 가능할 것 같다.
