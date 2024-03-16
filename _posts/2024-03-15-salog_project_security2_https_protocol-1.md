---
title: "프로젝트:샐로그 / 보안 2 - Https 프로토콜 적용과 문제해결"
author: Jeremiah Lee
date: 2024-03-15
categories: [ 프로젝트, 샐로그, 보안, 문제해결 ]
tags: [프로젝트, 샐로그, 보안, 통신, 문제해결 ]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Project_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

### ***시작하기 앞서***

저번 프로젝트에서는 로드밸런서를 활용했기 때문에 Https 프로토콜을 손쉽게 사용할 수 있었다.

그러나 올해 2월부터 AWS의 정책이 변경됨에 따라 로드밸런서에 자동으로 할당되는 퍼블릭 ipv4에 대한 요금이 청구되기 시작하여 로드밸런서를 활용하지 못했고,
이에 따라 서버의 통신 프로토콜을 https로 유지할 수 없었다.

그럼에도 보안이 강화된 통신 프로토콜인 https가 필요했고, 이를 어떻게 처리할 것인지 생각해보았다.

결국 http에서 https로 통신 프로토콜을 변경하는 작업은 인증서만 있으면 처리 가능하다.

그래서 기존에 발급했던 ACM을 활용하여 수동으로 웹 애플리케이션 백엔드 서버의 종단간 프로토콜을 https로 변경하려고 했는데,
ACM 자체가 AWS 서비스를 거쳐서만 사용 가능하기 때문에 ACM도 삭제했다.

즉, 직접적으로 EC2 인스턴스 내부에서 ACM 인증서를 다운로드 할 수 있는 방법은 없었다.

그 다음 방법을 찾다가 Let's Encrypt 라는 무료 SSL 인증서 발급 기관에서 인증서를 발급 받아 프로토콜을 변경할 수 있다는 것을 발견했다.

<br>

이번 포스트는 무료 SSL 인증서를 발급 받아 수동으로 종단간 https 프로토콜로 통신 할 수 있도록 구성한 방법과
해당 작업 중에 발생한 문제에 대해 기술할 것이다.

<br>
<br>
<br>

### ***EC2 인스턴스 내에서 SSL 인증서 발급***

가장 먼저 시도한 것이 SSL 인증서 발급이었다.

이 인증서가 없으면 https 프로토콜로의 변경은 애초에 이루어낼 수 없었다.

인증서 발급 기관에도 여러가지가 있지만 Let's Encrypt 를 선택했다.

이유는 두 가지다.

1. 무료 : Let's Encrypt는 무료로 SSL/TLS 인증서를 제공한다.
2. 자동화 : Let's Encrypt와 함께 사용되는 Certbot 같은 도구는 인증서의 발급, 설치, 갱신 과정을 자동화시킬 수 있게 한다.

이 두 가지 이유가 있기 때문에 Let's Encrypt를 선택했다.

<br>

이제 인증서를 발급 받고 EC2 인스턴스 내부에 다운로드 하는 과정을 살펴보자.

시간 순서대로 작성할 것이다.

##### **1. 도커 컨테이너 중지**

우선 EC2 내부에서 실행 중이던 도커 컨테이너가 80번 포트를 사용하고 있기 때문에 컨테이너를 중지 시켰다.

Let's Encrypt에서 인증서를 발급 받기 위해서는 80번 포트가 필요하기 때문이다.

<br>

##### **2. EC2에서 인증서 발급**

그 다음으로는 EC2 인스턴스 내부에 인증서를 발급 받아 저장해야 했다.

앞서 말했듯이 ACM의 경우 아마존 서비스를 통해서 구성된 종단간 통신에서만 사용이 가능했고, 내가 하려는 방식대로 수동으로 HTTPS 프로토콜을 사용하기 위해서는
직접 웹 애플리케이션에 인증서를 적용해야한다.

그렇기 때문에 EC2에 인증서를 발급 후 저장하고, 이 인증서를 애플리케이션에 적용시키려 했다.

우선 Let's Encrypt로 인증서를 발급 받기 위해 Certbot 이라는 도구를 설치해야했다.

```terminal
$ sudo yum install -y certbot
```

EC2의 버전은 Amazon Linux 2023이기 때문에 apt-get으로 Certbot을 설치하지 않고, yum 패키지를 사용하여 설치했다.

이후 nginx와 같은 별도의 웹 서버를 EC2 내부에서 실행 중인 것이 아닌 웹 애플리케이션 기본 내장 서버인 tomcat이 직접 클라이언트의 요청을 처리하고 있었기 때문에

```terminal
$ sudo certbot certonly --webroot -w /usr/share/nginx/html -d server.salog.kro.kr
```

와 같이 별도로 웹 서버를 설치 경로를 지정해서 발급 받는 것이 아니라

```terminal
sudo certbot certonly --standalone -d server.salog.kro.kr
```

--standalone 옵션을 사용하여 내장된 임시 웹 서버를 사용하여 인증서를 발급 받았다.

이 옵션은 별도로 운영 중인 웹 서버가 없거나, 기존 웹 서버의 설정을 변경하고 싶지 않을 때 사용하는데, 현재 별도 운영 중인 웹 서버가 따로 존재하는
것이 아니라 스프링 부트 내장 웹 서버인 tomcat을 사용하고 있기 때문에 이 방식으로 "server.salog.kro.kr"라는 도메인에 대한 인증서를 발급 받은 것이다.

이 경우, certbot이 일시적으로 80번 혹은 443번 포트를 사용하여 내장 웹 서버를 실행 시켜 인증서를 받급 받기 떄문에 이 포트들이 다른 서비스에 의해 사용되고 있지 않아야 한다.

그래서 이전 번에 80번 포트로 가동 중이던 컨테이너를 종료시킨 것이다.

또한, 기존에는 서버 도메인을 지정하지 않고 퍼블릭 DNS 주소를 주로 활용했었는데, 이번 기회에 서버 도메인을 지정했다.

이는, 추후 EC2를 모종의 이유로 재시작하게 되면 퍼블릭 DNS 주소가 변경됨에 따라 이 복잡한 인증서 발급 절차를 다시하기 보다는 도메인 서비스 사이트로 가서
CNAME에 연결된 주소만 바꿔주면 되기 때문이었다.

다시 돌아와, 이 명령어가 실행되면 인증서 발급이 시작되고, 그 전에

```terminal
Saving debug log to /var/log/letsencrypt/letsencrypt.log Enter email address (used for urgent renewal and security notices)
```

와 같은 알림이 발생한다. 

이 알림은 인증서를 갱신해야하는 때를 알리고, 보안 공지 등 Let's Encrypt의 안내를 받을 email 주소가 필요하다는 의미이다.

이후 쭉쭉 실행되어

```terminal
$ sudo certbot certonly --standalone -d server.salog.kro.kr
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Requesting a certificate for server.salog.kro.kr

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/server.salog.kro.kr/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/server.salog.kro.kr/privkey.pem
This certificate expires on 2024-05-30.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

```

이렇게 출력되면 인증서가 성공적으로 발급 된 것이다.

인증서에 대한 secret 키와, 만료일자가 적혀있다.

이제, 인증서가 발급 되었으니 이 인증서를 스프링 부트 설정에 적용해야 한다.

<br>

##### **3. 스프링 부트에 인증서 적용**

스프링 부트 애플리케이션에서 SSL 인증서를 적용하기 위해 발급 받은 PEM 형식의 인증서를 Java KeyStore 형식으로 변환해야한다.

변환하는 이유는 다음과 같다.
1. Java 호환성
   - Java 기반 애플리케이션은 인증서를 관리하기 위해 Java KeyStore 또는 PKCS12 형식을 사용한다.
   - Java 표준 보안 기능과 통합되어 있어, 인증서와 관련된 작업을 효율적으로 수행할 수 있도록 한다.
2. 보안 강화
   - KeyStore는 비밀번호로 보호되어 있으며, 인증서와 개인 키를 안전하게 저장할 수 있는 저장소 역할을 한다.
3. 용이성
   - 스프링 부트는 yml 또는 properties 파일을 통해 설정을 관리하는데, 인증서를 KeyStore 형식으로 변환하고 적용하게 되면 쉽게 관리할 수 있다.
4. 통합성
   - 많은 Java 애플리케이션 및 라이브러리들이 KeyStore를 통한 인증서 관리를 지원한다.

KeyStore 형식으로 변환하고자 openssl 명령어를 사용하였다.

```terminal
$ sudo openssl pkcs12 -export -in /etc/letsencrypt/live/server.salog.kro.kr/fullchain.pem -inkey /etc/letsencrypt/live/server.salog.kro.kr/privkey.pem -out keystore.p12 -name tomcat -CAfile /etc/letsencrypt/live/server.salog.kro.kr/chain.pem -caname root
```

와 같이 각 인증서의 절대경로를 지정하여 PEM 키를 바탕으로 keyStore 형식으로 변환시켰다.

여기서 주의해야할 점은 openssl 명령어가 pem 파일에 접근할 수 있는 권한이 있어야하기 때문에 sudo로 관리자 권한을 주어야한다.

여기까지 완료했다면

```terminal
Enter Export Password:
```

와 같이 PKCS12 KeyStore 파일에 설정할 비밀번호를 입력해야 한다.

이 비밀번호는 추후 애플리케이션 설정에서도 사용되기 때문에 중요한 부분이다.

또한, 여기서 생성된 keystore.p12 파일이 ec2 인스턴스의 /home/ec2-user 디렉토리에 저장되어 있기 때문에 추후 작업을 진행할 때 이를 염두에 두어야한다.

여기까지 완료되었다면 ssl 인증서를 java keyStore 형식으로 변환이 되었고, 이 정보를 애플리케이션 설정에 추가하여 연결시켜야 한다.

```yml
server:
  ...
  ssl:
    key-store: file:/app/keystore.p12
    key-store-type: PKCS12
    key-store-password: ${KEY_STORE_PASSWORD}
    key-alias: tomcat
    key-password: ${KEY_STORE_PASSWORD}
  ...
```

위와 같이 설정을 추가해주면 된다.

여기서 key-store 프로퍼티는 파일의 절대 위치를 파악하고 있어야 작성할 수 있는데, 기존 위치와 다른 점은 이후 다시 설명하겠다.

또한, 비밀번호의 경우는 중요한 보안 관련 내용이기 때문에 github 시크릿에 저장되어 있다.

<br>

여기까지 진행 한 다음 각 시크릿을 환경변수로 주입하기 위해 Github Actions 스크립트에 작성해주고 나면 작업이 종료된다.

재배포 후에는 https 프로토콜로 종단간 통신이 가능해지기 때문에 기존 80포트로 클라이언트와 통신하던 도커 컨테이너도 443 포트로 변경했다.

```yml
docker run -e ... -d -p 443:80 ${{ secrets.DOCKER_REPO }}/salog
```

이와 같이 스크립트에서 실행 포트를 변경하여, 클라이언트는 443 포트를 통해 ec2 인스턴스로 접속하고,
이 트래픽은 Docker 컨테이너의 80포트로 전달된다.

이를 통해 443 포트를 제공하게 된다.

<br>

##### **4. Http -> Https**

또한 http 프로토콜로 들어온 요청 역시 https로 리다이렉트하기 위해

```java
// HTTP -> HTTPS 리다이렉트
@Configuration
public class SecurityConfig extends SecurityConfigurerAdapter<DefaultSecurityFilterChain, HttpSecurity> {

    @Override
    public void configure(HttpSecurity http) throws Exception {
        http
                .requiresChannel()
                .requestMatchers(r -> r.getHeader("X-Forwarded-Proto") != null)
                .requiresSecure();
    }
}
```

와 같이 요청 처리 전 스프링 시큐리티에서 먼저 핸들링 할 수 있도록 시큐리티 설정 클래스에서 HTTPS 프로토콜로 리다이렉션하도록 구성했다.

<br>

이제 여기까지 진행한 다음 재배포를 시도하면 https 프로토콜로 클라이언트와 통신할 수 있으며, http로 들어온 요청 역시 https 프로토콜로 리다이렉션 시킨다.

그러다 큰 문제가 하나 있었다.

<br>
<br>
<br>

### ***도커 컨테이너에 위치한 애플리케이션이 keystore.p12 파일을 찾을 수 없는 문제***

앞선 설치, 발급, 설정 등을 통해 https 프로토콜로 통신할 수 있는 준비는 마쳤지만, 아주 커다란 문제가 있었다.

기존 배포 방식대로 중간에 컨테이너 기술이 포함되지 않은 Github Actions - AWS 의 배포 아키텍처를 구성했다면 이런 문제는 없었겠지만,
이번에는 Docker 라는 컨테이너 기술이 포함되어 있기 때문에 문제가 발생했다.

컨테이너 내부의 스프링 부트 애플리케이션은 EC2에 직접 접근할 수 없기 때문에 발생한 문제였다.

간단히 말하면, 컨테이너 내부에서 실행 중인 애플리케이션이 컨테이너 외부에 위치한 파일을 찾지 못하는 것이다.

앞서 설명한 대로 애플리케이션 설정에는 이 keystore.p12 파일의 절대 위치를 참조해서 파일을 읽어들이는데, ec2 내부에 존재하기 때문에 컨테이너를 지나 위치를 특정하기 어려웠다.

```terminal
org.springframework.context.ApplicationContextException: Failed to start bean 'webServerStartStop'; nested exception is org.springframework.boot.web.server.WebServerException: Unable to start embedded Tomcat server
```

이러한 에러가 발생되며 애플리케이션이 종료되었다.

당연히 당시에 추가한 것이 인증서 밖에 없었고 웹서버 자체가 실행되지 않는다면 의심가는 것이 이 "인증서"였기 때문에 원인은 금방 찾을 수 있었다.

처음에는 처음 인증서를 발급 받을 당시 80포트를 사용했기 때문에 여전히 사용 중이라 포트가 문제였을 것이라 생각하고 애플리케이션 실행 포트를 8080으로 변경했지만 그것이 문제는 아니었다.

다른 설정들은 전부 정확하기 때문에 다음으로 에러 로그를 자세히 살펴보았다.

```terminal
Caused by: java.io.FileNotFoundException: /home/ec2-user/keystore.p12 (No such file or directory)
        at java.base/java.io.FileInputStream.open0(Native Method) ~[na:na]
        at java.base/java.io.FileInputStream.open(Unknown Source) ~[na:na]
        at java.base/java.io.FileInputStream.<init>(Unknown Source) ~[na:na]
        at java.base/java.io.FileInputStream.<init>(Unknown Source) ~[na:na]
        at java.base/sun.net.www.protocol.file.FileURLConnection.connect(Unknown Source) ~[na:na]
        at java.base/sun.net.www.protocol.file.FileURLConnection.getInputStream(Unknown Source) ~[na:na]
        at org.apache.catalina.startup.CatalinaBaseConfigurationSource.getResource(CatalinaBaseConfigurationSource.java:118) ~[tomcat-embed-core-9.0.82.jar!/:na]
        at org.apache.tomcat.util.net.SSLUtilBase.getStore(SSLUtilBase.java:207) ~[tomcat-embed-core-9.0.82.jar!/:na]
        at org.apache.tomcat.util.net.SSLHostConfigCertificate.getCertificateKeystore(SSLHostConfigCertificate.java:209) ~[tomcat-embed-core-9.0.82.jar!/:na]
        at org.apache.tomcat.util.net.SSLUtilBase.getKeyManagers(SSLUtilBase.java:289) ~[tomcat-embed-core-9.0.82.jar!/:na]
        at org.apache.tomcat.util.net.SSLUtilBase.createSSLContext(SSLUtilBase.java:253) ~[tomcat-embed-core-9.0.82.jar!/:na]
        at org.apache.tomcat.util.net.AbstractJsseEndpoint.createSSLContext(AbstractJsseEndpoint.java:105) ~[tomcat-embed-core-9.0.82.jar!/:na]
        ... 34 common frames omitted
```

이런 에러 로그들은 보통 맨 마지막을 보면 문제 해결에 가깝더라.

하여튼, 에러 로그를 살펴보니 keystore.p12 파일을 찾지 못해 발생했던 것이었고,
해결하기 위해 경로를 변경해보거나, 접근 권한을 수정하는 등의 작업을 거쳤지만 여전히 동일한 에러가 발생했다.

그렇다면 결국 이 파일 자체를 스프링 부트에서 찾을 수 없다는 의미가 되기 때문에 컨테이너 내부의 애플리케이션이 외부에 위치한 파일을 찾고자 했기 때문이라는 것을 깨달았다.

<br>

도커 컨테이너는 호스트 시스템의 파일 시스템과 격리되어 있다.

다시 말해, 컨테이너는 기본적으로 호스트 시스템의 파일이나 디렉토리에 직접 접근할 수 없다.

따라서 호스트 시스템에 있는 keystore.p12 파일을 도커 컨테이너에서 직접 참조하려고 하면 파일을 찾을 수 없는 문제가 발생하는 것이다.

<br>

##### **시도 1. 파일을 컨테이너 내부에 복사**

이 문제를 해결하기 위해 떠올린 것이 컨테이너 내부로 EC2 디렉토리에 배치된 파일을 복사하는 것이었다.

컨테이너가 실행되기 전에 파일을 그대로 복사하고 복사된 위치를 설정으로 잡으면 문제가 없을 것이라 생각했기 때문이다.

또한, 배포 때마다 복사하게 되면 만약 파일에 변동사항이 있을 때 마다 자동적으로 해결될 것이라 생각했다.

그래서 컨테이너 실행 이전에 ec2에 위치한 keystore.p12 파일을 복사하도록 스크립트를 변경했다.

```script
script: |
    docker rm -f $(docker ps -aq)
    cp /home/ec2-user/keystore.p12 /app/
    docker pull ${{ secrets.DOCKER_REPO }}/salog
    docker run -e SPRING_PROFILES_ACTIVE=deploy ~~~ -d -p 443:8080 ${{ secrets.DOCKER_REPO }}/salog
    docker image prune -f
// 참고로 환경 변수 주입 부분은 제거했다.
```

이 방식으로 시도해보았을 때 문제가 호스트 시스템에서 실행되는 것이 문제였다.

즉, 컨테이너가 실행되기 이전 단계이기 때문에 이 시점에서 /app/ 경로는 존재하지 않게 되고, 결국 무의미한 판단이었다.

그래서 이 방식으로는 해결하지 못했다.

<br>

##### **시도 2. 볼륨 마운트 방식**

다음으로 시도한 방식은 비슷하지만 다른 개념이었다.

이 방법은 볼륨 마운트 기능을 사용하여 호스트 시스템의 파일이나 디렉토리를 컨테이너 내부의 특정 경로에 연결하는 방식이다.

즉, 컨테이너 실행 시마다 keystore.p12 파일이 위치한 디렉토리를 "연결"해주는 방법이다.

도커는 -v 또는 --volume 옵션을 제공하여 호스트 파일 시스템을 공유할 수 없는 문제를 해결할 수 있다.

이 옵션을 사용하면 파일을 "마운트" 하여 컨테이너 내부에서 직접 접근할 수 있게 해준다.

```script
script: |
    docker rm -f $(docker ps -aq)
    docker pull ${{ secrets.DOCKER_REPO }}/salog
    docker run -e SPRING_PROFILES_ACTIVE=deploy ~~~ -v /home/ec2-user/keystore.p12:/app/keystore.p12 -d -p 443:8080 ${{ secrets.DOCKER_REPO }}/salog
    docker image prune -f
// 참고로 환경 변수 주입 부분은 제거했다.
```

이렇게 컨테이너 실행 시에 -v 옵션을 통해 호스트의 /home/ec2-user/ 디렉토리에 위치한 keystore.p12 파일을 컨테이너 내부의 /app/ 디렉토리로 마운트한다.

즉, 파일을 "공유"하는 것이다.

이를 통해 호스트 시스템의 파일을 컨테이너 내부에서 직접 수정하거나 읽을 수 있게 되었다.

이 해결법을 통해 파일의 변경 사항이 호스트와 컨테이너 양쪽에서 실시간으로 반영되기 때문에 모종의 이유로 배포를 중단해야할 필요도 없어졌다.

이 간단한 옵션을 사용하여 "무중단 배포"를 이어나갔고, 문제도 해결하여 Https 프로토콜로 종단간 통신도 구성하였다.

<br>
<br>
<br>

***

문제에 대한 이유는 대부분 도커 사용법 미흡 때문이었지만
이를 통해 도커 사용법에 대해 조금 더 자세히 알게 되었고, 보안에 있어서 기본적이지만 중요한 프로토콜의 변경 방법도 알게 되었다.

또한, 단순히 "복사"하는 것이 아니라 "마운트"하는 것으로 인증서의 변경 사항을 반영하고자 배포를 중단할 필요가 없어졌기 때문에
오히려 더욱 좋은 해결법을 도출해낼 수 있었다.
