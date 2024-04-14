---
title: "정보처리기사 실기 / iX - 소프트웨어 개발 보안 구축 ☆"
author: Jeremiah Lee
date: 2024-04-14
categories: [ 정보처리기사, 실기 ]
tags: [자격증]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Certificates/EIP/Certificate_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

실기 시험 준비

수제비 2023 참조

***

<br>
<br>
<br>

# C1. 소프트웨어 개발 보안 설계

<br>

## i. 소프트웨어 개발 보안 설계
  - 소스 코드 등에 존재하는 보안 취약점을 재거하고, 보안을 고려하여 기능을 설계 및 구현하는 보안 활동

### A. SW 개발 보안의 구성요소
  - **개발 보안 3대 요소**
    - **기밀성(Confidentiality)**
      - 인가되지 않은 개인 혹은 시스템 접근에 따른 정보 공개 및 노출을 차단하는 특성
      - 인가된 사용자에 대해서만 접근 가능
    - **무결성(Integrity)**
      - 인가된 사용자에 대해서만 자원 수정이 가능해야하며 전송 중인 정보는 수정되지 않아야 하는 특성
    - **가용성(Availability)**
      - 인가된 사용자는 가지고 있는 권한 범위 내에서 언제든 자원 접근이 가능해야 하는 특성
  - **개발 보안 용어**
    - **자산(Assets)** : 조직의 데이터 또는 조직의 소유자가 가치를 부여한 대상
    - **위협(Threat)** : 조직이나 기업의 자산에 악영향을 끼칠 수 있는 사건이나 행위
    - **취약점(Vulnerability)** : 위협이 발생하기 위한 사전 조건으로 시스템의 정보 보증을 낮추는 데 사용되는 약점
    - **위험(Risk)** : 위협이 취약점을 이용하여 조직의 자산 소실 피해를 가져올 가능성

<br>
<br>

### B. SW 개발 보안을 위한 공격기법의 이해

#### 1. DoS 공격
  - 시스템을 악의적으로 공격해서 해당 시스템의 자원을 부족하게 하여 원래 의도된 용도로 사용하지 못하게 하는 공격
  - 1대의 공격자 컴퓨터에서 타깃 시스템에 악성 패킷을 보내는 방식
  - **DoS 공격의 종류**
    - **SYN 플러딩(SYN Flooding)**
      - TCP 프로토콜의 구조적인 문제를 이용한 공격
      - 서버의 동시 가용 사용자 수를 SYN 패킷만 보내 점유하여 다른 사용자가 서버를 사용 불가능하게 하는 공격
    - **UDP 플러딩**
      - 대량의 UDP 패킷을 만들어 임의의 포트 번호로 전송, 응답 메시지(ICMP)를 생성하게 해서 지속해서 자원을 고갈시키는 공격
    - **스머프(Smurf)/스머핑(Smurfing)** 
      - 출발지 주소를 공격 대상의 IP로 설정하여 네트워크 전체에 ICMP Echo 패킷을 직접 브로드캐스팅하여 공격
    - **죽음의 핑(PoD; Ping Of Death)**
      - ICMP 패킷을 정상적인 크기보다 아주 크게 만들어 전송하면 다수의 IP 단편화 발생, 수신 측에서 단편화된 패킷을 처리하는 과정에서 부하 혹은 오버플로우 발생시켜 공격
    - **랜드 어택(Land Attack)**
      - 출발지 IP와 목적지 IP를 같은 패킷 주소로 만들어 보냄으로써 수신자가 자기 자신에게 응답을 보내게 하여 시스템의 가용성을 침해하는 공격 (출발지와 목적지 주소 동일)
    - **티어 드롭(Tear Drop)**
      - IP 패킷의 재조합 과정에서 잘못된 Fragment Offset 정보로 인해 수신 시스템이 문제를 발생하도록 만드는 공격
    - **봉크(Bonk)**
      - 패킷을 분할하여 보낼 때 처음 패킷을 1번으로 보낸 후 다음 패킷을 보낼 때도 순서번호를 모두 1번으로 조작하여 전송하는 공격
    - **보잉크(Boink)**
      - 처음 패킷을 1번으로 보낸 후 중간에 패킷 시퀀스 번호를 비정상적인 상태로 보내서 부하를 일으키는 공격

<br>

#### 2. DDoS 공격
  - 여러 대의 공격자를 분산 배치하여 동시에 동작하게 함으로써 공격
  - **DDoS 공격 구성요소**
    - **핸들러(Handler)** : 마스터 시스템의 역할을 수행하는 프로그램
    - **에이전트(Agent)** : 공격 대상에 직접 공격을 가하는 시스템
    - **마스터(Master)** : 공격자에게서 직접 명령을 받는 시스템
    - **공격자(Attacker)** : 공격을 주도하는 해커의 컴퓨터
    - **데몬 프로그램(Daemon)** : 에이전트 시스템의 역할을 수행하는 프로그램
  - **DDoS 공격 도구**
    - **Trinoo** : 많은 소스로부터 통합된 UDP flood 서비스 거부 공격을 유발하는 데 사용되는 도구
    - **Tribe Flood Network** : 많은 소스에서 하나 혹은 여러 개의 목표 시스템에 대해 서비스 거부 공격을 수행할 수 있는 도구
    - **Stacheldraht** : 분산 서비스 거부 에이전트 역할을 하는 Linux 및 Solaris 시스템용 멀웨어 도구

<br>

#### 3. DRDoS 공격
  - 공격자는 출발지 IP를 공격대상 IP로 위조하여 다수의 반사 서버로 요청 정보를 전송, 공격 대상자는 반사 서버로부터 다량의 응답을 받아서 서비스 거부가 되는 공격
  - DDoS에 비해 공격 근원지 파악이 어렵고, 트래픽 생성 효율이 좋음

<br>

#### 4. 세션 하이재킹
  - 케빈 미트닉이 사용했던 공격 방법 중 하나
  - TCP의 세션 관리 취약점을 이용한 공격
  - Victim과 Server 사이의 패킷을 스니핑하여 시퀀스 넘버를 획득, 공격자는 데이터 전송 중인 Victim과 Server 사이를 비동기화 상태로 강제 전환, 스니핑으로 획득한 클라이언트 시퀀스 넘버를 이용하여 공격

<br>

#### 5. 애플리케이션 공격
  - **HTTP GET 플러딩**
    - Cache Control Attack
    - 과도한 Get 메시지를 이용하여 웹 서버의 과부하를 유발하는 공격
  - **Slowloris**
    - HTTP GET 메서드를 사용하여 헤더의 최종 끝을 알리는 개행 문자열을 전송하지 않고 \r\n만 전송하여 대상 웹 서버와 연결상태를 장시간 지속시키고 연결 자원을 모두 소진시키는 서비스 거부 공격
  - **RUDY ATTACK**
    - 요청 헤더의 Content-Length를 비정상적으로 크게 설정하여 메시지 바디 부분을 매우 소량으로 보내 계속 연결상태를 유지시키는 공격
  - **Slow Read Attack**
    - TCP 윈도 크기를 낮게 설정하여 서버로 전달, 해당 윈도 크기를 기준으로 통신하면서 데이터 전송이 완료될 때까지 연결을 유지하게 만들어 자원 고갈 공격
  - **Hulk DoS**
    - 공격자가 공격대상 웹 사이트 URL을 지속적으로 변경하면서 다량으로 Get 요청을 발생시키는 서비스 거부 공격

<br>

#### 6. 네트워크 공격
  - **스니핑(Sniffing)**
    - 공격대상에게 직접 공격을 하지 않고 데이터만 몰래 들여다보는 수동적 공격
  - **네트워크 스캐너(Scanner), 스니퍼(Sniffer)**
    - 네트워크 하드웨어 및 소프트웨어 구성의 취약점을 파악하기 위해 공격자가 취약점을 탐색하는 공격 도구
  - **패스워드 크래킹(Password Cracking)**
    - **사전(Dictionary) 크래킹**
      - 시스템 또는 서비스의 ID와 패스워드를 크랙하기 위해서 가능성 있는 단어를 파일로 만들어 놓고 단어를 대입하여 크랙하는 공격
    - **무차별(Brute Force) 크래킹**
      - 패스워드로 사용될 수 있는 영문자, 숫자, 특수문자 등을 무작위로 입력해서 알아내는 공격
    - **패스워드 하이브리드 공격**
      - 사전 공격 + 무차별 공격
    - **레인보우 테이블 공격**
      - 패스워드 별로 해시 값을 미리 생성해서 테이블에 모아 놓고, 해시 값을 테이블에서 검색해서 역으로 패스워드 찾아내는 공격
  - **IP 스푸핑(IP Spoofing)**
    - 침입자가 본인의 패킷 헤더를 인증된 호스트의 IP 어드레스로 위조하여 타깃에 전송하는 공격
  - **ARP 스푸핑**
    - 특정 호스트의 MAC 주소를 자신의 MAC 주소로 위조한 ARP Reply 생성, 희생자에게 지속적으로 전송하여 희생자의 ARP Cache Table에 특정 호스트의 MAC 정보를 공격자의 MAC 정보로 변경, 희생자로부터 특정 호스트로 나가는 패킷을 스니핑하는 공격
  - **ICMP Redirect 공격**
    - 3계층에서 스니핑 시스템을 네트워크에 존재하는 또 다른 라우터라고 알림으로써 패킷의 흐름을 바꾸는 공격
  - **트로이 목마**
    - 악성 루틴이 숨어 있는 프로그램으로 겉보기에는 정상적인 프로그램이지만 실행하면 악성 코드가 실행됨

<br>

#### 7. 시스템 보안 위협
  - **버퍼 오버플로우(Buffer Overflow) 공격**
    - 메모리에 할당된 버퍼 크기를 초과하는 양의 데이터를 입력하여 이로 인해 프로세스의 흐름을 변경시켜서 악성 코드를 실행시키는 공격
    - **스택 버퍼 오버플로우** : 메모리 영역 중 Local Value나 함수의 Return Address가 저장되는 스택 영역에서 발생하는 오버플로우 공격
    - **힙 버퍼 오버플로우** : 프로그램 실행 시 동적으로 할당되는 힙 영역에 할당된 버퍼 크기를 초과하는 데이터를 입력하여 메모리의 데이터와 함수 주소 등을 변경, 공격자가 원하는 임의의 코드를 실행하는 공격
  - **백도어**
    - 어떤 제품이나 컴퓨터 시스템, 암호 시스템 혹은 알고리즘에서 정상적인 인증 절차를 우회하는 기법
  - **주요 시스템 보안 공격기법**
    - **포맷 스트링 공격**
      - 포맷 스트링을 인자로 하는 함수의 취약점을 이용한 공격, 외부로부터 입력된 값을 검증하지 않고 입출력 함수의 포맷 스트링을 그대로 사용하는 경우 발생하는 취약점 공격
    - **레이스 컨디션 공격**
      - 둘 이상의 프로세스나 스레드가 공유자원을 동시에 접근할 때 접근 순서에 따라 비정상적인 결과가 발생하는 조건/상황
    - **키로거 공격**
      - 컴퓨터 사용자의 키보드 움직임을 탐지해서 저장하고, ID나 패스워드 등과 같은 개인 정보를 몰래 빼가는 공격
    - **루트킷**
      - 시스템 침입 후 침입 사실을 숨긴 채 차후의 침입을 위한 백도어, 트로이 목마 등 설치, 원격 접근 등

<br>

#### 8. 보안 관련 용어
  - **스피어피싱**
    - 사회 공학의 한 기법, 특정 대상을 선정한 후 그 대상에게 일반적인 이메일로 위장한 메일을 지속적으로 발송하여, 링크 클릭을 유도해 개인정보 탈취
  - **스미싱**
    - SMS + Fishing
    - 문자메시지를 이용하여 신뢰할 수 있는 출처가 보낸 것처럼 가정, 개인 정보 탈취
  - **큐싱**
    - 큐알 코드 + 피싱
    - 큐알 코드를 통해 악성 앱을 내려받도록 유도 후 개인 정보 탈취
  - **봇넷**
    - 악성 프로그램에 감염되어 악의적인 의도로 사용될 수 있는 다수의 컴퓨터들이 네트워크로 연결된 형태
  - **APT 공격**
    - 특정 타깃을 목표로, 다양한 수단을 통한 지속적이고 지능적인 맞춤형 공격
  - **공급망 공격**
    - 개발사의 네트워크에 침투, 소스 코드의 수정 등을 통해 악의적인 코드 삽입 혹은 배포 파일 변경하는 방식을 통해 사용자 PC에 설치 또는 업데이트 시 감염
  - **제로데이 공격**
    - 보안 취약점이 발견되어 널리 공표되기 전에 해당 취약점을 악용하여 이루어지는 보안 공격, 공격의 신속성
  - **웜**
    - 스스로를 복제하여 네트워크 등의 연결을 통해 전파하는 악성 소프트웨어
  - **악성 봇**
    - 스스로 실행되지 못하고, 해커의 명령에 의해 원격에서 제어 또는 실행이 가능한 프로그램 혹은 코드
  - **사이버 킬체인**
    - 록히드 마틴의 사이버 킬체인은 공격형 방위시스템으로 지능적, 지속적 공격에 대해 7단계 프로세스별 분석 및 대응을 체계화한 APT 공격 방어 분석 모델
  - **랜섬웨어**
    - 감염된 시스템의 파일들을 암호화하여 복호화할 수 없도록 하고 몸값 요구하는 소프트웨어
  - **이블 트윈**
    - 무선 Wifi 피싱 기법, 합법적인 Wifi 제공자처럼 위장하고 연결된 사용자들의 정보를 탈취
  - **사회공학**
    - 사람들의 심리와 행동 양식을 교묘하게 이용해서 원하는 정보를 얻는 공격
  - **트러스트존**
    - 프로세서 안에 독립적인 보안 구역을 따로 두어 중요한 정보를 보호하는 APM사에서 개발한 보안 기술
  - **타이포스쿼팅**
    - 사이트에 접속할 때 주소를 실수하는 경우를 이용하기 위해 유사한 도메인을 미리 등록, URL 하이재킹
  - **다크 데이터**
    - 수집된 후 저장은 되어 있지만, 분석에 활용되지 않는 다량의 데이터

<br>
<br>

### C. 서버 인증 및 접근 통제
  - 다중 사용자 시스템과 망 운영 시스템에서 접속자의 로그인 정보를 확인하는 보안 절차

#### 1. 인증 기술의 유형
  - **지식기반 인증** : 사용자가 기억하고 있는 지식으로 인증 / IP, Password
  - **소지기반 인증** : 소지하고 있는 사용자 물품으로 인증 / 공인인증서, OTP
  - **생체기반 인증** : 고유한 사용자의 생체 정보로 인증 / 홍채, 정맥, 얼굴, 지문
  - **특징(=행위) 기반 인증** : 사용자의 특징을 활용하여 인증 / 서명, 발걸음

<br>

#### 2. 서버 접근 통제
  - 사람 또는 프로세스가 서버 내 파일에 읽기, 쓰기, 실행 등의 접근 여부를 허가하거나 거부하는 기능
  - **접근 통제 용어**
    - **주체** : 객체나 객체 내의 데이터에 대한 접근을 요청하는 능동적인 개체(행위자)
    - **객체** : 접근 대상이 수동적인 개체 혹은 행위가 일어나는 아이템(제공자)
    - **접근** : 읽고, 쓰고, 삭제하거나 수정하는 등의 행위를 하는 주체의 활동
  - **서버 접근 통제 유형**
    - **임의적 접근 통제(DAC; Discretionary Access Control)** : 주체나 그룹의 신분에 근거하여 객체에 대한 접근을 제한
    - **강제적 접근 통제(MAC; Mandatory Access Control)** : 객체에 포함된 정보의 허용등급과 접근 정보에 대해 주체가 갖는 접근 허가 권한에 근거하여 객체에 대한 접근을 제한
    - **역할 기반 접근 통제(RBAC; Role Based Access Control)** : 중앙 관리자가 사용자와 시스템의 상호관계를 통제하여 조직 내 맡은 역할에 기초하여 자원에 대한 접근을 제한
  - **3A**
    - 유무선 이동 및 인터넷 환경에서 가입자에 대한 안전하고 신뢰성 있는 인증, 권한 부여, 관리를 체계적으로 제공하는 정보 보호 기술
    - **3A 구성**
      - **인증** : 접근을 시도하는 가입자 또는 단말에 대한 식별 및 신분을 검증
      - **권한 부여** : 검증된 가입자나 단말에게 특정 수준의 권한과 서비스를 허용
      - **계정 관리** : 리소스 사용에 대한 정보를 수집하고 관리하는 서비스
  - **인증 관련 기술**
    - **SSO(Single Sign On)** : 커버로스에서 사용되는 기술, 한 번의 인증 과정으로 여러 컴퓨터상의 자원을 이용할 수 있도록 해주는 인증 기술
    - **커버로스(Kerberos)** : 1980년대 중반 MIT의 Athena 프로젝트의 일환으로 개발, 클라이언트-서버 모델에서 동작하고 대칭 키 암호기법에 바탕을 둔 티켓 기반의 프로토콜

<br>

#### 3. 접근 통제 보호 모델
  - **벨-라파듈라 모델(BLP; Bell-LaPadula Policy)**
    - 미 국방부 지원 보안 모델, 보안 요소 중 기밀성을 강조, 강제적 정책에 의해 접근 통제
    - **벨-라파듈라 모델 속성**
      - **No Read Up** : 보안수준이 낮은 주체는 보안 수준이 높은 객체를 읽어서는 안됨
      - **No Write Down** : 보안수준이 높은 주체는 보안수준이 낮은 객체에 기록하면 안됨
  - **비바 모델(Biba Model)**
    - 벨-라파듈라 모델의 단점 보완, 무결성 보장하는 최초의 모델
    - **비바 모델 속성**
      - **No Read Down** : 높은 등급의 주체는 낮은 등급의 객체를 읽을 수 없음
      - **No Write Up** : 낮은 등급의 주체는 상위 등급의 객체를 수정할 수 없음

<br>
<br>

### D. SW 개발 보안을 위한 암호화 알고리즘
  - 데이터의 무결성 및 기밀성 확보를 위해 정보를 쉽게 해독할 수 없는 형태로 변환하는 기법
  - **암호 알고리즘 관련 주요 용어**
    - **평문(Plain/Plaintext)** : 암호화되기 전의 원본 메시지
    - **암호문(Cipher/Ciphertext)** : 암호화가 적용된 메시지
    - **암호화(Encrypt)** : 평문을 암호문으로 바꾸는 작업
    - **복호화(Decrypt)** : 암호문을 평문으로 바꾸는 작업
    - **키** : 적절한 암호화를 위해 사용하는 값
    - **치환 암호(대치 암호, Substitution Cipher)** : 비트, 문자 또는 문자의 블록을 다른 비트, 문자 또는 블록으로 대체하는 방법
    - **전치 암호(Transposition Cipher)** : 비트, 문자 또는 블록이 원래 의미를 감추도록 자리바꿈 등을 히용하여 재배열하는 방법

#### 1. 암호 알고리즘 방식
  - **양방향 방식**
    - **대칭 키 암호 방식**
      - 암호화와 복호화에 같은 키를 사용
      - **블록 암호 방식**
        - 긴 평문을 암호화하기 위해 고정 길이의 블록을 암호화하는 블록 암호 알고리즘을 반복하는 방법
        - DES / AES / SEED
      - **스트림 암호 방식**
        - 매우 긴 주기의 난수열을 발생시켜 평문과 더불어 암호문을 생성하는 방식
        - RC4
    - **비대칭 키 암호 방식**
      - 공개키 암호 방식
      - 공개키는 누구나 비밀키는 소유자만
      - RSA / ECC / ElGamal / 디피-헬만 
  - **일방향 방식**
    - 임의 길이의 정보를 입력받고 고정된 길이의 암호문을 출력하는 암호 방식
    - **해시 암호 방식**
      - **MAC(Message Authentication Code)** : 키를 사용하는 메시지 인증 코드로 메시지의 정당성을 검증하기 위해 메시지와 함께 전송되는 값
      - **MDC(Modification Detection Code)** : 키를 사용하지 않는 변경 감지 코드로 수신자는 받은 데이터로부터 새로운 MDC를 생성하여 송신자에게 받은 MDC와 비교하여 해당 메시지가 변경되지 않았음을 보장하는 값

<br>

#### 2. 암호 알고리즘 상세
  - **대칭 키 암호화 알고리즘**
    - **DES(Data Encrypt Standard)**
      - IBM에서 개발, 미연방 표준
      - 블록 크기는 64bit, 키 길이는 56bit인 페이스텔 구조, 16라운드 암호화 알고리즘
    - **AES(Advanced Encrypt Standard)**
      - 미국 표준 기술 연구소(NIST)에서 발표한 블록 암호화 알고리즘
      - 블록 크기 128bit, 키 길이에 따라 128, 192, 256bit로 분류
    - **SEED** 
      - 한국인터넷진흥원(KISA)이 개발
      - 128bit 비밀키로부터 생성된 16개의 64bit 라운드 키를 사용하여 총 16회의 라운드를 거쳐 128bit 평문 블록을 128bit 암호문 블록으로 암호화하여 출력하는 방식
      - 블록 크기 128bit, 키 길이에 따라 128, 182, 256bit로 분류
    - **ARIA(Academy, Research Institute, Agency)**
      - 국가정보원과 산학연구협회가 개발
      - 경량 환경 및 하드웨어에서의 효율성 향상을 위해 개발
      - 블록 크기 128bit, 키 길이에 따라 128, 182, 256bit로 분류
    - **IDEA(International Data Encrypt Algorithm)**
      - DES를 대체하기 위해 스위스 연방기술 기관에서 개발
      - 128bit의 키를 사용하여 64bit의 평문을 8라운드에 거쳐 64bit의 암호문들 만듦
    - **LFSR(Linear Feedback Shift Register)**
      - 시프트 레지스터의 일종, 레지스터에 입력되는 값이 이전 상태 값들의 선형 함수로 계산되는 구조로 되어 있는 스트림 암호화 알고리즘
    - **Skipjack**
      - 미 국가안보국(NSA)에서 개발
      - Fortezza Card에 칩 형태로 구현
      - 음성을 암호화하는 데 주로 사용
  - **비대칭 키 암호화 알고리즘**
    - **RSA(Rivest-Shamir-Adleman)**
      - MIT 수학 교수 세명이 고안
      - 큰 인수의 곱을 소인수 분해하는 수학적 알고리즘을 사용
    - **디피-헬만(Diffie-Hellman)**
      - 최초의 공개키 알고리즘
      - 유한 필드 내에서 이산대수의 계산이 어려운 문제가 기본 원리
    - **ECC(Elliptic Curve Cryptography)**
      - 코블리치와 밀러가 RSA 암호 방식에 대한 대안으로 제안
      - 타원 곡선 암호는 유한체 위에서 정의된 타원곡선 군에서의 이산대수의 문제에 기초한 알고리즘
    - **ElGamal**
      - 전사서명과 데이터 암-복호화에 함께 사용 가능
      - 이산대수의 계산이 어려운 문제가 기본 원리
  - **해시 암호화 알고리즘**
    - **MD5(Message-Digest algorithm 5)**
      - R.rivest가 MD4를 개선한 알고리즘
      - 프로그램이나 파일의 무결성 검사에 사용
      - 512bit 짜리 입력 메시지 블록에 대해 차례로 동작하여 128bit의 해시값을 생성
    - **SHA-1(Secure Hash Algorithm)**
      - NSA에서 미 정부 표준으로 지정
      - 160bit의 해시값을 생성
    - **SHA-256/384/512**
      - 256bit의 해시값을 생성
    - **HAS-160**
      - KCDSA를 위해 개발된 해시함수
    - **HAVAL**
      - 메시지를 1024bit로 나누고 128,150,192,224,256 비트인 메시지 다이제스트를 출력하는 알고리즘

<br>
<br>

### E. 안전한 전송을 위한 데이터 암호화 전송

#### 1. IPSec
  - IP 계층(3계층)에서 무결성과 인증을 보장하는 인증 헤더와 기밀성을 보장하는 암호화를 이용한 IP 보안 프로토콜

<br>

#### 2. SSL/TLS
  - 전송계층(4계층)과 응용계층(7계층) 사이에서 클라이언트-서버 간의 웹 데이터 암호화, 상호 인증 및 전송 시 데이터 무결성을 보장하는 보안 프로토콜

<br>

#### 3. S-HTTP(Secure Hypertext Transfer Protocol)
  - 웹상에서 네트워크 트래픽을 암호화하는 주요 방법 중 하나, 클라이언트-서버 간에 전송되는 모든 메시지를 각각 암호화하여 전송하는 기술

***

<br>
<br>
<br>
<br>

# C2. 소프트웨어 개발 보안 구현

<br>

## i. SW 개발 보안 구현

### A. 시큐어 코딩 가이드
  - 설계 및 구현 단계에서 해킹 등의 공격을 유발할 가능성이 있는 잠재적인 보안 취약점을 사전 제거하고 외부 공격으로붙 ㅓ안전한 소프트웨어를 개발하는 기법

<br>
<br>

### B. 입력 데이터 검증 및 표현
  - 입력 데이터로 인해 발생하는 문제들을 예방하기 위해 구현 단계에서 검증해야 하는 보안 점검 항목들

#### 1. 입력 데이터 검증 및 표현 취약점
  - **XSS(Cross Site Scripting)**
    - 검증되지 않은 외부 입력 데이터가 포함된 웹페이지가 전송되는 경우, 사용자가 해당 웹페이지를 열람함으로써 웹페이지에 포함된 부적절한 스크립트가 실행되는 공격
    - **XSS 공격 유형**
      - **Stored XSS** : 방문자들이 악성 스크립트가 포함된 페이지를 읽어 봄과 동시에 악성 스크립트가 실행, 감염
      - **Reflected XSS** : 공격용 악성 URL을 생성한 후 이메일로 사용자에게 전송, 해당 URL 클릭 시 즉시 공격 스크립트가 피해자로 반사
      - **DOM(Document Object Model) XSS** : 공격자는 DOM 기반 XSS 취약점이 있는 브라우저를 대상으로 조작된 URL을 이메일을 통해 발송, 클릭 시 공격
  - **CSRF(Cross-Site Request Forgery)**
    - 사용자가 자신의 의지와는 무관하게 공격자가 의도한 행위를 특정 웹사이트에 요청하게 하는 공격
  - **SQL 삽입(SQL Injection)**
    - 응용 프로그램의 보안 취약점을 이용해서 악의적인 SQL 구문을 삽입, 실행시켜서 DB에 접근하는 공격
    - **SQL 삽입 공격 유형**
      - **Form SQL Injection** : HTML Form 기반, 사용자 인증을 위한 쿼리 문의 조건을 임의로 조작하여 인증을 우회하는 공격
      - **Union SQL Injection** : 쿼리의 UNION 연산자를 사용하여 한 쿼리의 결과를 다른 쿼리의 결과에 결합하여 공격
      - **Stored Procedure SQL Injection** : 저장 프로시저를 사용하여 공격
      - **Mass SQL Injection** : 기존 SQL Injection의 확장 개념, 한 번의 공격으로 대량의 DB 값이 변조되어 치명적인 영향을 미치는 공격
      - **Error-Based SQL Injection** : DB 쿼리에 대한 에러값을 기반으로 한 단계씩 점진적으로 DB 정보를 획득
      - **Blind SQL Injection** : DB 쿼리에 대한 오류 메시지를 반환하지 않으면 공격할 수 없는 Error-Based 와 달리 오류 메시지가 아닌 쿼리 결과의 참과 거짓을 통해 의도하지 않은 SQL 문을 실행함으로써 공격

<br>
<br>

### C. 보안 기능
  - 개발 단계에서 인증, 접근제어, 기밀성, 암호화, 권한 관리 등을 적절하게 구현하기 위한 보안 점검 항목들

<br>
<br>

### D. 에러 처리
  - 프로그램 실행 시 발생하는 에러를 예외 처리하지 못하거나, 에러 정보에 중요한 정보가 포함될 때 발생할 수 있는 취약점을 예방하기 위한 보안 점검 항목들

<br>
<br>

### E. 세션 통제
  - 다른 세션 간 데이터 공유 등 세션과 관련되어 발생할 수 있는 취약점을 예방하기 위한 보안 점검 항목들

<br>
<br>

### F. 코드 오류
  - 구현 단계에서 프로그램 변환 시 오류, 서버의 리소스의 부적절한 반환 등 흔히 실수하는 프로그램 오류를 예방하기 위한 보안 항목들

<br>
<br>

### G. 캡슐화
  - 외부에 은닉이 필요한 중요 데이터와 필요한 기능성을 불충분하게 캡슐화했을 때 인가되지 않은 사용자에게 데이터 유출 등이 발생할 수 있는 취약점 예방을 위한 보안 검증 항목들

<br>
<br>

### H. API 오용
  - 서비스에서 제공되는 이용에 반하는 방법으로 API를 이용하거나 보안에 취약한 API를 오용하여 발생할 수 있는 보안 취약점 예방을 위한 보안 검증 항목들

<br>
<br>
<br>

## ii. 시스템 보안 구현

### A. 유닉스/리눅스 주요 로그 파일
  - **wtmp/wtmpx** : 사용자 로그인/로그아웃 정보
  - **utmp/utmpx** : 현재 시스템에 로그인한 사용자 정보
  - **btmp/btmpx** : 로그인에 실패한 정보
  - **lastlog** : 사용자별 최근 로그인 시간 및, 접근한 소스 호스트에 대한 정보
  - **sulog** : switch user 명령어 실행 성공/실패 결과에 대한 정보
  - **acct/pacct** : 사용자별로 실행되는 모든 명령어에 대한 로그
  - **xferlog** : FTP 서비스 데이터 전송 기록 로그
  - **messages** : 부트 메시지 등 시스템의 기본 로그 파일, 운영 전반 메시지 저장
  - **secure** : 보안 관련 주요 로그 기록

<br>
<br>

### B. 보안 솔루션
  - **방화벽(Firewall)** : 기업 내외부 간 트래픽을 모니터링하여 접근 허용/차단 시스템
  - **웹 방화벽(WAF; Web Application Firewall)** : SQL 삽입, XSS 등 웹 공격 탐지, 차단하는 웹 애플리케이션 보안에 특화된 보안장비
  - **네트워크 접근 제어(NAC; Network Access Control)** : 바이러스나 웜 둥의 위협 제어, 단말기가 내부 네트워크 접속 시도 시 제어
  - **침입 탐지 시스템(IDS; Intrusion Detection System)** : 네트워크에서 발생하는 이벤트 모니터링, 비인가 사용자 행위 실시간 탐지
  - **침입 방지 시스템(IPS; Intrusion Prevention System)** : 네트워크에 대한 공격, 침입 실시간 차단, 유해트래픽 조치 능동 처리
  - **무선 침입 방지 시스템(WIPS; Wireless Intrusion Prevention System)** : 인가되지 않은 무선 단말기의 접속을 자동 탐지 및 차단, 보안에 취약한 무선 공유기 탐지
  - **통합 보안 시스템(UTM; Unified Threat Management)** : 다양한 보안 장비의 기능을 하나로 통합
  - **가상사설망(VPN; Virtual Private Network)** : 인터넷과 같은 공중망에 인증, 암호화, 터널링 기술을 활용하여 전용망과 같은 효과를 가지는 보안 솔루션
  - **SIEM(Security Information and Event Management)** : 다양한 보안 장비와 서버, 네트워크 장비 등으로부터 보안 로그와 이벤트 정보를 수집한 후 정보 간의 연관성을 분석하여 위협 상황을 인지하고 침해사고에 신속 대응하는 보안 관제 솔루션
  - **ESM(Enterprise Security Management)** : 여러 보안 시스템으로부터 발생한 각종 이벤트 및 로그를 통합해서 관리, 분석, 대응하는 전사적 통합 보안 관리 시스템

#### 1. 시스템 보안 솔루션
  - **스팸 차단 솔루션** : 메일 서버 앞단에 위차하여 프록시 메일 서버로 동작
  - **보안 운영체제** : 컴퓨터 운영체제의 커널에 보안 기능을 추가한 솔루션

<br>

#### 2. 콘텐츠 유출 방지 보안 솔루션
  - **보안 USB** : 정보 유출 방지 등의 보안 기능을 갖춘 USB 메모리
  - **데이터 유출 방지(DLP; Data Loss Prevention)** : 조직 내부 중요 자료가 외부로 나가는 것을 탐지, 차단
  - **디지털 저작권 관리(DRM; Digital Right Management)** : 디지털 저작물에 대한 보호와 관리를 위한 솔루션

<br>
<br>
<br>

## iii. SW 개발 보안 테스트와 결함 관리
  
### A. 소프트웨어 개발 보안 테스트 유형
  - **정적 분석** : SW를 실행하지 않고 보안 약점 분석
  - **동적 분석** : SW 실행환경에서 보안 약점 분석

<br>
<br>
<br>

## iV. 비지니스 연속성 계획(BCP)
  - 각종 재해, 장애, 재난으로부터 위기관리를 기반으로 복구 등을 통해 비지니스 연속성을 보장하는 체계

### A. 비지니스 연속성 계획 관련 주요 용어
  - **BIA(Business Impact Analysis)** : 장애나 재해로 인해 운영상의 주요 손실을 볼 것을 가정하여 시간에 따른 영향도 및 손실평가를 조사하는 BCP를 구축하기 위한 비지니스 영향 분석
  - **RTO(Recovery Time Objective)** : 업무중단 시점부터 업무가 복귀되어 다시 가동될 때까지의 시간
  - **RPO(Recovery Point Objective)** : 업무중단 시점부터 데이터가 복구되어 다시 가동될 때 데이터의 손실 허용 시점
  - **DRP(Disaster Recovery Plan)** : 재난으로 장기간에 걸쳐 시설의 운영이 불가능한 경우를 대비한 재난 복구 계획
  - **DRS(Disaster Recovery System)** : 재해복구계획의 원활한 수행을 지원하기 위해 평상 시 확보해 두는 인적, 물적 자원 및 이들에 대한 지속적인 관리체계가 통합된 재해복구센터
    - **Mirror Site** : 주 센터와 데이터복구센터 모두 운영 상태로 실시간 동시 서비스가 가능한 재해복구센터
    - **Hot Site** : 주 센터와 동일한 수준의 자원을 대기 상태로 원격지에 보유하면서 동기, 비동기 방식의 미러링을 통해 데이터의 최신 상태를 유지하고 있는 재해복구센터
    - **Warm Site** : 중요성이 높은 자원만 부분적으로 재해복구센터에 보유하고 있는 센터
    - **Cold Site** : 데이터만 원격지 보관, 재해 시 데이터를 근간으로 필요 자원을 조달하여 복구할 수 있는 재해복구 센터

<br>
<br>
<br>

## V. 보안 용어

### A. 보안 공격 관련 용어
  - **부 채널 공격** : 암호화 알고리즘의 실행 시기의 전력 소비, 전자기파 방사 등의 물리적 특성을 측정하여 암호 키 등 내부 비밀 정보를 부 채널에서 획득하는 공격
  - **드라이브 바이 다운로드** : 해커가 불특정 웹 서버와 웹 페이지에 악성 스크립트 설치, 접속 시 실행되어 감염
  - **워터링홀** : 특정인에 대한 표적 공격, 특정인이 잘 방문하는 웹 사이트에 악성 코드를 심거나 악성코드를 배포하는 URL로 자동 유인
  - **비지니스 스캠** : 기업 이메일 계정 도용하여 대금 가로채는 범죄
  - **하드 블리드** : OpenSSL 암호화 라이브러리의 하트비트라는 확장 모듈에서 클라이언트 요청 메시지를 처리할 때 데이터 길이에 대한 검증 수행을 하지 않는 점을 사용한 공격
  - **크라임웨어** : 중요 정보를 탈취하고나 유출을 유도하여 금전적인 이익 등의 범죄행위 목적 악성코드
  - **토르 네트워크** : 네트워크 경로를 알 수 없도록 암호화 기법을 사용하여 데이터 전송, 익명으로 인터넷 사용 가상 네트워크
  - **MITM 공격** : 네트워크 통신을 조작하여 통신 내용 도청 및 조작
  - **DNS 스푸핑 공격** : 공격 대상에게 전달되는 DNS Lookup을 조작하거나 DNS 캐시 정보를 조작하여 의도되지 않은 주소로 접속되게 하는 공격
  - **포트 스캐닝** : 공격자가 침입 전 대상의 호스트에 어떤 포트가 활성화되어 있는지 확인하는 기법
  - **디렉토리 리스팅** : 웹 애플리케이션을 사용하고 있는 서버의 미흡한 설정으로 인해 인덱싱 기능이 활성화 되어 있을 경우 모든 디렉토리 및 파일 목록을 볼 수 있는 취약점
  - **리버스 쉘 공격** : 타깃 서버가 클라이언트로 접속해서 클라이언트가 타깃 서버의 쉘을 획득 후 공격
  - **익스 플로잇** : 소프트웨어나 하드웨어의 버그 또는 취약점을 이용, 공격자가 의도한 동작 혹은 명령 실행하도록 하는 코드나 행위
  - **스턱스넷 공격** : 독일 지멘스사의 SCADA 시스템 공격 목표로 제작된 악성코드
  - **크리덴셜 스터핑** : 사용자 계정을 탈취해서 공격, 다른 곳에 유출된 로그인 정보를 무작위 대입

<br>
<br>

### B. 보안 공격 대응 관련 용어
  - **허니팟** : 의도적으로 설치해 둔 시스템으로 노출시켜 해커 유인
  - **OWASP Top 10** : 취약점 중 경격 빈도가 높으며 보안상 큰 영향을 줄 수 있는 10가 취약점에 대한 가이드
  - **핑거프린팅** : 멀티미디어 콘텐츠에 저작권 정보와 구매한 사용자 정보를 삽입 후 불법 배포자 위치 추적
  - **워터마킹** : 디지털 콘텐츠에 저작권자 정보 삽입
  - **이상금융거래탐지시스템(FDS; Fraud Detection System)** : 전자금융거래에 사용되는 정보를 종합 분석 후 의심 거래 탐지, 이상 거래 차단
  - **CC(Common Criteria)** : 정보기술의 보안 기능과 보증에 대한 평가 기준
  - **사이버 위협정보 분석 공유시스템(C-TAS; Cyber Threats Analysis System)** : 사이버 위협정보를 체계적 수립해서 자동화된 정보공유
  - **장착형 인증 모듈(PAM; Pluggable Authentication Module)** : 리눅스 시스템 내에서 사용되는 각종 애플리케이션 인증을 위해 제공되는 라이브러리
  - **CVE(Common Vulnerabilities and Exposures)** : 미국 MITRE 사에서 공개적으로 알려진 소프트웨어의 보안취약점을 표준화한 식별자 목록
  - **CWE(Common Weakness Enumeration)** : 미국 MITRE 사가 중심이 되어 공통적으로 발생하는 약점을 분류한 목록
  - **ISMS(Information Security Management System)** : 정보보호 관리체계, 조직의 주요 정보자산을 보호하기 위해 정보보호 관리 절차와 과정을 수립하여 지속 관리하고 운영하기 위한 종합적인 체계
  - **PIMS(Personal Security Management System)** : 기업이 개인정보보호 활동을 체계적-지속적으로 수행하기 위해 필요한 보호조치 쳬게를 구축했는지 여부를 점검, 평가하는 인증제도
  - **PIA(Privacy Impact Assessment)** : 개인정보를 활용하는 새로운 정보 시스템의 도입이나 개인정보 취급이 수반되는 기존 정보 시스템의 중대한 변경 시, 사전 조사 및 예측 검토하여 개선 방안 도출하는 체계적 절차
  - **TKIP(Temporal Key Integrity Protocol)** : 임시 키 무결성 프로토콜, 안전하지 않은 WEP 암호화 표준을 대체하기 위한 암호 프로토콜

***

<br>
<br>
<br>

***

이번 과목은 꾸준히 1~2문제 출제되는 과목이다.

그만큼 중요하고, 외울 내용이 많다.

이번 과목을 작성할 때는 중요도가 낮은 내용은 일부분 빼버렸다.    
그렇기 때문에 지금 적혀있는 내용 전부 다 중요하고, 단어에 대해 물어보는 시험인 특성상 전부 출제될 확률이 높다.

그래서 사실상 전부 외워야한다.

대부분 단어 - 내용 으로 묶어서 정리되어 있긴한데, 너무 많아도 별 수 없다.

출제 되었던 건 마찬가지로 두번 나오거나 할 확률이 적고,
대신 그 바운더리 내에서 출제되는 경향이 있긴 하지만 그럼에도 많다...

정 안되겠으면 "공격" 만이라도 외우자

주로 공격 용어에 대해 물어볼 확률이 높고
그외에 지엽적인 내용은 확률이 적을 수 있고, 나와도 틀린다는 생각을 해야할 수 있다.

왜냐면 이 보안 과목 말고도 SQL 3~4 문제 / 응용 SW 기초기술 활용 2~3문제 / 프로그래밍 언어 활용 7~9문제로
중요한 개념과 내용이 많기 때문이다.

다시 말하면 위 세 과목을 다 맞는다면 다른 과목은 전부 버려도 될거라는 뜻이다.    
그런데 다 맞으면 좋겠지만 그렇지 못할 가능성이 높기 때문에 다른 과목도 공부하는 것이다.

여튼 말이 길어졌지만 이번 과목은 단어-내용으로 최대한 외우자.
안 되겠으면 "공격" 부분 만이라도.