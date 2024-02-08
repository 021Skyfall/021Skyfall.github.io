---
title: "정보처리기사 / 22.03 필기 모의고사 오답노트"
author: Jeremiah Lee
date: 2024-02-07
categories: [ 정보처리기사, 필기 오답노트 ]
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

## 2022 3월 기출문제

<br>

### 1과목 : 소프트웨어 설계

<br>

#### 19.	입력되는 데이터를 컴퓨터의 프로세서가 처리하기 전에 미리 처리하여 프로세서가 처리하는 시간을 줄여주는 프로그램이나 하드웨어를 말하는 것은?

     1.	EAI
     2.	FEP
     3. GPL
     4. Duplexing

##### 정답 : 2
- EAI (Enterprise Application Integration) : 기업 응용 프로그램 통합으로 기업용 응용 프로그램의 구조적 통합 방안을 가리킴
- FEP (Front-End Processor) : 입력되는 데이터를 컴퓨터의 프로세스가 처리하기 전에 미리 처리하여 프로세서가 차지하는 시간을 줄여주는 프로그램이나 하드웨어
- GPL (General Public License) : 자유 소프트웨어 재단(OSF)에서 만든 자유 소프트웨어 라이센스
- Duplexing : 이중통신 또는 쌍방향 통신, 두 지점 사이에서 정보를 주고 받는 전자 통신 시스템

<br>
<br>
<br>

***

### 4과목 : 프로그래밍 언어 활용

<br>

#### 76.	다음 C언어 프로그램이 실행되었을 때, 실행 결과는?

```C
#include <stdio.h>
#include <stdiib.h>
int main(int argc, char *argv[]) {
    char str1[20] = "KOREA";
    char str2[20] = "LOVE";
    char* p1 = NULL;
    char* p2 = NULL;
    p1 = str1;
    p2 = str2;
    str1[1] = p2[2];
    str2[3] = p1[4];
    strcat(str1, str2);
    printf("%c", *(p1+2));
    return 0;
```

     1.	E
     2.	V
     3.	R
     4.	O

##### 정답 : 3
- str[1] = str2[2] => str1의 KOREA 중 O가 V로 변경
- str1 = KVREA
- str2[3] = str1[4] => str2의 LOVE 중 E가 A로 변경
- str2 = LOVA
- strcat(str1, str2) => str = "KVREALOVA"
- p1 + 2 = str1[2] = R

<br>
<br>
<br>

***

### 5과목 : 정보시스템 구축 관리

<br>

#### 100.소프트웨어 개발 방법론의 테일러링(Tailoring)과 관련한 설명으로 틀린 것은?

     1.	프로젝트 수행 시 예상되는 변화를 배제하고 신속히 진행하여야 한다.
     2.	프로젝트에 최적화된 개발 방법론을 적용하기 위해 절차, 산출물 등을 적절히 변경하는 활동이다.
     3.	관리 측면에서의 목적 중 하나는 최단기간에 안정적인 프로젝트 진행을 위한 사전 위험을 식별하고 제거하는 것이다.
     4.	기술적 측면에서의 목적 중 하나는 프로젝트에 최적화된 기술 요소를 도입하여 프로젝트 특성에 맞는 최적의 기법과 도구를 사용하는 것이다.

##### 정답 : 1
- 테일러링(Tailoring) : 프로젝트 상황 특성에 맞게 정의된 소프트웨어 개발 방법론 절차, 사용기법 등을 수정 및 보완하는 작업

<br>
<br>
<br>

***

여러 번 풀고 나서 또 틀린 문제를 기록한 것이기 때문에 추후 재풀이 시 늘어날 수 있음
