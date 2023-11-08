---
title: "Chapter7 - 네트워크 보안 기술 C"
author: Jeremiah Lee
date: 2023-09-13
categories: [ 그림으로 배우는 네트워크 원리 ]
tags: [책]
pin: true
math: true
mermaid: true
image: /assets/img/StudyGroupLog/Network_for_pic.png
path: /commons/devices-mockup.png
lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
alt: Something
---
***

### 7-9

#### 거점 간 통신을 저비용으로 안전하게 수행한다

<br>

##### a. 거점 간 통신에 인터넷을 사용한다

기업의 복수 거점 LAN끼리 통신하기 위해서 WAN을 이용한다. WAN을 통해서 자사의 거점 LAN끼리만 통신할 수 있는 사설 네트워크를 만들 수 있다. WAN 통신사업자가 제대로 보안을 확보해 준다는 장점도 있다. 단, WAN을 이용하려면 상당한 비용이 들어간다.


WAN 구축 비용과 비교하면 인터넷 접속 비용은 훨씬 저렴하다. 단, 인터넷은 누가 접속했는지 알 수 없으므로, 데이터가 도청되는 등 보안에 관한 위험이 있다.

<br>

#### b. 인터넷을 사설 네트워크로


인터넷을 경유해 거점 간 통신을 안전하게 할 수 있도록 인터넷 *VPN을 이용한다. 인터넷을 가상으로 사설 네트워크처럼 다루는 기술이다. 여러 가지 구현 방법이 있지만, 인터넷 VPN의 주요 구현 방법은 아래와 같다.

1. 거점 LAN의 라우터 사이를 가상으로 연결한다.(=터널링)

2. 거점 LAN 간의 통신은 터널을 경유하도록 라우팅한다.

3. 터널을 경유하는 데이터를 암호화한다.

<br>

데이터 암호화에는 IPSec이나 SSL과 같은 암호화 프로토콜을 이용한다. 덧붙여, 일반 인터넷으로 보내는 데이터는 암호화하지 않은 채 그대로 전송한다.


*VPN : Virtual Private Network

<br>

### 정리

- 인터넷을 사설 네트워크인 것처럼 다루는 기술이 인터넷 VPN
- 인터넷 VPN에서는 거점 간 라우터를 가상으로 연결함
- 거점 간 데이터는 터널을 경유하도록 해서 암호화도 함

<br>
<br>
<br>

***

그림으로 배우는 네트워크 원리 종료

​

2회차 정독 후에 [만들면서 배우는 클린 아키텍처] 공부 할 것
