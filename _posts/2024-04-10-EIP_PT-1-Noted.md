---
title: "정보처리기사 실기 1 / 요구사항 확인"
author: Jeremiah Lee
date: 2024-04-10
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
[코딩하는 핑가 블로그](https://ss-o.tistory.com/95?category=945579) 참조

<br>
<br>
<br>

### C1. 소프트웨어 개발 방법론

#### 1. 소프트웨어 개발 방법론
1. 소프트웨어 생명주기 모델
   - 개념 : 소프트웨어 생명주기는 시스템의 요구분석부터 유지보수까지 전 공정을 체계화한 절차


2. 소프트웨어 생명주기 모델 프로세스
   - **요구사항 분석** : 개발할 소프트웨어의 기능과 제약 조건, 목표 등을 소프트웨어 사용자와 함께 명확히 정의하는 단계
     - 기능 요구사항 / 비기능 요구사항
   - **설계** : 시스템 명세 단계에서 정의한 기능을 실제 수행할 수 있도록 수행방법을 논리적으로 결정하는 단계
     - 시스템 구조 설계 / 프로그램 설계 / UI 설계
   - **구현** : 설계 단계에서 논리적으로 결정한 문제 해결 방법을 특정 프로그래밍 언어를 사용하여 실제 프로그램을 작성하는 단계
     - 인터페이스 개발 / 자료 구조 개발 / 오류 처리
   - **테스트** : 시스템이 정해진 요구를 만족하는지, 예상과 실제 결과가 어떤 차이를 보이는지 검사하고 평가하는 단계
     - 단위 / 통합 / 시스템 / 인수 테스트
   - **유지보수** : 시스템이 인수되고 설치된 후 일어나는 모든 행동
     - 예방 / 완전 / 교정 / 적응 유지보수
            

3. 소프트웨어 생명주기 모델 종류
   - **폭포수 모델 (WaterFall)** : 개발 시 각 단계를 확실히 마무리 후 다음 단계로 넘어감 
     - 가장 오래됨 / 선형 / 고전적 / 명확 / 요구사항 변경 어려움
     - 절차 : 검토 → 계획 → 분석 → 설계 → 구현 → 테스트 → 유지보수
   - **프로토타이핑 모델** : 주요 기능을 프로토타입으로 구현, 피드백을 반영하여 개발
     - 발주자, 개발자 모두에게 공동의 참조 모델을 제공 / 구현 단계의 구현 골격
   - **나선형 모델** : 시스템 개발 시 위험을 최소화하기 위해 점진적으로 개발
     - 절차 : 계획 및 정의 → 위험 분석 → 개발 → 고객 평가
