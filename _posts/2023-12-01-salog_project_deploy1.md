---
title: "프로젝트:샐로그 / 배포 1 (Github Actions/Docker/AWS) + Docker"
author: Jeremiah Lee
date: 2023-12-01
categories: [ 프로젝트, 샐로그, 도커 ]
tags: [프로젝트, 샐로그, 배포, Github Actions, 도커, AWS]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Project_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

처음 예고했던대로 Github Actions - AWS를 활용한 자동 배포 환경을 만드는 데에 더 해 중간에 Docker가 껴있는 모양으로 배포를 진행했다.

어떤 식으로 진행했는지 기록하기 전에 가장 먼저 참고했던 블로그는 아래와 같다.
1. [글쓰는 개발자](https://souljit2.tistory.com/74)
2. [대학원생 개발자의 일상](https://gr-st-dev.tistory.com/731)
3. [공부하는 개발자의 이야기](https://wnsgml972.github.io/setting/2020/07/20/docker/)
4. [jomminii_before](https://velog.io/@devmin/Docker-deployment)
5. [Subicura's Blog](https://subicura.com/2016/06/07/zero-downtime-docker-deployment.html)
6. [tilsong.log](https://velog.io/@tilsong/%EC%BD%94%EB%93%9C-Push%EB%A1%9C-%EB%B0%B0%ED%8F%AC%EA%B9%8C%EC%A7%80-Github-Actions-Docker) (도움을 제일 많이 받았다.)

이해와 활용이 어려운 만큼 참고했던 블로그도 많다.

사실 각자의 상황이 다르기 때문에 보자마자 단박에 이해하고 사용하기에는 어려움이 따를 것이다.

그냥 전체적인 흐름과 장단점을 보기 위해 참고했고, 특히 도커에 관해 이해하고자 아래의 영상도 같이 보았다.

[드림코딩 채널 - 도커 한방에 정리](https://www.youtube.com/watch?v=LXJhA3VWXFA&t=3s)

만약 도커에 관한 개념이 헷갈리고 확실치 않다면 위의 유튜브 영상을 한 번 보고 오면 좋다.

<br>
<br>
<br>

***

도커에 관한 내용을 정리해보면 다음과 같다.

### **도커의 사용 이유**
1. 환경 일관성: Docker를 사용하면 개발 환경, 테스트 환경, 운영 환경 등 어디에서든 동일한 운영 환경을 만들 수 있다. 이로 인해 "내 컴퓨터에서는 잘 돌아가는데, 왜 서버에서는 안 돌아가지?"라는 문제를 해결할 수 있다.
2. 경량화: Docker는 가상화 기술을 사용하지만, 전통적인 가상 머신과는 다르게 운영체제를 공유하므로, 더 적은 자원을 사용하면서도 더 빠른 성능을 제공한다.
3. 모듈화와 확장성: Docker를 사용하면 애플리케이션과 그에 관련된 설정을 하나의 Docker 이미지로 만들어, 이를 기반으로 컨테이너를 생성한다. 이를 통해 애플리케이션 구성 요소를 모듈화하고, 필요에 따라 쉽게 확장할 수 있다.

### **도커의 장/단점**
- 장점
  1. 개발 편의성: 이미지 생성과 배포가 간단해서 개발과 배포 시간을 단축시킬 수 있다.
  2. 이식성: Docker 컨테이너는 어떤 시스템에서도 동일하게 동작할 수 있기 때문에, 이식성이 높다.
  3. 성능: Docker는 컨테이너 기반으로 가상화되기 때문에, 일반적인 가상 머신에 비해 성능 손실이 적다.
- 단점
  1. 윈도우에서의 호환성: Docker는 리눅스 커널을 기반으로 동작하기 때문에, 윈도우에서는 동일한 성능을 내기 어렵다.
  2. 보안: Docker 컨테이너는 호스트 시스템과 커널을 공유하기 때문에, 잘못된 설정이나 취약점이 있을 경우 보안 문제가 생길 수 있다.
  3. 복잡한 구성: Docker를 효과적으로 사용하려면 Dockerfile, Docker Compose 등 다양한 도구와 설정을 이해하고 활용해야 하므로, 학습 곡선이 높을 수 있다.

### **도커 이미지**
도커 이미지는 애플리케이션을 실행하는데 필요한 모든 것을 담고 있는 불변의 파일 시스템이다.   

이는 실행 가능한 소프트웨어 코드, 라이브러리, 환경 변수, 파일 등을 포함한다.   

이미지는 컨테이너를 생성하는 데 사용되며, 도커 이미지는 Dockerfile에서 정의하고 docker build 명령어로 생성할 수 있다.

### **도커 컨테이너**
도커 컨테이너는 도커 이미지의 인스턴스로, 실제 애플리케이션을 실행하는 데 사용된다. 

컨테이너는 이미지를 기반으로 생성되며, 독립적으로 실행되는 프로세스와 파일 시스템을 가지고 있다.

컨테이너는 docker run 명령어로 생성하고 시작할 수 있으며, 하나의 이미지로 부터 여러 개의 컨테이너를 생성할 수 있다.

### **이미지와 컨테이너**
도커 이미지는 실행 가능한 애플리케이션의 "레시피"라고 할 수 있고, 도커 컨테이너는 그 레시피를 바탕으로 생성된 "요리"라고 할 수 있다.

<br>

위 블로그들과 영상에도 충분히 설명하고 있는 내용들이다.

<br>
<br>
<br>

도커는 기본적으로 호스트 OS의 커널을 공유하여 독립적인 실행 환경(컨테이너)을 제공하는 플랫폼으로 기본적으로 리눅스 환경에서 가장 원활하게 작동한다.

물론 Mac이나 Widows 에서 실행이 가능하다.

당장 나의 경우도 Windows OS를 사용 중이기 때문에 Docker Desktop 애플리케이션을 설치하여 도커를 사용했다. (Mac Os의 경우도 동일하다.)

<br>

### **Docker Desktop**

이 Docker Desktop은 윈도우 위에 리눅스 VM을 생성하고 이 VM 위에서 도커를 실행하는 방식으로 동작한다.

즉, 이 Docker Desktop을 통해 윈도우나 맥에서도 Docker를 사용하여 독립적인 실행 환경을 구성할 수 있다.

이는, 개발자들 사이에서 소프트웨어의 일관성을 유지할 수 있게 한다.

단, 주의해야할 것은 Docker는 결국 리눅스 OS에서 동작하는 것이다.

다시 말해, 호스트 OS의 커널을 공유하여 리눅스 환경을 효율적으로 구성하고 그 위에 도커를 얹는 방식이다.

<br>

### **잠깐, 리눅스 환경을 구성한다면 결국 VM이 아닌가?**

비슷하다고는 할 수 있지만, 그렇지 않다.

이는 전통적인 VM과 도커가 사용하는 컨테이너 기술의 근본적인 차이 때문인데, 

VM의 경우 전체 운영체제의 리소스를 포함하여 독립적으로 구성된다.

다시 말해, 호스트 OS의 자원을 굉장히 많이 잡아먹는다는 의미이며, 각각의 VM이 독립적이기 때문에 VM 끼리 통신 시에 네트워크 통신을 해야한다.

반면, 도커 컨테이너의 경우 호스트 OS의 커널을 공유하여 경량화된 실행 환경을 제공한다.

이 경량화된 실행 환경이란, 도커의 컨테이너를 실행하기 위한 최소한의 환경으로만 리눅스 환경을 구성한다는 뜻이다.

이로 인해, 일반적인 VM보다 훨씬 적은 자원을 사용하게 되고, 더 빠르게 동작할 수 있다.

또한, 컨테이너 간에 데이터를 주고받는 것은 네트워크 통신이 필요하지 않다.

결론적으로, 도커를 Window나 Mac에서 활용할 때는 Docker Desktop으로 인해 VM과 비슷한 기술이 사용되지만,
이는 도커 컨테이너를 실행하기 위한 최소한의 조치일 뿐이다.

그러므로 도커 컨테이너 기술은 전통적인 VM과 근본적으로 다르다고 할 수 있다.

<br>

### **커널을 공유한다고?**

커널은 OS의 핵심 부분으로, 하드웨어와 소프트웨어 사이에서 중재자 역할을 한다.

커널은 시스템 자원과 하드웨어를 관리하며, 애플리케이션의 요청을 하드웨어가 이해할 수 있는 명령으로 변환하고, 그 반대의 역할도 한다.

예를 들어, 어떤 프로그램이 파일을 저장하려고 요청을 한다면, 커널은 하드 디스크에 데이터를 쓰는 데 필요한 하드웨어 명령을 실행하게 된다.

앞서 도커 구성 방식을 설명하다가 나온 "호스트 OS의 커널을 공유한다."는 말은 도커 컨테이너가 실행되는 호스트 운영 체제의 커널을 사용한다는 의미이다.

즉, 각 도커 컨테이너는 독립된 환경을 가지지만, 하드웨어에 대한 요청을 처리하는 커널은 호스트 OS의 것을 그대로 사용한다.

이는 각 컨테이너가 자체 OS를 가질 필요가 없으므로, 컨테이너를 더 가볍게 만들고, 생성과 삭제가 빠르며, 실행 시 필요한 자원도 적게 들게 한다.

이렇게 도커 컨테이너가 호스트 OS의 커널을 공유하는 것이 가능한 이유는, 도커가 리눅스의 컨테이너 기술을 기반으로 하기 때문이다.

리눅스 컨테이너는 리눅스 커널의 기능을 사용하여, 하나의 커널을 여러 독립적인 환경(컨테이너)로 분리하는 기술이다.

결론적으로, 도커 컨테이너는 호스트 OS의 핵심 부분만 사용하므로 추가적인 리소스가 필요하지 않다.

이를 통해 소프트웨어의 일관성을 유지하고, "내 환경에서는 잘 돌아가는데 다른 환경에서는 왜 안 돌아가지?"라는 문제를 해결할 수 있는 것이다.

<br>
<br>
<br>

***

이제 다시 배포로 돌아온다.

내가 진행한 서버 배포의 플로우는 다음과 같다.

![](/assets/img/projects/salog/Deploy_diagram.png)

<br>

흐름을 이해하는 데에는 무조건 그림이 최고다.

위 플로우를 순서대로 내가 진행한 내용을 작성할 것이고, 각각의 흐름에서 중요한 내용을 짚고 넘어갈 것이다.

<br>
<br>
<br>

### **IDE → Github Actions**

가장 먼저 Github Actions까지의 흐름이다.

매번 배포 자동화를 구성할 때마다 사용했던 Github Actions인데, 이번에도 역시 동일하다.

로컬 IDE에서 개발 후 깃헙으로 push를 하게 되면 github actions가 미리 정의해둔 스크립트 파일을 읽고 작성된 명령들을 실행한다.

물론 프로젝트의 메인 레포가 내 레포가 아니라 다른 팀원의 레포이기 때문에 main 브랜치로 merge가 되어야 배포가 시작된다.

정확히 말하면, 내 로컬 환경에서 feat 브랜치로 기능을 구현하고나서 dev 브랜치로 merge 후 내 깃헙 레포의 dev 브랜치로 push한다.

그 다음으로 프로젝트 메인 레포의 dev 브랜치로 pr을 보내고 리뷰가 작성되면 merge한다.

마지막으로 메인 레포의 dev 브랜치에서 main 브랜치로 merge가 되고 나면 그제서야 github actions가 동작하게 되는 것이다.

<br>

깃헙 액션이 실행되고 나서 수행할 명령을 담은 스크립트는 다음과 같다.

```yaml
name: Java CI with Gradle

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

permissions:
  contents: read

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      - name: Set up JDK 11
        uses: actions/setup-java@v3
        with:
          java-version: '11'
          distribution: 'temurin'

      # Github secrets로부터 데이터를 받아서, 워크 플로우에 파일을 생성
      - name: Make application.properties
        run: |
          cd ./server/src/main/resources
          touch application.properties
          echo "${{ secrets.PROPERTIES }}" > application.properties
        shell: bash

      - name: Build with Gradle
        run: |
          cd ./server/
          chmod +x ./gradlew
          ./gradlew clean build -x test
  ...
```

<br>

gradle.yml 파일인데 github action은 이 파일에 담긴 명령을 읽고서 실행한다.

뒤에 도커 관련 명령과 서버에 배포하는 명령도 있지만 부분적으로 보기 위해 일단 생략했다.

스크립트를 잘라서 보면 다음과 같다.

```yaml
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

permissions:
  contents: read
```

<br>

이 부분은 main 브랜치에 push가 되거나 pr 시에 해당 스크립트를 실행하겠다는 의미이다.

기본적으로 읽기 권한만 가지고 있다.

```yaml
jobs:
  build:

    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      - name: Set up JDK 11
        uses: actions/setup-java@v3
        with:
          java-version: '11'
          distribution: 'temurin'
```

<br>

다음은 실행 명령이다.

github actions 러너가 우분투 환경에서 실행될 수 있도록 지정해 주었고, steps 이후부터 실행할 작업을 의미한다.

"actions/checkout@v3" 이 부분은 깃헙 액션의 공식 액션으로 워크플로우가 실행되는 러너에 메인 레포의 코드가 복사된다.

그 다음으로는 "actions/setup-java@v3" 이 공식 셋업 액션을 사용하여 temurin 배포판 JDK 11버전을 설치하도록 했다.

**(참고로 runner란 github actions 사용 시 GitHub에서 제공하는 인스턴스 OS이다.)**

```yaml
      - name: Make application.properties
        run: |
          cd ./server/src/main/resources
          touch application.properties
          echo "${{ secrets.PROPERTIES }}" > application.properties
        shell: bash

      - name: Build with Gradle
        run: |
          cd ./server/
          chmod +x ./gradlew
          ./gradlew clean build -x test
```

<br>

이제 마지막 부분이다.

여기서 make application.properties인 이름에서 알 수 있듯이 github secret으로 저장된 application.properties 내용들을 "./server/src/main/resources"
위치에 파일을 생성해 복사하는 것이다.

이를 통해 설정 파일을 안전하게 사용할 수 있다.

(지금은 변경되었지만, 이전 프로젝트에서는 이 방법을 생각 못해 공용 비밀번호가 노출되는 등의 문제가 있었다...)

그다음 Build with Gradle 명령을 실행하는데, 마찬가지로 이름에서 알 수 있듯, gradle을 빌드하여 우리의 웹 애플리케이션 서버를 jar 파일로 말아주는 명령을 수행한다.

<br>

여기까지 문제 없이 진행되었다면 github actions 러너 안에 jdk11 버전이 설치되어 있을 것이고, 웹 서버 jar 파일이 말려서 들어 있을 것이다.

이제 그 다음인 도커 플로우로 들어간다.

<br>
<br>
<br>

### **Github Actions → Docker**

여기서 부터는 gradle.yml 파일의 다음 부분을 봐야한다.

```yaml
 ...
      - name: Log in to Docker Hub
        run: docker login -u ${{ secrets.DOCKER_USERNAME }} -p ${{ secrets.DOCKER_PASSWORD }}

      - name: Build Docker image
        run: docker build -f ./server/Dockerfile -t ${{ secrets.DOCKER_REPO }}/salog .

      - name: Push Docker image to Docker Hub
        run: docker push ${{ secrets.DOCKER_REPO }}/salog

      - name: Deploy to server
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.HOST }}
          username: ec2-user
          key: ${{ secrets.KEY }}
          script: |
            docker rm -f $(docker ps -aq)
            docker pull ${{ secrets.DOCKER_REPO }}/salog
            docker run -d -p 80:80 ${{ secrets.DOCKER_REPO }}/salog
            docker image prune -f
```

<br>

이 파일 내용은 위의 "IDE → Github Actions" 내용에서부터 이어진다.

다시 순서대로 보면,

```yaml
      - name: Log in to Docker Hub
        run: docker login -u ${{ secrets.DOCKER_USERNAME }} -p ${{ secrets.DOCKER_PASSWORD }}
```

<br>

도커에 로그인한다.

이 로그인 ID/PW는 당연히 github secret에 저장되어 있다.

해당 내용을 읽어 github actions 러너에서 도커로 접속하는 것이다.

```yaml
      - name: Build Docker image
        run: docker build -f ./server/Dockerfile -t ${{ secrets.DOCKER_REPO }}/salog .
```

<br>

다음으로는 도커 이미지를 생성한다.

이 이미지 생성은 dockerfile을 참조하기 때문에 지정된 위치의 Dockerfile을 찾아서 읽고 해당 내용에 맞춰 도커 이미지를 생성하게된다.

이 dockerfile은 스크립트에 대한 설명 이후에 작성할 것이다.

```yaml
      - name: Push Docker image to Docker Hub
        run: docker push ${{ secrets.DOCKER_REPO }}/salog
```

<br>

이미지를 생성했으니 EC2 인스턴스와 이미지를 공유하기 위해 docker hub에 push해야한다.

이 docker hub는 간단하게 말해 github과 같은 역할을 한다. 한 마디로 저장소이다.

```yaml
      - name: Deploy to server
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.HOST }}
          username: ec2-user
          key: ${{ secrets.KEY }}
          script: |
            docker rm -f $(docker ps -aq)
            docker pull ${{ secrets.DOCKER_REPO }}/salog
            docker run -d -p 80:80 ${{ secrets.DOCKER_REPO }}/salog
            docker image prune -f
```

<br>

마지막으로 ec2 인스턴스에서 실행할 명령이다.

우선 미리 생성된 ec2 인스턴스로 접속한다.

script 이후 순서대로 명령어의 의미는 아래와 같다.
1. 모든 실행 중인 도커 컨테이너를 강제로 종료하고 삭제한다.
1. docker hub에 push된 이미지를 pull한다.
2. 해당 이미지를 백그라운드(-d)에서 80포트로 실행한다. (컨테이너의 80포트가 호스트(ec2)의 80포트에 바인딩된다.(-p))
3. 사용하지 않는 도커 이미지를 강제로 삭제한다.

<br>

이 과정을 거쳐 자동배포 환경을 구성했다.

<br>
<br>
<br>

끝내기 전에 도커 이미지 생성을 위한 dockerfile을 살펴봐야한다.

```dockerfile
# Build stage
FROM gradle:7.2.0-jdk11 AS build
COPY --chown=gradle:gradle . /home/gradle/
WORKDIR /home/gradle/server
RUN gradle build --no-daemon --no-watch-fs


# Package stage
FROM openjdk:11-jre-slim
COPY --from=build /home/gradle/server/build/libs/salog-0.0.1-SNAPSHOT.jar /app/salog.jar
EXPOSE 80
ENTRYPOINT ["java","-jar","/app/salog.jar"]
```

<br>

도커 이미지를 생성하기 위해서 dockerfile이 정의되어 있어야한다.

dockerfile의 내용은 다음과 같다.

```dockerfile
# Build stage
FROM gradle:7.2.0-jdk11 AS build
COPY --chown=gradle:gradle . /home/gradle/
WORKDIR /home/gradle/server
RUN gradle build --no-daemon --no-watch-fs
```

<br>

1. 도커 이미지 빌드의 기반은 "gradle:7.2.0-jdk11"이다.
2. 현재 디렉토리의 모든 파일과 폴더(.)를 Docker 이미지의 /home/gradle/ 디렉토리로 복사한다.
   - "--chown=gradle:gradle" 옵션은 복사된 파일들의 소유자와 그룹을 gradle로 설정한다.
3. 도커 이미지 내에서 작업 디렉토리를 "/home/gradle/server"로 설정한다. 이후에 실행되는 RUN, CMD, ENTRYPOINT, COPY 및 ADD 명령어는 이 디렉토리를 기반으로 실행된다.
4. 도커 이미지를 빌드하는 동안 "gradle build --no-daemon --no-watch-fs" 명령을 실행한다. 이 명령은 Gradle 데몬을 사용하지 않고, 파일 시스템 감시를 비활성화하면서 Gradle 빌드를 수행함을 의미한다.
   - 이 파일 감시 기능은 성능 최적화와 관련이 있다. 빌드 중에 파일 변경을 감지하고 변경이 감지되면 자동으로 재빌드를 수행하는 기능인데, 개발 중이 아닌 CI/CD 환경에서는 불필요하다. CI/CD 파이프라인에서는 소스 코드가 변경될 일이 없기 때문이다.

```dockerfile
# Package stage
FROM openjdk:11-jre-slim
COPY --from=build /home/gradle/server/build/libs/salog-0.0.1-SNAPSHOT.jar /app/salog.jar
EXPOSE 80
ENTRYPOINT ["java","-jar","/app/salog.jar"]
```

<br>

1. "openjdk:11-jre-slim " 기반 이미지를 사용한다. JRE가 포한되어 있어 java 앱을 실행하는데 필요한 것이 포함되어있다.
2. 빌드 후 생성된 jar 파일을 새 이미지에 복사한다. "/home/gradle/server/build/libs/salog-0.0.1-SNAPSHOT.jar" 파일을 새이미지의 "/app/salog.jar" 위치에 복사한다.
3. Docker 컨테이너의 80번 포트를 외부에 노출한다. 이를 통해 컨테이너 외부에서 이 포트로 접속할 수 있게 된다.
4. 도커 컨테이너가 시작될 때 실행할 명령을 설정한다. "java -jar /app/salog.jar" 명령을 실행한다.

<br>

이 dockerfile을 통해 도커 이미지를 생성하고 실행시키는 작업을 한다.

<br>
<br>
<br>


***

항상 스크립트를 구성해서 배포를 할 때 문제가 되는 것은 각 파일의 위치를 지정하는데에 있다.

처음 이 과정을 진행했을 때는 "러너"라는 것의 존재를 몰라 이 명령들이 어디서 실행되는지 때문에 헤맸었다.

그 이후에는 러너 내부로 접속해 볼 수 없기 때문에 파일들의 위치를 특정하는 것이 어려웠다.

파일 위치에 관련해서는 항상 .github 폴더가 있는 곳이 최상위 디렉토리라고 기억하고 있으면 된다.

<br>

이 외에 도커에 대한 개념과 사용법을 잘 몰라서 로컬 docker desktop에서 실습을 한번 해보았고, 덕분에 어느정도 파악을 하고 진행했다.

어렵다? 실습이다.

***

<br>

이 다음으로는 AWS 설정으로 들어가야하는데, 생각보다 글이 굉장히 길어져 AWS 설정과 로드밸런서, 무료 도메인에 관한 설명은 다음 포스트에서 진행하겠다.
