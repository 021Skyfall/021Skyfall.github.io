---
title: "프로젝트:샐로그 / 코드 정적 분석 도구 적용 2 (sonarQube - 2)"
author: Jeremiah Lee
date: 2024-09-20
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

```
./gradlew sonar \
  -Dsonar.projectKey=salog \
  -Dsonar.projectName='salog' \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.token=***
```

이 명령어를 사용하여 sonarQube로 스캔하기 전에 몇 가지 해야할 설정이 있다.

<br>
<br>
<br>

### Sonar Scanner

SonarQube 가 프로젝트를 스캔할 수 있도록 스캐너 프로그램을 

[SonarScanner CLI](https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/scanners/sonarscanner/)

여기서 다운 받고, 시스템 환경 변수로 스캐너의 bin 디렉토리를 추가한다.

추가했다면 sonar-scanner --version 명령어를 CMD에 입력하여 잘 되어있는지 확인한다.

<br>
<br>
<br>

### build.gradle 설정 추가

스캐너 까지 설정을 완료해서 위 명령어로 스캔을 시작하려 했으나 

```
* What went wrong:
Task '.projectKey=salog' not found in root project 'salog'.
```

이런 에러가 발생했다.

프로젝트 키가 잘 못 되었다는 건데 일단 설정 자체에는 문제가 없었고, 

```
./gradlew sonar --info
```

이 명령어로 조금 더 자세한 에러 내용을 보았더니

```
What went wrong: Execution failed for task ':sonar'.
Not authorized. Analyzing this project requires authentication. 
Please check the user token in the property 'sonar.token' or the credentials in the properties 'sonar.login' and 'sonar.password'.
```

토큰이 맞지 않는다고 한다...

일단 토큰 자체에도 문제가 없었기 때문에 이를 해결하기 위한 다른 방법으로, build.gradle에 직접 sonarQube 설정을 추가하기로 했다.

<br>

```
sonarqube {
	properties {
		property "sonar.projectKey", "salog"
		property "sonar.projectName", "salog"
		property "sonar.host.url", "http://localhost:9000"
		property "sonar.token", "토큰"
	}
}
```

이렇게 sonarQube 에 대한 설정을 직접 적어넣었고,

```
BUILD SUCCESSFUL in 22s
```

빌드에 성공했다.

<br>
<br>
<br>

### SonarQube 결과 (수정 전)

![](/assets/img/projects/salog/code_analysis/sonarqube_result1.png)

스캔에 대한 최초 결과이다.

우선 각 항목에 대해 어떤 것인지 부터 살펴보자.

#### 1. Quality Gate
  - 코드의 품질 기준을 의미하는데 Passed로 나오며, 설정한 품질 기준을 통과했음을 의미한다. 현재 sonarQube의 기본 품질 기준을 따른다.

#### 2. Security
  - 현재 0 Open issues 로 나오며 보안 관련 문제가 없음을 나타낸다. 즉, 코드에 보안 취약점이 발견되지 않았다는 의미다.

#### 3. Reliability
  - 코드의 신뢰성과 관련한 문제를 나타낸다. 주로 코드의 안정성과 관련된 이슈로, 여러 가지 문제들이 포함될 수 있다. 현재는 5개의 문제가 발견된 상태이다.

#### 4. Maintainability
  - 유지보수성과 관련한 문제를 나타낸다. 코드의 가독성, 복잡성, 중복성 등을 포함할 수 있으며 현재 257개의 문제가 발견된 상태이다.

#### 5. Coverage
  - 코드 테스트 커버리지를 의미한다. 현재 0%로 나타나는데, 이는 SonarQube 스캔 시 작성되어 있는 테스트가 실행되지 않아서 그렇다.

#### 6. Duplications
  - 코드의 중복 정도를 나타낸다. 현재 1.0%로, 5,500줄의 코드에서 중복이 발견되었다는 의미이다.

#### 7. Accepted Issues
  - 해결하지 않은 이슈가 없음을 나타낸다. 발견된 문제 중에서 승인된 문제는 없다는 의미이다.

#### 8. Security Hotspots
  - 보안 핫스팟은 잠재적인 보안 문제를 나타내며, 현재 6개 발견되었다. 이는, 반드시 문제가 있는 것이 아니지만 해당 코드에 대한 검토가 필요하다는 의미이다.

<br>

각 결과의 이미는 위와 같으며, 여기서 커버리지에 대해 조금 더 알아보고 가자면,
이 커버리지의 경우 스캔 시 테스트가 빌드되지 않아서 그렇다.

이를 해결하기 위해서는 Jacoco와 같은 Java 애플리케이션 코드 커버리지를 측정하는 플러그인이 필요하며, 이 플러그인으로 커버리지를 측정하고 SonarQube에 정보를 넘겨주는 것으로
해당 문제를 해결할 수 있다.

대표적으로 Jacoco를 사용하는 이유는 SonarQube와 쉽게 통합할 수 있도록 설계되어 있기 때문이며, 이를 통해 커버리지 리포트를 SonarQube에 간단하기 전달할 수 있다.

하지만 이번 포스트에서는 SonarQube를 활용하여 코드의 품질을 향상 시키는 것이 목적이며, 코드에 대한 테스트 커버리지의 경우 이전 테스트 포스트에서 담당한 파트를 다룬 적이 있다.

해당 포스트를 보면 전체 테스트 커버리지를 85% 이상 달성한 것을 볼 수 있기 때문에 이번 포스트에서는 Jacoco 플러그인을 설치하여 SonarQube와 연동하는 작업은 하지 않았다.

<br>
<br>
<br>

### 스캔 결과를 바탕으로 코드 수정

sonarQube 를 통해 스캔을 한 다음 각 문제점을 수정하기 위해 대시보드를 살펴보자.

앞선 스캔 결과 리포트에서 문제점을 찾은 파트로 넘어가면

![](/assets/img/projects/salog/code_analysis/sonarqube_scan_result_handle.png)

이와 같이 어떠한 문제점이 발견되었는지 확인할 수 있다.

여기서 각 문제점을 클릭하면

![](/assets/img/projects/salog/code_analysis/sonarqube_result_inCode.png)

작성한 코드로 접근되며 해당 코드에 어떤 문제가 있는지까지 보여준다.

이를 통해 sonarQube 에서 스캔한 결과를 바탕으로 문제가 있는 코드를 수정하고 코드의 품질을 향상 시킬 수 있다.

<br>

우선 중복 코드 라인을 살펴보니 팀원의 담당 파트 뿐이라 건들기 애매했다.

그래서 잠재적 문제가 있을 수 있는 보안 핫스팟을 제외하고, 
그 외에 유지보수성과 관련된 코드 이슈들은 내용이 너무 많아 블로그에 작성하기에는 적합하지 않다고 판단했다.

그럼 남은 이슈는 평가가 D로 가장 낮은 코드 신뢰성 부분인데, 이 부분에 대한 내용을 수정하고 A로 향상 시켜보자.

<br>
<br>
<br>

### 코드 신뢰성 향상

우선 주요 문제는 다섯 가지이다.

이 5개의 문제 중 한 가지는 담당 파트가 아니기 때문에 팀원에게 일러주었으며, 나머지 부분에 대한 수정을 거쳤다.

제일 처음 문제는 JwtTokenizer 클래스에서 발생한 문제이다.

![](/assets/img/projects/salog/code_analysis/sonarqube_field_injection_problem.png)

이 문제는 의존성에 대한 필드 주입을 해제하고 대신 생성자 주입을 적용하라는 의미인데,
내부를 살펴보면

![](/assets/img/projects/salog/code_analysis/sonarqube_injection_prom.png)

이렇게 @Autowired 어노테이션을 사용해서 필드 주입하는 것보다 생성자를 통한 의존성 주입을 하라고 나온다.

여기서 이 이슈가 왜 문제인지를 좀 더 자세히 알고 싶다면

![](/assets/img/projects/salog/code_analysis/sonarqube_injection_prob_why_tab.png)

Why is this an issue? 탭에서 확인할 수 있으며
내용을 요약하면 필드 주입은 의존성이 명확하지 않고 유효하지 않은 객체를 생성할 위험이 있으며, 테스트를 어렵게 만든다. 대신 생성자 주입을 사용하는 것이 더욱 안전하고 유지보수에 용의하다.

정도가 될 수 있다.

또한, 해결 방법도 친절히

![](/assets/img/projects/salog/code_analysis/sonarqube_injection_prob_solve_tab.png)

알려준다.

<br>

해당 내용을 바탕으로 JwtTokenizer에 MemberRepository를 주입하는 방식을 생성자 방식으로 변경했으며,
문제 없이 실행되는 것을 확인했다.

이제 다시 sonarQube로 스캔하면

![](/assets/img/projects/salog/code_analysis/sonarqube_result2.png)

이와 같이 코드 신뢰성 이슈가 5 -> 4 로 문제 하나가 해결된 것을 볼 수 있다.

또한, 유지보수성 문제가 있는 코드도 257 -> 256 으로 하나가 줄었다.

<br>

한 가지 주의해야할 점은 코드를 수정하고서 불필요한 import 등을 삭제하지 않으면 스캔 후 Quality Gate 가 실패했다고 뜨니 조심하자.

<br>

이외에도 

```java
// 조회된 사용자 정보를 바탕으로 클레임 생성
Map<String, Object> newClaims = new HashMap<>();
newClaims.put("roles", member.get().getRoles());
newClaims.put("memberId", member.get().getMemberId());
newClaims.put("username", member.get().getEmail());
```

JwtTokenizer의 사용자 정보 바탕으로 클레임 생성 로직 중, member 가 존재하지 않는 경우를 핸들링하지 않던 것을

```java
// 조회된 사용자 정보를 바탕으로 클레임 생성
Map<String, Object> newClaims = new HashMap<>();
member.ifPresent(m -> {
  newClaims.put("roles", member.get().getRoles());
  newClaims.put("memberId", member.get().getMemberId());
  newClaims.put("username", member.get().getEmail());
});
```

이와 같이 수정했다.

<br>

세번 째 문제였던

OauthService 에서 소셜 로그인한 회원을 정보를 DB에 저장하고 해당 내용을 바탕으로 jwt를 생성하던 로직인

```java
if (verifiedEmail(email)) {
  oauthMember = memberRepository.findByEmail(email).get();
} else {
  ...
```

이 로직에서 Optional의 값에 접근하기 전에 isPresent() 또는 isEmpty()를 사용하여 안전성을 높이는 것이 좋기 때문에

```java
if (verifiedEmail(email)) {
  Optional<Member> optionalMember = memberRepository.findByEmail(email);
  if (optionalMember.isPresent()) {
    oauthMember = optionalMember.get();
  } else {
    // 이메일이 존재하지 않을 경우의 처리
    throw new NoSuchElementException("No member found with email: " + email);
  }
  
    ...
```

이렇게, 우선적으로 정보가 없는 경우의 예외 상황을 핸들링했다.

<br>

마지막 문제는 본인 인증 중, 이메일로 난수를 보내는 데, 이 난수 생성기에 문제가 있었다.

```java
    public String generateRandomCode(int length) {
        String characters = "난수~";
        StringBuilder sb = new StringBuilder(length);
        Random random = new Random();
```

여기서는 "Save and re-use this 'Random'" 라는 경고가 발생했으며,
이는 Random 객체를 매번 새로 생성하는 것보다 재사용하는 것이 추천된다는 의미이다.

특히 이 객체가 여러 번 사용될 경우 불필요한 객체 생성을 초래하기 때문에 성능이 저하될 수 있다.

```java
@Component
public class RandomGenerator {
  private final SecureRandom secureRandom; // SecureRandom 객체를 멤버 변수로 선언

  public RandomGenerator() {
    this.secureRandom = new SecureRandom(); // 생성자에서 초기화
  }

  public String generateRandomCode(int length) {
    String characters = "난수생성";
    StringBuilder sb = new StringBuilder(length);

    for (int i = 0; i < length; i++) {
      int index = secureRandom.nextInt(characters.length());
      char randomChar = characters.charAt(index);
      sb.append(randomChar);
    }

    return sb.toString();
  }
}
```

그래서 이와 같이 Random 객체를 멤버 변수로 선언하고, 생성자에서 초기화하도록 변경했다.

이로써 random 객체가 클래스의 모든 메서드에서 재사용될 수 있도록 변경했으며 이 random 객체를 사용하여 랜덤 코드를 생성하게 된다.

또한, Random 클래스는 생성된 난수를 예측할 수 있는 경우가 있을 수 있기 때문에 보안을 향상 시키기 위해 예측 불가능한 난수 생성기인 secureRandom 클래스로 변경했다.

<br>

이제 코드의 신뢰성을 높이기 위한 작업은 전부 완료했다.

<br>
<br>
<br>

### SonarQube 결과 (수정 후)

수정 후 결과 리포트를 보기 전

sonarQube 토큰을 시스템 환경 변수로 설정하고 

```
sonarqube {
	properties {
		property "sonar.projectKey", "salog"
		property "sonar.projectName", "salog"
		property "sonar.host.url", "http://localhost:9000"
		property "sonar.token", System.getenv("sonarQube_token")
	}
}
```

build.gradle 내부 sonarQube 플러그인 설정을 위와 같이 변경했다.

<br>

그 다음 위에서 코드 신뢰성을 확보하는 작업을 진행한 수정 후 sonarQube 스캔 결과를 보자.

![](/assets/img/projects/salog/code_analysis/sonarqube_result3.png)

코드 신뢰성 관련 이슈들을 수정하고 나니 D -> A 로 등급이 올랐다.

또한 유지보수 이슈 코드 수도 257 -> 255로 줄었다.

<br>

이런 식으로 SonarResource 의 sonarQube, sonarLint 를 통해 코드의 품질을 높일 수 있으며, 전체적인 애플리케이션의 품질을 향상 시킬 수 있다.

<br>
<br>
<br>

### 후기

두 포스트에 걸쳐 SonarQube 를 통해 전체 코드 품질에 대한 리포트를 생성하고, 이슈가 있는 코드들을 어시스트 받아 수정했다.

이외에도 SonarLint의 사용법을 알았기 때문에 추후 개발할 때도 이 SonarLint가 코드를 실시간 추적하여 주의해야할 점을 알려주기 때문에 놓칠 수 있는 보안 문제나 의존성 문제, 혹은
중복성 문제 등을 사전에 파악할 수 있을 것이다.

이를 통해 코드 품질을 높이고 더 나아가 애플리케이션 자체의 품질을 향상 시킬 수 있게 되었다.

<br>

다음으로 해볼 만한 작업은 Jacoco 플러그인을 샐로그에 설치하여 SonarQube에 연결하고, 지금은 SonarQube 로 확인할 수 없는 테스트 커버리지를 표시하게 하는 방법이나
보안 핫스팟 문제를 해결하는 방법 등이 있을 것이다.

<br>

우선은 전체 코드 품질을 향상 시킨 것이 개운하다.
