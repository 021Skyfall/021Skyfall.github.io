---
title: "프로젝트:샐로그 / 이슈 1 - 배포 단계"
author: Jeremiah Lee
date: 2023-12-18
categories: [ 프로젝트, 샐로그, 문제해결 ]
tags: [프로젝트, 샐로그, 문제해결, 배포 ]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Project_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

### *1. 문제 발생*

자동 배포 구성은 프로젝트 산출물을 전부 작성하고 나서 본격적으로 구현에 들어가기 앞서 전부 끝내 놓았다.

해당 내용은 카테고리의 이전 내용을 보면 찾을 수 있다.

그 이후 나는 회원 쪽을 담당해서 구현을 하기 시작했고, 최근에 요구사항대로 구현이 끝나 개발 브랜치에서 배포 브랜치로 pr해 merge했고,
배포가 시작되었다.

그런데 프론트 팀원이 해당 배포된 서버를 바탕으로 로컬에서 테스트 중...   
"로그인이 안되요."   
라는 말에 덜컹했다.

그래서 가장 먼저 시도한 것이 EC2로 접근하여 서버 로그를 먼저 살펴본 것이다.

이미 기존에 구현할 때 시큐리티 디버깅을 켜놓았기 때문에 로그가 출력되는데,
들어가보니 jwt 토큰이 생성 되기 전에 jwt 시크릿 키가 0bit로 너무 짧게 설정되어 있어
해당 시크릿 키를 바탕으로 토큰을 생성하기에는 보안이 취약하다는 에러 로그가 출력되어 있었다.

이상했다.

현재 액션 스크립트인 gradle.yml에서, 우리는 application.yml 파일을 따로 프로젝트에 작성하지 않고 깃헙 secrets에 해당 파일 내용을
적어 넣은 다음 이 application.yml 파일을 그대로 ~/src/main/resources 위치에 생성하여 내용을 복사하게끔 작성해놓았기 때문에 무언가 이상했다.

```yml
          cd ./server/src/main/resources
          touch application.yml
          echo "${{ secrets.YML }}" > application.yml
        shell: bash
```

스크립트는 위와 같은 내용으로 작성되어 있었다.

위와 같은 명령을 통해 필요한 값들이 들어있는 설정 파일 내용이 고스란히 복제되어 jwt 시크릿 키 등 잘 불러와야하는데 그렇지 못 한 것 같았다.

이때 application.yml 파일 안에는 값들을 직접 넣었기 때문에 jwt 시크릿 키 값이 0bit일리가 없다.

의아한 점이 많았다.

yml 파일 내용이 정상적으로 복제되지 않았다면 설정 파일의 부재로 인해 서버 자체가 실행되지 않았을 것이다.

더군다나, 아래의 코드를 활용해 서버 실행 시 설정 값이 제대로 있는지까지 확인했다.

```java
		// JWT_SECRET_KEY 환경 변수 읽기
		String jwtSecretKey = System.getenv("JWT_SECRET_KEY");
		System.out.println("JWT_SECRET_KEY: " + jwtSecretKey);

		// EMAIL 환경 변수 읽기
		String email = System.getenv("EMAIL");
		System.out.println("EMAIL: " + email);

		// EMAIL_PASSWORD 환경 변수 읽기
		String emailPassword = System.getenv("EMAIL_PASSWORD");
		System.out.println("EMAIL_PASSWORD: " + emailPassword);
```

그런데도 안되었기 때문에 다른 방법으로 접근해 보았다.

<br>

### *2. application.yml에 필요한 값들*

우선은 서버가 실행이 되었기 때문에 yml 파일의 내용이 복제되어 들어가 있다고 가정했다.

그런데 jwt나 email sender와 같은 설정 값이 필요한 기능들이 여전히 동작하지 않았기 때문에
깃헙 secrets에 해당 내용을 입력하고(예를 들어 jwt 시크릿 키), yml 파일의 해당 위치에 그 값을 넣어주도록 설정했다.

```yml
          cd ./server/src/main/resources
          touch application.yml
          echo "${{ secrets.YML }}" > application.yml
          echo "JWT_SECRET_KEY=${{ secrets.JWT_SECRET_KEY }}" >> application.yml
          echo "email=${{secrets.email}}" >> application.yml
          echo "password=${{secrets.password}}" >> application.yml
        shell: bash
```

이 명령이 실행되면 각각 yml 파일에 있는 JWT_SECRET_KEY 등이 깃헙 secrets에 지정된 내용으로 변경된다.

이렇게 넣고 깃헙 액션을 실행시키자, 이번에는 다른 문제가 발생했다.

![](/assets/img/projects/salog/issue1_contexttestfail.png)

도커 이미지를 빌드하는 중에 contextLoads가 실패하여 깃헙 액션이 중단 되었다.

Spring Application Context는 스프링 앱의 핵심으로, 애플리케이션의 설정과 빈들을 관리한다.

이러한 context가 제대로 로드되지 않으면 애플리케이션 자체가 실행되지 않거나 지금 볼 수 있듯이 빌드가 불가능하다.

내용을 미루어보아 작성한 변수 값을 제대로 읽어오지 못해 빌드 과정 중에 해당 값들이 필요한 기능이 실행되지 않아서 라고 생각할 수 있다.

그런데 우리는 yml 파일에 해당 값들을 직접 써놓았고, 프로젝트 resources에는 이 설정 값들이 직접 쓰여있는 yml 파일이 들어있는 것과 다름없기에
실패하는 이유를 알지 못했다.


위 내용이 실패한 이유로는 
1. Secrets 값이 잘못 되어 있음 → 이름과 값 모두 확인하여 다시 작성해보았음
2. application.yml 파일의 위치 → 이 역시도 정확히 생성됨
3. 테스트 환경에서 필요한 환경 변수 설정 안됨 → 이 테스트 시점이 위의 yml 파일 생성 이후 시점이기 때문에 아닐거라 생각했지만 이 방향에 초점을 두고 넘어감

그래서 스크립트에 작성된 위 내용이 잘못되었다고 판단하고 비밀 키 값들을 환경 변수로 작성하여 활용하는 방향으로 틀었다.

<br>

### *2-1. 사실*

사실 위의 내용 중 명령어가 잘못 되어 있었다.

애당초 application.yml 파일 내부에

```yml
JWT_SECRET_KEY=${JWT_SECRET_KEY}
```

이런 식으로 환경변수를 활용하게 끔 작성되어 있었고, 
이렇게 되어 있다면 위 리눅스 명령어가 실행되어도 이 설정은 그대로 남아 환경 변수를 읽어오는 것을 기다리게 된다.

즉 gradle.yml에 적은 명령줄대로면 application.yml 과 상관없이 JWT_SECRET_KEY=깃헙 secrets에 있는 값 줄이 생성될 것이다.

그래서 정상 작동하는 스크립트라고 볼 수 없다.

어찌되었던 우리는 환경 변수 문제라고 생각했기 때문에 다음 해결법으로 넘어갔다.

<br>

### *3. 환경변수로*

환경 변수를 통해 필요한 값들을 읽어들이는 작업을 진행했다.

이 작업은 도커 이미지를 실행할 때 환경 변수를 주입해주는 작업이다.

```yml
          script: |
            docker rm -f $(docker ps -aq)
            docker pull ${{ secrets.DOCKER_REPO }}/salog
            docker run -e JWT_SECRET_KEY=${{ secrets.JWT_SECRET_KEY }} -e EMAIL=${{ secrets.EMAIL }} -e EMAIL_PASSWORD=${{ secrets.EMAIL_PASSWORD }} -d -p 80:80 ${{ secrets.DOCKER_REPO }}/salog
            docker image prune -f
```

위와 같이 실행 시 환경 변수를 깃헙 secrets 에서 불러들여 주입해준다.

yml에는 설정 값이 들어있는 그대로 두고 이미지 실행 시에 또 주입해주는 방식이다.

이 경우 깃헙 액션이 실패하지 않고 정상적으로 배포는 된다.

다만, 가장 처음과 마찬가지로 jwt 시크릿 키 값이 0bits 인 약한 키 예외가 또 발생했다.

이메일 전송도 설정 정보가 맞지 않아 제대로 전송이 안되었다.

사실 이 때까지만 해도 이유를 몰라 포기하고 싶었다.

서버 실행 시 위의 로그를 출력하게 끔 했을 때, 정상적으로 각 값들이 출력되었기 때문에 도대체 왜 설정 정보를 읽지 못하는 예외가 발생하는지 몰랐다.

<br>

### *4. yml 삭제*

3번의 과정도 안 통했으니 이번에는 yml 내의 중요 값을 환경 변수로 전부 활용할 수 있도록 
yml 파일을 삭제하고 직접 넣은 다음 각 값을 환경 변수로 불러올 수 있게끔 설정했다.

물론 깃헙 액션 스크립트 내용 중 application.yml을 생성하여 내용을 깃헙 시크릿에서 복사하는 명령을 삭제했다.

이미지 실행 시 환경 변수를 불러들이는 명령도 그대로 두었는데, 또 contextLoads 예외가 발생하여 깃헙 액션이 실행되지 않았다.

앞서 얘기한 context를 로드하는 과정에 대한 내용으로 힌트를 얻어 이번에는 이미지 생성, 그래들 빌드 시점에도 환경 변수를 주입해보자는 생각으로
도커파일 내용 중 빌드에 관련된 부분을 삭제하고, 깃헙 액션 스크립트의 그래들 빌드 명령줄에도 환경 변수를 주입해보기로 했다.

이번이 마지막이었다.

이것도 안 되면 아예 개발자 커뮤니티에 질문을 올리든, 알고 있는 사람 아무나 붙잡고 물어보든 할 예정이었다.

```yml
      - name: Build with Gradle
        run: |
          cd ./server/
          chmod +x ./gradlew
          ./gradlew clean build -x test -Dspring.profiles.active=default -DJWT_SECRET_KEY=${{ secrets.JWT_SECRET_KEY }} -DEMAIL=${{ secrets.EMAIL }} -DEMAIL_PASSWORD=${{ secrets.EMAIL_PASSWORD }}

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

그리하여 우리의 깃헙 액션 스크립트는 위와 같은 형태가 되었고, 빌드 시점에는 환경 변수를 주입하고 실행 시점에는 주입 하지 않는 방식으로 가보았다.

결과만 말하면,

이 경우 깃헙 액션 실행, 이미지 빌드 과정 중 contextLoads 실패는 발생하지 않지만 배포가 끝나고 ec2에서 확인해보니 환경 변수를 읽어오지 못 해 실행 되지 않았다.

그런데 이 경우에는 실행 시점에 환경 변수를 주입해주면 된다고 생각해 다음으로 넘어갔다.

<br>

### *5. 해결*

그래서 결국 깃헙 액션 스크립트는 다음과 같이 최종적으로 변경했다.

```yml
      - name: Build with Gradle
        run: |
          cd ./server/
          chmod +x ./gradlew
          ./gradlew clean build -x test -Dspring.profiles.active=default -DJWT_SECRET_KEY=${{ secrets.JWT_SECRET_KEY }} -DEMAIL=${{ secrets.EMAIL }} -DEMAIL_PASSWORD=${{ secrets.EMAIL_PASSWORD }}

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
            docker run -e JWT_SECRET_KEY=${{ secrets.JWT_SECRET_KEY }} -e EMAIL=${{ secrets.EMAIL }} -e EMAIL_PASSWORD=${{ secrets.EMAIL_PASSWORD }} -d -p 80:80 ${{ secrets.DOCKER_REPO }}/salog
            docker image prune -f
```

빌드 시점과 실행 시점에 둘 다 환경 변수를 주입해주는 방법이다.

여기까지 진행하면서, application.yml 내용 주입 명령을 삭제하고 직접 생성했으며, dockerfile의 빌드 명령을 삭제했다.

마지막으로 위 스크립트를 바탕으로 배포하고, 서버를 실행했을 때 문제없이 모든 기능이 정상 작동 하였다.

ec2 내부에서 서버의 로그를 봐도 정상적으로 동작하고 있었다.

결국 처음 배포 로직과는 많이 달라졌지만 해결했다는 게 중요하다.

<br>

### *6. 이유*

정리하면, application.yml 내용 주입 명령을 삭제하고 직접 생성했으며, dockerfile의 빌드 명령을 삭제했으며
빌드 시점과 실행 시점에 둘 다 환경 변수를 주입해주는 방법으로 진행했고 성공했다.

이때 까지 생각한 이유는 다음과 같다.

```
그래들 빌드 시점에 환경 변수가 주입되어야, 정상적으로 빌드되어 이 환경 변수를 활용하는 기능들이 동작 할 수가 있을 것

그런데, 그래들 빌드 시점에 환경 변수를 주입하지 않아서 해당 기능들이 환경변수가 없이 빌드됨

그렇기 때문에 이미지 실행 시 환경 변수를 읽어오고, 실행시켜도 서버에서는 이미 환경변수 없이
빌드가 되어버려서 정상 작동 불가했을 것

그런데 어떻게 환경변수 없이 서버가 가동할 수 있었는가?
-> 도커 기본 환경변수가 입력되었을 것 -> 그렇기 때문에 jwt 시크릿키 0bits 였던 것
```

그런데 여기서 좀 의아한 점이 yml 파일 안에 각 값들이 들어있는 형태였기 때문에 빌드할 때 마찬가지로 해당 값들을 활용했을 것이고 그렇다면 정상적으로 동작했어야 한다고 생각한다.

그런데 내 생각과는 달리 정상적으로 동작하지 않았기 때문에 미궁으로 빠졌다.

몇 가지 의문점이 나왔다.
1. 그래들 빌드 이전에 환경 변수를 주입하고서 이미지 빌드 안되는 이유
   - 그래들 빌드 이전에 yml 파일 생성 시 환경 변수를 직접 덮어써도 안됐던 이유 -> 깃헙 액션 문제
2. 도커 이미지 실행 명령줄에 환경 변수를 주입시켰는데 환경 변수를 활용하지 못하는 이유
   - 서버에서 환경 변수를 출력하면 읽어옴, jwt나 email 등 기능이 못 읽음

이 의문을 바탕으로 구글링 + 코파일럿에게 질문 등을 거친 결과

```
환경 변수의 사용 시점과 설정 시점의 차이 때문
처음 스크립트에서는 환경 변수가 도커 컨테이너를 실행할 때 설정되었다. 
그러나 도커 이미지는 이미 빌드가 완료된 상태였기 때문에, 빌드 시점에서 필요한 환경 변수를 참조할 수 없었던 것
즉, 애플리케이션의 빌드 과정에서 환경변수가 필요한 경우, 환경변수는 빌드 시점에 설정되어야함
환경 변수는 그것이 필요한 시점에 이미 준비되어 있어야 함
-> 왜 암것도 없는데 실행됨?
기본 환경변수가 있어서 실행 시점에서 다른 환경 변수를 읽어들이는 특성이 있을 수 있음
```

위와 같은 결론에 도달했다.

사실 이유를 정확히 알지는 못한다.

누군가가 지나다가 이 글을 보고 제발 정답 메일을 보내주었으면 한다.

이런 경우는 개인마다 상황, 로직등이 다 달라 찾기가 너무 힘들다.

속 시원하게 풀리진 않았지만, 한 가지 알게된 것은

빌드 시점과 실행 시점의 환경변수 주입은 다르다는 것이다.

내 생각에는 도커를 활용하여 배포를 진행했기 때문에 이 주입 시점이 다른 것 같다.

도커가 껴있지 않다면 그냥 그래들 빌드와 동시에 서버가 실행될 것이고 이 때 환경변수를 주입해주면 되었으니까.

<br>
<br>
<br>

***

시원하게 풀리지 않아서 속이 아프다.

아마 도커를 완벽히 이해하지 못하고 사용했기 때문이라고 생각한다.

빌드와 배포가 분리되어 있는 이런 상황에서는 환경 변수를 서버 빌드, 실행 시점에 둘 다 환경 변수를 주입해주어야한다는 결과는 꼭 기억해야한다.
