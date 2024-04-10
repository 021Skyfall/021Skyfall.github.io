---
title: "SSL/TLS 인증서의 개념"
author: Jeremiah Lee
date: 2024-03-22
categories: [ TLS 인증서 ]
tags: [인증서, 통신, 개념]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/solo_study_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

### ***개요***

"샐로그" 프로젝트 당시, 우리는 서버와 클라이언트가 HTTPS 프로토콜로 통신할 수 있도록 프로젝트가 배포된 EC2 인스턴스의 퍼블릭 DNS 주소를
CNAME의 값으로 주고 도메인을 설정한 다음 SSL 인증서를 발급 받아 서버의 통신 프로토콜을 HTTPS로 변경했었다.

[프로젝트:샐로그 / 보안 2 - Https 프로토콜 적용과 문제해결](https://021skyfall.github.io/posts/salog_project_security2_https_protocol-1/)

그러나 이 인증서가 SSL 인증서인지 혹은 TLS 인증서 인지, 아니면 SSL/TLS 인증서인지 여전히 헷갈린다.

이번 기회에 이 개념을 바로 잡고자 이번 포스트를 작성한다.

<br>
<br>
<br>

### ***SSL? TLS?***

우선 "인증서"라는 것이 검증된 제 3자를 통해 우리의 웹 사이트가 인증된 사이트라는
서명을 받아 안전한 통신을 할 수 있도록 말 그대로 "인증"하는 것이라는 건 대충 알고있다.

그렇다면 이 통신 인증서와 항상 함께 등장하는 SSL, TLS는 무엇일까?

<br>

당장 SSL/TLS를 풀어 쓰면 SSL은 Secure Sockets Layer로 보안 소켓 계층을 의미하고, TLS는 Transport Layer Security로 전송 계층 보안을 의미한다.

뭔가 많이 비슷하다는 생각이 든다.

그렇다, SSL과 TLS은 명칭에서 약간 차이가 있을 뿐 동일한 의미를 나타내는 용어이다.

<br>

그러면 왜 구분을 하는가?

이는 단순히 "버전"에 대한 차이이다.

오래되어 결함이 있는 SSL을 보완하기 위해 업데이트된 것이 TLS이다.

SSL 3.0 버전 이후 부터는 TLS 1.0 버전으로 부르는 등, TLS는 SSL의 업그레이드 버전이라고 생각하면 된다.

<br>

SSL은 보안 소켓 계층의 약자로, 네트워크 상의 두 디바이스 또는 애플리케이션 간에 보안 연결을 생성하는 통신 프로토콜 또는 규칙 세트이다.

인터넷을 통해 보안 인증이나 데이터를 공유하기 전, 신뢰를 구축하고 상대방을 인증하는 것이 중요한데,
SSL은 애플리케이션 또는 브라우저가 모든 네트워크에서 안전하고 암호화된 통신 채널을 만드는 데 사용할 수 있는 기술이다.

그런데, 이 SSL은 앞서 말했듯이 오래된 기술이기 때문에 몇 가지 보안적 결함이 있는 기술이다.

이러한 SSL의 기존 취약성을 수정하고 업그레이드된 것이 TSL, 전송 계층 보안 기술이다.

이 TSL을 통해 더욱 효율적으로 상대방을 인증하고 암호화된 통신 채널을 지원할 수 있다.

<br>

즉, 이 기술을 통해 웹 브라우저와 웹 서버 간의 송수신 되는 개인 정보 및 데이터를 암호화하여 안전하게 전송할 수 있도록 한다.

이러한 기술을 사용할 수 있게 해주는 것이 디지털 문서인 "인증서"이다.

이 인증서에는 보통 웹사이트의 도메인 이름, 회사 이름, 회사 위치 등의 구체적인 정보와 함께 공개키가 포함되어 있으며,
이 공개키를 활용해 통신을 암호화하는 것이다.

이를 활용하면 웹 사이트의 주소 중 프로토콜이 HTTP가 아닌 HTTPS로 변경이 되고, 크롬 기준으로 주소 좌측이 자물쇠로 표현된다.

<br>
<br>
<br>

### ***SSL과 TLS의 차이점***

차이점이라고는 하지만 동일한 기술에 TLS가 상위호환인 기술이기 때문에 업그레이드 된 점 이라고 생각하는 편이 올바르다.

아래는 [AWS SSL/TLS 인증서 공식 문서](https://aws.amazon.com/ko/compare/the-difference-between-ssl-and-tls/)
에서 참고한 내용이다.

![](/assets/img/solo_study/certification/화면%20캡처%202024-03-26%20143712.png)

<br>
<br>
<br>

### ***인증서의 타입과 레벨***

인증서에는 타입과 레벨이라는 것이 있다.

우선 타입은 싱글 / 멀티 / 와일트카드 이렇게 3가지 종류로 나누어진다.

각각 싱글은 Full-Domain 기준으로 1개의 인증서가 발급된다. 즉, 1개의 도메인당 1개의 인증서가 발급되는 형태이다.

다음으로 멀티의 경우, 1개의 대표 도메인에 여러 도메인을 존속시켜서 1개의 인증서처럼 보이도록 발급된다.

멀티 인증서는 싱글 인증서와 마찬가지로 도메인 1개당 비용이 발생하지만, 여러 도메인을 하나의 인증서로 통합해서 관리할 수 있다는 장점이 있다.

마지막으로 와일드 카드는 *.domain.com 형식으로 인증서가 발급된다.

여기서 * 자리의 sub-domain은 무제한으로 적용되는 인증서이다. 단, base-domain이 동일해야한다.

<br>

다음으로 레벨은 신뢰 수준에 따라 DV / OV / EV 세 단계로 나뉜다.

DV (Domain Validation)는 도메인 검증을 기본으로 하며, 다른 레벨에 비해 간편하고, 인증서를 빠르게 발급 받을 수 있다.

주로 소규모 업체나 그룹웨어 서비스에서 많이 이용한다.

OV (Organization Validation)는 인증서 정보상에 회사명과 주소가 표기되어 있는 신뢰도 높은 인증서이며,
주로 공공기관 또는 비지니스 기업에서 선호하는 기업 표준형 인증서이다.

EV (Extended Validation)는 인증서 중 가장 레벨이 높고 신뢰도가 가장 높은 인증서이며,
민감한 개인정보를 다루는 정부기관 및 금융기관, 마이데이터 mTLS 인증서로 많이 사용하는 등급이다.

<br>
<br>
<br>

### ***인증서를 왜 사용해야하는가***

사실 앞선 개념들로 SSL/TLS 인증서를 사용하는 이유는 충분히 알았을 것 같다.

그래도 정리하고 넘어가고자 한다.

<br>

인증서를 사용하는 데에는 여러 가지 이유가 있다.

딱 한 가지를 뽑으라면 당연히 "보안" 때문이다.

이러한 이유를 상세히 정리하면 아레와 같다.

1. 민감한 데이터 보호
   - SSL/TLS 인증서는 브라우저와 웹 사이트 간의 전송되는 데이터를 암호화하여 안전하게 전송하는데 도움이 된다.
2. 고객 신뢰 강화
   - 고객들은 방문하는 웹 사이트가 개인정보를 안전하게 보호하고 있는지 민감해 한다.
   - 인증서를 통해 안전하게 보호되고 있음을 알 수 있다.
3. 업계 규제 준수
   - 일부 기업은 데이터 기밀성 및 보호에 대한 업계 규정을 준수해야 한다.
   - 안전한 온라인 통신을 위한 업계 요구 사항으로 SSL/TLS 인증서를 통해 웹 서버를 보호하는 것을 포함한다.
4. SEO 순위 상승
   - 주요 검색 엔진에서 SSL/TLS 보안 웹 사이트는 SSL/TLS 인증서가 없는 유사한 웹 사이트보다 더 높은 노출 순위를 차지할 수 있다.
   - 이로 인해 방문자가 증가할 수 있다.

<br>
<br>
<br>

주요 내용들은
[AWS SSL/TLS 인증서 공식 문서](https://aws.amazon.com/ko/compare/the-difference-between-ssl-and-tls/)
와 [써트코리아 공식 문서](https://www.certkorea.co.kr/ssl/sp.php?p=11)
를 참고했다.

<br>
<br>
<br>

***

SSL과 TLS가 서로 다른 기술이고 완전히 다른 인증 기술을 사용하는 줄 알았는데, 
그게 아니라 SSL의 업그레이드 버전이 TLS이고 TLS 인증서로 불러야 맞다는 것을 알게되었다.

AWS 공식 문서에도 나와있지만 AWS에서는 TLS 1.2 이상을 지원한다고 되어 있다.

아마 AWS 뿐만아니라 대부분 웹 서비스는 SSL/TLS를 공통 지원할 것이다.

이렇게 잘 모르는 내용을 정리하면서 내 지식이 되어 간다는게 뿌듯하다.