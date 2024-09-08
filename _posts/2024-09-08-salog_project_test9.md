---
title: "프로젝트:샐로그 / 코드 정적 분석 도구 적용 1 (sonarLint)"
author: Jeremiah Lee
date: 2024-09-08
categories: [ 프로젝트, 샐로그, 정적 테스트 ]
tags: [프로젝트, 샐로그, 정적 테스트]
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

코드 정적 분석 도구에 대한 내용을 알게 되었고 대표적으로 사용되는 도구인 sonarLint와 sonaQube에 대해 살펴보았다.

실제로 사용해보면서 구체적으로 파악되기 시작했는데, 우선 sonarLint의 경우 인텔리제이의 플러그인 설치를 통해 손쉽게 사용 가능하다.

그러나 sonarLint는 서버 전체 코드를 분석해 수치화 해주는 것이 아닌 실시간으로 보고 있는 화면에 작성되어있거나 작성하는 코드에 대한 실시간 분석만 해준다.

나는 코드를 전체적으로 분석하여 내가 맡은 파트에 대해 어느정도 코드나 구조적으로 개선을 하는 것이 목표이기 때문에 sonarLint만 활용해서는 이 목표를 달성할 수 없다고 생각했다.

그래서 sonarLint 뿐만 아니라 sonarQube를 직접 설치하여 활용해보았고 해당 내용을 이 포스트에 작성한다.

<br>
<br>
<br>

### sonarLint 설치

sonarLint의 경우 인텔리제이의 플러그인으로 설치할 수 있으며 별도의 추가 작업 없이 설치만 해도 사용 가능하다.

물론 상황에 따라 설정을 할 수 있지만 해당 내용은 일단 넘어가고, sonarLint를 적용했을 때 어떤 모습을 보이는 지 먼저 작성한다.

<br>

![](/assets/img/projects/salog/code_analysis/marketplace_sonarLint.png)

이렇게 인텔리제이 플러그인 마켓플레이스에서 sonarLint를 받아서 설치하고 사용할 수 있다.

<br>
<br>
<br>

### sonarLint 활용

설치 후 인텔리제이를 재실행하고 코드를 분석하고자하는 클래스를 열어보면

![](/assets/img/projects/salog/code_analysis/sonarLint_mark.png)

이와 같은 sonarLint 마크가 우측 상단에 노출되고 해당 마크를 클릭하면

![](/assets/img/projects/salog/code_analysis/sonarLint_analysis.png)

이런 식으로 하단 콘솔 창에 분석 결과가 실시간으로 나온다.

<br>

이 분석 창을 상세히 살펴보면

![](/assets/img/projects/salog/code_analysis/sonarLint_analysis_result.png)

어떤 것이 이슈인지 샘플 코드와 함께 보여준다.

이를 통해 규칙에 맞게 코드를 수정할 수 있다.

<br>

현재 발생한 문제는

```java
List<LedgerTagDto.MonthlyResponse> tagIncomes = totalIncomeByTag.stream()
  .map(obj -> new LedgerTagDto.MonthlyResponse((String) obj[0], obj[1] != null ? (Long) obj[1] : 0))
  .collect(Collectors.toList());
```

이와 같이 자바에서 Stream API를 사용할 때 Collectors.toList() 를 사용해서 그렇다.

이 Collectors.toList()는 가변적인 리스트를 반환하기 때문에 수정이 가능하고, 이에 따라 데이터가 변질 될 우려가 있다는 것이다.

그래서 Java 10 버전 부터 추가된 Stream.toList() 메서드를 사용해 불변 리스트를 반환하도록 하고, 코드의 의도를 명확히 하라는 주의를 준 것이다.

<br>

이후 문제가 되는 해당 코드를 규칙에 맞게 수정하고 나면

![](/assets/img/projects/salog/code_analysis/sonarLint_fix_code.png)

이렇게 강조 표시가 없어지고 sonarLint에서 더 이상 추적하지 않는다.

<br>

![](/assets/img/projects/salog/code_analysis/sonarLint_comple_exam.png)

이해 하기 쉽게 sonarLint 에서도 위와 같이 수정 예시를 보여준다.

<br>
<br>
<br>

### sonarLint 상세 규칙 설정

혹여나 팀 별로 규칙이 있거나 특정 규칙을 수정하고 싶은 경우에는

![](/assets/img/projects/salog/code_analysis/sonarLint_settings.png)

File -> Settings -> tools -> SonarLint 위치에 접근해서 상세한 규칙을 설정 할 수 있다.

<br>
<br>
<br>

### 후기

sonarLint의 사용법에 대해 간단히 알아보았다.

처음에는 sonarLint만 활용해서 코드의 품질을 높여볼 생각이었는데, 결국 서버 전체의 코드 품질을 높이는 작업을 해야하기 때문에
이 sonarLint 하나만으로는 수치화가 되지 않아서 불편하다고 생각이 들었다.

그래서 다음 포스트에서는 sonarQube 를 설치, 적용하고 코드 품질에 대한 종합적인 보고서를 출력해 개선 전, 후를 비교하는 작업을 진행할 것이다.

순서는,

1. sonarQube 로 수정 전 보고서 출력
2. sonarLint 로 맡은 파트 각 클래스 코드 품질 향상
3. sonarQube 로 수정 후 보고서 출력

이 될 것이다.
