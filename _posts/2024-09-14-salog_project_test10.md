---
title: "프로젝트:샐로그 / 코드 정적 분석 도구 적용 2 (sonarQube - 1)"
author: Jeremiah Lee
date: 2024-09-14
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

계속해서 코드 정적 분석 도구를 활용해보는 작업을 진행 중이다.

이전 포스트에서는 SonarLint 를 활용하여 실행 전 작성된 코드를 실시간 분석하고, 이 분석 결과를 바탕으로 몇 가지 코드를 수정해보았다.

이를 통해 코드 품질을 향상 시키고, 팀 별로 협업 중이라면 임의의 스타일을 정해 더 견고한 시스템을 제작할 수 있을 것이다.

<br>

하지만 SonarLint 만을 가지고는 전체적인 분석 결과에 대한 보고서를 출력해낼 수 없기 때문에 SonarQube를 사용해보고자 했으며,
이 SonarQube 를 사용하여 샐로그 프로젝트의 전체 코드 상태를 출력해본 뒤 SonarLint를 활용해 수정이 필요한 부분에서 도움을 받고 
코드 품질을 향상 시켜볼 예정이다.

<br>

우선 SonarQube 의 사용법을 샐로그를 통해 기록하고, 다음 포스트에서는 SonarLint로 각 코드를 수정하고 SonarQube 스캔 결과를 전, 후로 비교해 볼 것이다.

<br>
<br>
<br>

### SonarQube 실행

가장 먼저 SonarQube 를 설치한다.

[SonarSource 공식 홈페이지](https://www.sonarsource.com/products/sonarqube/downloads/)
에 접속해서 SonarQube를 커뮤니티 버전으로 다운받고 사용하면 된다.

이 커뮤니티 버전으로 무료 사용할 수 있으며 다운 받고 살펴보면 각 OS에 맞는 파일들이 들어가 있다.

<br>

현재 테스트 해볼 환경이 윈도우 환경이기 때문에 윈도우 폴더로 접근해서 StartSonar.bat을 실행시킨다.

이 프로그램을 실행시키면 SonarQube 대시보드가 9000번 포트를 통해 실행되고 브라우저로 접속해서 확인할 수 있다.

http://localhost:9000

<br>
<br>
<br>

### 실행 문제 해결

원래는 위의 과정을 거치면 별 다른 문제 없이 실행되어야 하는데, 나는 실행이 안되고

```
WARNING: Please consider reporting this to the maintainers of org.sonar.process.PluginSecurityManager
WARNING: System::setSecurityManager will be removed in a future release
2024.11.07 16:36:40 INFO  app[][o.s.a.SchedulerImpl] Process[Web Server] is stopped
2024.11.07 16:36:40 INFO  app[][o.s.a.SchedulerImpl] Process[ElasticSearch] is stopped
2024.11.07 16:36:40 INFO  app[][o.s.a.SchedulerImpl] SonarQube is stopped

```

이런 에러 로그가 출력되면서 바로 프로그램이 꺼져버렸다.

<br>

SonarQube 실행 안됨을 키워드 삼아 몇 가지 해결법을 찾아보았고

주로 나타날 수 있는 문제가 세 가지 였다.

1. 자바 버전 확인
2. 메모리 조정
3. 포트 충돌 확인 netstat -ano | findstr :9000 / netstat -ano | findstr :9001

<br>

자바 버전은 17버전이 깔려있는데, SonarQube는 11버전 이상 부터 호환된다고 공식 문서에 나와 있어서 문제가 아니라고 생각했으며,
혹여나 시스템 환경 변수로 설정 해둔 JAVA_HOME 이 제대로 연결이 안되어 있나 싶어서 확인해봤는데 이 마저도 아니었다.

그래서 이 SonarQube가 자바를 못 찾는건가 싶어서

StartSonar.bat 파일을 텍스트 에디터로 열고, [참고 블로그](https://hippo0718.tistory.com/43)에서 본대로 자바의 경로를 직접 지정해 주었다.

그러나 이 방법도 통하지 않았다.

<br>

두 번째로 메모리를 조정해봤는데, SonarQube 폴더의 conf/sonar.properties 파일을 에디터로 열어

```
sonar.web.javaOpts=-Xmx512m -Xms128m -> sonar.web.javaOpts=-Xmx512m -Xms256m
sonar.ce.javaOpts=-Xmx512m -Xms128m -> sonar.ce.javaOpts=-Xmx512m -Xms256m
```

위와 같이 메모리 할당량을 늘려주었다.

하지만 이 방법으로도 해결되지 않았다.

<br>

마지막으로 혹시나 9000, 9001 포트가 사용 중인지 확인하기 위해 cmd 콘솔 창을 열어 확인해 보았지만
역시나 그 어떤 프로그램도 사용하고 있지 않았다.

<br>

이후로 혹시 또 다른 로그가 기록 되어 있는지 확인하기 위해 SonarQube의 로그 폴더로 접근해
sonar.log, ce.log, es.log 파일들을 살펴보았고, 엘라스틱서치 로그에서 문제를 찾을 수 있었다.

```

2024.11.07 16:44:46 WARN  es[][o.e.n.NativeAccess] Unable to load native provider. Native methods will be disabled.
java.lang.UnsatisfiedLinkError: Unable to load library 'zstd':
지정된 모듈을 찾을 수 없습니다.

지정된 모듈을 찾을 수 없습니다.

지정된 모듈을 찾을 수 없습니다.

...
```

이와 같은 로그가 출력되어 있어, 관련 내용을 찾아보았고, 어디서는 네이티브 라이브러리를 사용하지 않도록    
config/elasticsearch.yml 파일에서

```
native:false
```

옵션을 넣고 끄라고는 했지만 성능이 저하될 수 있는 방법이기 때문에 딱히 내키지 않았다.

그래서 조금 더 구글링을 해보았고, 결국 sonarQube 공식 커뮤니티로 들어가게 되었다.

<br>

공식 커뮤니티에서 나와 같은 문제를 마주한 사람이 있었는데,

https://community.sonarsource.com/t/native-library-win32-x86-64-zstd-dll-not-found-in-resource-path/129667

제일 최근 QnA고 서로 답변이 너무 느려 문제 해결에 대한 내용은 없었다...

그래서 조금 더 둘러보았고,

https://community.sonarsource.com/t/elastisearch-error-when-starting-sonarqube-community-on-windows/128206/6

여기서 나와 동일한 문제가 있는 사람을 또 발견했다.

이 사람은 스스로 몇 가지 시도를 해보다가 해결을 했으며, 그 방법이 상당히 당황스러웠다.

<br>

해결 방법은 단순히 컴퓨터 디스크 용량을 비우는 것.

사실 이 때 나도 디스크 용량이 20기가 정도가 남아있었어서 큰 용량은 아니지만 적은 용량도 아니고, 설마 디스크를 비우는 것으로 이 문제가 해결 될까? 싶었다.

그래도 혹시 몰라 100기가 이상을 비우고서 재부팅하고 다시 SonarQube 를 실행했더니...

잘 돌아간다...

너무 어이가 없었다.

20기가 정도면 그래도 용량이 넉넉한 편인데, 엘라스틱서치 시동에 얼마나 많은 가상 용량을 차지하기 때문에 이런걸까...

<br>

여튼 디스크 용량을 확보하는 것으로 문제를 해결 했으며,
이걸로도 해결이 안되었다면 zstd.dll을 수동으로 다운 받아 엘라스틱서치 디렉토리에 넣어볼 생각이었다.

<br>
<br>
<br>

### SonarQube 사용

이제 SonarQube 를 설치하고 실행까지 진행했으니 사용하는 방법에 대해 알아보자.

<br>

우선 SonarQube 대시보드에 접근한다.

```
http://localhost:9000
```

보통 SonarQube는 9000, 9001번 포트에서 실행한다.

초기 화면 접근 시 로그인을 하도록 되어있는데, 기본적으로 admin / admin 이지만 이 id/pw 로 접근하고 나면 pw를 변경하도록 유도한다.

비밀번호를 변경하고 접속하면

![](/assets/img/projects/salog/code_analysis/sonarqube_start_view.png)

이런 화면이 출력된다.

여기서 선택적으로 CI/CD 통합하여 자동화된 분석을 설정할 수 있지만 지금은 로컬에서 테스트 중이기 때문에 Local project 를 생성한다.

![](/assets/img/projects/salog/code_analysis/sonarqube_local_project_create.png)

생성 페이지로 넘어가면 위와 같이 출력되고 여기서 이름과 키를 지정한다.

그 다음은 새 코드에 대한 설정인데, 이는 기본으로 두었다.

<br>

이후,

![](/assets/img/projects/salog/code_analysis/sonarqube_analysis_method.png)

이와 같이 어떤 레포지터리에 있는 소스 코드를 지정할 지 선택하는 란이 나타나는데, 이 경우 마찬가지로 로컬을 선택한다.

<br>

![](/assets/img/projects/salog/code_analysis/sonarqube_token_prov.png)

이제 연결할 토큰을 만료 기한을 설정한 다음 발급 받는다.

다음으로 자신의 프로젝트에 맞게 빌드를 골라주면

![](/assets/img/projects/salog/code_analysis/sonarqube_analysis_projects.png)

이와 같이 플러그인 추가 방법과 sonarqube 실행 명령어를 보여준다.

<br>

이제 샐로그 프로젝트에 sonarqube 플러그인을 추가하고 프로젝트 최상위 디렉토리로 이동해 명령어를 사용해서 리포트를 생성해보자.

<br>
<br>
<br>

### 참고

- [기은P 티스토리](https://narup.tistory.com/181)
- [샘오리_개발노트](https://samori.tistory.com/78)
- [HSRyuuu](https://innovation123.tistory.com/260)
























