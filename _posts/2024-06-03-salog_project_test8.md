---
title: "프로젝트:샐로그 / 테스트 - API 문서화 feat.수입 통합 테스트"
author: Jeremiah Lee
date: 2024-06-03
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

이번 포스트는 API 문서화에 관해서이다.

이전 포스트에서 회원 통합 테스트를 살펴보았는데, 테스트 케이스 로직 자체는 수입이나 회원이나 크게 다를게 없다.

그래서 직접 깃헙에 가서 코드를 보고 파악하는 편이 좋다고 생각한다.

<br>

API 문서화는 이전에 한번 학습한 적이 있었지만 이번 처럼 프로젝트 진행 중에 출력해본 것은 처음이다.

이 문서는 무조건 테스트를 거쳐야만 출력할 수 있는데, 문서를 만들기 위한 스니펫이 테스트를 통해 만들어지기 때문이다.

<br>
<br>
<br>

### API 문서화 시점

통합 테스트는 전체 시스템이나 애플리케이션의 여러 컴포넌트가 함께 잘 작동하는 지 확인하는 과정이다.

이 시점에서는 대부분의 API 기능이 완성되어 있기 때문에, 문서화를 통해 API의 사용 방법과 응답 등을 명확하게 기술할 수 있다.

또한, 통합 테스트를 통해 API의 동작을 검증함으로써, 문서화된 내용이 실제 API의 동작과 일치하는지 확인할 수 있다.

이를 통해 개발 단계에서 놓칠 수 있는 세부 사항을 보완하고, 빠른 피드백을 얻어 유지보수를 용이하게 한다.

<br>

결과적으로 API 문서화란 개발 과정에서 여러 시점에 걸쳐 점진적으로 진행할 수 있다.

초기에는 API의 설계와 인터페이스에 초점을 맞춘 문서화를 시작할 수도 있고,
후반에 들어서는 설명서처럼 사용법과 정확성을 파악할 수 있다.

정리하자면, 통합 테스트 시점에는 API 기능이 완성된 상태이기 때문에 문서를 완성할 수 있으므로 이 시점에 문서화를 진행하여 유지보수 작업, 혹은 디버깅 작업을 실시하는 것이 좋다.

<br>
<br>
<br>

### API 문서화 과정

문서를 출력하기 위해서는 앞서 말했 듯 문서의 조각들인 스니펫이 필요하다.

이 스니펫은 테스트 결과를 바탕으로 생성되는데, 정확한 뜻은 재사용 가능한 소스 코드, 기계어, 텍스트의 조각이다.

<br>

API에 요청과 응답을 거치는 테스트를 작성하고 해당 테스트가 실행되면 결과를 텍스트 조각으로 생성,
이후 해당 내용을 바탕으로 문서를 작성하게 된다.

보통 이 과정은 문서의 바탕이 되는 서식만 설정해 놓으면 ascii doctor 가 자동으로 구성해준다.

<br>

#### build.gradle

우선 문서화를 진행하기 위해, 필요한 의존 라이브러리를 설정해야 한다.

```java
plugins {
	id 'java'
	id 'org.springframework.boot' version '2.7.17'
	id 'io.spring.dependency-management' version '1.0.15.RELEASE'
	id 'org.asciidoctor.jvm.convert' version '3.3.2'
}
```

기존 플러그인에 추가로 asciidoctor 를 jvm에서 사용할 수 있도록 해주고,

```java
dependencies {
    ...
	// api 문서화
	testImplementation 'org.springframework.restdocs:spring-restdocs-mockmvc'
    ...
}
```

문서화를 위한 테스트 의존 라이브러리를 추가한다.

<br>

또한 테스트 결과로 생성된 스니펫의 디렉토리를 설정해주고, 매 빌드 시마다 깔끔한 생성을 위해 스니펫을 초기화 해주는 설정을 진행했다.

```java
tasks.named('test') {
	useJUnitPlatform()
	outputs.dir(snippetsDir)  // 테스트 출력 디렉토리를 설정
}

clean { // 빌드 시 스니펫 초기화
	delete 'build/generated-snippets'
}
```

<br>
<br>

#### 각 도메인 별 문서 분리

다음으로 테스트 클래스를 살펴보기 전에 한 가지 더 알아볼 것이 있는데,
내가 맡은 파트가 총 4 가지의 도메인이기 때문에 각 부분을 헷갈리지 않게 분리해 문서화를 진행했다.

```java
// 도메인 별 문서 분리

task memberTest(type: Test) {
	include '**/MemberIntegrationTest.class'
	outputs.dir file("$buildDir/generated-snippets/MemberIntegrationTest")
	useJUnitPlatform()
}

task incomeTest(type: Test) {
	include '**/IncomeIntegrationTest.class'
	outputs.dir file("$buildDir/generated-snippets/IncomeIntegrationTest")
	useJUnitPlatform()
}

task fixedIncomeTest(type: Test) {
	include '**/FixedIncomeIntegrationTest.class'
	outputs.dir file("$buildDir/generated-snippets/FixedIncomeIntegrationTest")
	useJUnitPlatform()
}

task budgetTest(type: Test) {
	include '**/BudgetIntegrationTest.class'
	outputs.dir file("$buildDir/generated-snippets/BudgetIntegrationTest")
	useJUnitPlatform()
}
```

각 테스트의 타입을 분리하고, JUnit5 로 테스트가 실행될 수 있게 작성했다.

또한, 각 스니펫을 서로 다른 디렉토리에 생성되게 작성했다.

<br>

```java
task asciidoctorMember(type: org.asciidoctor.gradle.jvm.AsciidoctorTask) {
	sourceDir = file('src/docs/asciidoc/MemberIntegrationTest')
	outputDir = file("$buildDir/docs/asciidoc/MemberIntegrationTest")
	inputs.dir file("$buildDir/generated-snippets/MemberIntegrationTest")
	attributes 'snippets': file("$buildDir/generated-snippets/MemberIntegrationTest")
	dependsOn memberTest
	doLast {
		copy {
			from "${outputDir}/html5"
			into "src/main/resources/static/docs/MemberIntegrationTest"
		}
	}
}

task asciidoctorIncome(type: org.asciidoctor.gradle.jvm.AsciidoctorTask) {
	sourceDir = file('src/docs/asciidoc/IncomeIntegrationTest')
	outputDir = file("$buildDir/docs/asciidoc/IncomeIntegrationTest")
	inputs.dir file("$buildDir/generated-snippets/IncomeIntegrationTest")
	attributes 'snippets': file("$buildDir/generated-snippets/IncomeIntegrationTest")
	dependsOn incomeTest
	doLast {
		copy {
			from "${outputDir}/html5"
			into "src/main/resources/static/docs/IncomeIntegrationTest"
		}
	}
}

task asciidoctorFixedIncome(type: org.asciidoctor.gradle.jvm.AsciidoctorTask) {
	sourceDir = file('src/docs/asciidoc/FixedIncomeIntegrationTest')
	outputDir = file("$buildDir/docs/asciidoc/FixedIncomeIntegrationTest")
	inputs.dir file("$buildDir/generated-snippets/FixedIncomeIntegrationTest")
	attributes 'snippets': file("$buildDir/generated-snippets/FixedIncomeIntegrationTest")
	dependsOn fixedIncomeTest
	doLast {
		copy {
			from "${outputDir}/html5"
			into "src/main/resources/static/docs/FixedIncomeIntegrationTest"
		}
	}
}

task asciidoctorBudget(type: org.asciidoctor.gradle.jvm.AsciidoctorTask) {
	sourceDir = file('src/docs/asciidoc/BudgetIntegrationTest')
	outputDir = file("$buildDir/docs/asciidoc/BudgetIntegrationTest")
	inputs.dir file("$buildDir/generated-snippets/BudgetIntegrationTest")
	attributes 'snippets': file("$buildDir/generated-snippets/BudgetIntegrationTest")
	dependsOn budgetTest
	doLast {
		copy {
			from "${outputDir}/html5"
			into "src/main/resources/static/docs/BudgetIntegrationTest"
		}
	}
}
```

이후, 스니펫을 바탕으로 생성되는 문서의 빌드 디렉토리를 지정하고 각각 다른 문서로 작성될 수 있게 설정했다.

<br>

이 과정을 통해 각 도메인 별로 문서를 분리할 수 있게 되었고, 보다 가독성 좋게 구성했다.

<br>
<br>

#### 테스트 클래스 문서화 설정

위와 같은 설정을 거쳐 문서화를 진행할 준비는 다 끝났다.

이제 기존에 작성된 테스트 클래스에서 실행 시 결과 조각 모음인 스니펫을 생성해야 한다.

```java
@SpringBootTest // 테스트 환경 애플리케이션 컨텍스트 로드
@AutoConfigureMockMvc // MockMvc 자동 구성, 웹 계층 테스트
@AutoConfigureRestDocs(outputDir = "build/generated-snippets") // Rest Docs 자동 구성, 문서화
@TestMethodOrder(MethodOrderer.OrderAnnotation.class) // 테스트 케이스 순서 보장
@Transactional
public class IncomeIntegrationTest {
  ...
}
```

이전 포스트에서 살펴봤던 애너테이션들인데, 여기서 나머지를 제외하고 @AutoConfigureRestDocs가 바로 API 문서화를 위한 Rest Docs 자동 구성해주는 애너테이션이다.

또한 outputDir 속성으로 어떤 위치에 생성된 스니펫을 저장할 것인지 지정했다.

이 속성과 클래스 내의 설정이 어우러져 각 클래스의 이름에 맞는 디렉토리에 스니펫들이 생성되어 저장된다.

<br>

이후는 build.gradle에 설정된 mockMvc 문서화 라이브러리를 바탕으로 테스트 결과를 문서화 처리하면 된다.

```java
    @Test
    @DisplayName("수입 생성 성공 1 : 태그가 null인 경우")
    @Order(1)
    void postIncomeTest_Success1() throws Exception {
        // given
        IncomeDto.Post postDto = new IncomeDto.Post(
                10000, "testName", "testMemo", LocalDate.of(2024, 1, 1), null
        );

        // LocalDate 커스텀 직렬화
        Gson gson = new GsonBuilder()
                .registerTypeAdapter(LocalDate.class, new LocalDateSerializer())
                .serializeNulls() // null 값 포함
                .create();

        String content = gson.toJson(postDto);

        // when
        mockMvc.perform(
                        post("/income/post")
                                .header("Authorization", token)
                                .accept(MediaType.APPLICATION_JSON)
                                .contentType(MediaType.APPLICATION_JSON)
                                .characterEncoding("UTF-8")
                                .content(content)
                )

                // then
                .andExpect(status().isCreated())
                .andDo(print())

                // documentation
                .andDo(document("IncomeIntegrationTest/postIncomeTest_Success1",
                        preprocessRequest(prettyPrint()),
                        preprocessResponse(prettyPrint()),
                        requestHeaders(
                                headerWithName("Authorization").description("JWT 액세스 토큰")
                        ),
                        requestFields(
                                fieldWithPath("money").description("수입 금액"),
                                fieldWithPath("incomeName").description("수입명"),
                                fieldWithPath("memo").description("수입에 대한 간단한 메모"),
                                fieldWithPath("date").description("수입 날짜"),
                                fieldWithPath("incomeTag").description("수입 태그 이름")
                        ),
                        responseFields(
                                fieldWithPath("incomeId").description("수입 식별자"),
                                fieldWithPath("money").description("수입 금액"),
                                fieldWithPath("incomeName").description("수입명"),
                                fieldWithPath("memo").description("수입에 대한 간단한 메모"),
                                fieldWithPath("date").description("수입 날짜"),
                                fieldWithPath("incomeTag").description("수입 태그")
                        )
                ));
    }
```

가독성이 좋게 작성한다고 주석으로 각 부분을 나누었다.

then 까지는 기존에 작성된 테스트 케이스와 동일하고, documentation 파트 부터 mockMvc를 통해 문서화를 진행한다.

어느 위치에 스니펫이 저장될 것인지를 지정하고, JSON 형식의 요청과 응답 데이터를 보기 좋게 설정하는 preprocessRequest/Response 옵션을 적용했다.

그 다음 헤더, 요청 바디, 응답 바디의 내용을 스니펫으로 추출하는 설정이다.

이 설정들이 하나라도 어긋나 있거나 JSON 필드의 속성과 이름이 다르다면 문서화에 실패한다.

<br>

모든 테스트 케이스에 걸쳐 이러한 설정들을 적용하고 나면
테스트 클래스를 전체 실행하여 스니펫을 생성해준다.

그러면, 스니펫들이 아래와 같이 빌드된다.

![](/assets/img/projects/salog/build_snippets.png)

이제 생성된 스니펫을 활용해 문서를 만들어야하는데,
이 문서의 서식을 결정할 파일을 하나 생성해야한다.

![](/assets/img/projects/salog/adoc_file_src.png)

이렇게 adoc 확장자명을 가진 파일들을 생성하고

```java
= API Documentation
:toc: left
:toclevels: 2
:sectlinks:
:sectanchors:
:doctype: book
:icons: font
:encoding: utf-8
:docdir: {docdir}

= API Documentation
:revnumber: 0.0.1-SNAPSHOT
:revdate: 2024-06-14 14:32:58 +0900

= 수입 생성

== pass

=== 수입 생성_성공 1 : 태그가 Null인 경우

include::{docdir}/build/generated-snippets/IncomeIntegrationTest/postIncomeTest_Success1/http-request.adoc[]
include::{docdir}/build/generated-snippets/IncomeIntegrationTest/postIncomeTest_Success1/request-headers.adoc[]
include::{docdir}/build/generated-snippets/IncomeIntegrationTest/postIncomeTest_Success1/request-fields.adoc[]
include::{docdir}/build/generated-snippets/IncomeIntegrationTest/postIncomeTest_Success1/http-response.adoc[]
include::{docdir}/build/generated-snippets/IncomeIntegrationTest/postIncomeTest_Success1/response-fields.adoc[]

=== 수입 생성_성공 2 : 태그가 존재하지 않는 경우

include::{docdir}/build/generated-snippets/IncomeIntegrationTest/postIncomeTest_Success2/http-request.adoc[]
include::{docdir}/build/generated-snippets/IncomeIntegrationTest/postIncomeTest_Success2/request-headers.adoc[]
include::{docdir}/build/generated-snippets/IncomeIntegrationTest/postIncomeTest_Success2/request-fields.adoc[]
include::{docdir}/build/generated-snippets/IncomeIntegrationTest/postIncomeTest_Success2/http-response.adoc[]
include::{docdir}/build/generated-snippets/IncomeIntegrationTest/postIncomeTest_Success2/response-fields.adoc[]

=== 수입 생성_성공 3 : 태그가 이미 존재하는 경우

include::{docdir}/build/generated-snippets/IncomeIntegrationTest/postIncomeTest_Success3/http-request.adoc[]
include::{docdir}/build/generated-snippets/IncomeIntegrationTest/postIncomeTest_Success3/request-headers.adoc[]
include::{docdir}/build/generated-snippets/IncomeIntegrationTest/postIncomeTest_Success3/request-fields.adoc[]
include::{docdir}/build/generated-snippets/IncomeIntegrationTest/postIncomeTest_Success3/http-response.adoc[]
include::{docdir}/build/generated-snippets/IncomeIntegrationTest/postIncomeTest_Success3/response-fields.adoc[]

...
  
== Version 0.0.1-SNAPSHOT
Last updated 2024-06-14 14:32:58 +0900
```

이런 식으로 문서에 필요한 내용을 담아준다.

그 다음 gradlew test 명령을 실행하여 전체 테스트를 실행하면 되는데, 문서화를 위해 asciidoctor 옵션까지 적용하여 실행한다.

간단히 터미널에서 루트 디렉토리로 이동하여

```
./gradlew test asciidoctor   
```

위 명령어를 실행하면 생성되었던 스니펫을 바탕으로 문서화가 자동으로 이루어진다.

<br>

위와 같은 과정을 거치고 나면 build/docs/asciidoc 디렉토리에

![](/assets/img/projects/salog/build_api_docs.png)

이렇게 html 문서들이 생성되고

이 문서를 브라우저나 여타 프로그램으로 열면

![](/assets/img/projects/salog/api_docs_final.png)

이런 문서인 것을 확인할 수 있다.

<br>

이를 바탕으로 협업이나 유지보수 시 문서를 사용할 수 있게 되는 것이다.

<br>
<br>
<br>

***

### 후기

샐로그 프로젝트를 유지보수하면서 테스트 케이스를 전 기능에 걸쳐 작성하고, 문서화까지 진행해 보았다.

솔직히 이 과정을 거치면서 뼈저리게 느끼는 것은 테스트 작성과 문서화는 개발 중에 하는 것이 맞다는 것이다.

슬라이스 테스트는 하지 않더라도 서비스 로직 하나가 완성되면 유닛 테스트는 필수이고,
API 하나가 완성되면 통합 테스트는 필수이다.

또한 통합 테스트가 진행되면 원활한 협업 커뮤니케이션을 위해 문서화도 필수라고 생각한다.

애당초 계획된 것과 다른 경우가 많기 때문에 이런 문서라도 하나 뽑아 놓으면 모두가 행복할 것이라고 생각한다.

<br>

담당한 도메인들에 대한 커버리지가 80% 이상이므로 중요한 부분은 테스트가 진행되었다고 판단하고
다음은 성능 테스트이다.

성능 테스트를 진행한 다음 개선해보는 유지보수를 진행해 볼 생각이다.

<br>

마지막으로 담당한 모든 도메인의 통합 테스트 커버리지 자료로 마무리한다.

#### 회원
![](/assets/img/projects/salog/memberIntegrationTest_Coverage_byIntelliJ.png)

<br>

#### 수입
![](/assets/img/projects/salog/IncomeIntegrationTest_Coverage_byIntelliJ.png)

<br>

#### 고정수입
![](/assets/img/projects/salog/FixedIncomeIntegrationTest_Coverage_byIntelliJ.png)

<br>

#### 예산
![](/assets/img/projects/salog/BudgetIntegrationTest_Coverage_byIntelliJ.png)

<br>

테스트 대상이 그리 많진 않지만 최소 80% 이상 커버하는 걸 보니 흐뭇하다.
