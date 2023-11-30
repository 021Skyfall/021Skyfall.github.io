---
title: "프로젝트:샐로그 / 문서"
author: Jeremiah Lee
date: 2023-11-30
categories: [ 프로젝트, 샐로그 ]
tags: [프로젝트, 샐로그, 개발 문서]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Project_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

샐로그 프로젝트의 문서를 정리할 것이다.

그닥 긴 내용은 없고, 단순히 기록용이기 때문에 사진과 pdf파일을 첨부하고 문서 툴로 사용하는 구글 시트 링크를 건다.

이 기록을 바탕으로 내 블로그를 보는 누군가가 참고를 할 수도 있을 것이고, 주로 내가 보려고 작성하는 블로그이기 때문에 무엇이 되었던 도움이 될 것이다.

<br>

[문서 구글 시트 링크](https://docs.google.com/spreadsheets/d/1_bI9UAymfg1Typ5DVZzbXZe_OQ08q-AZO9Ve1Ph1SL8/edit?usp=sharing)

<br>
<br>
<br>

### **사용자 요구사항 정의서**

![](/assets/img/project_pdfs/salog/salog-userrequirment_sheet1.png)

![](/assets/img/project_pdfs/salog/salog-userrequirment_sheet2.png)

**pdf** : [사용자 요구사항 정의서](/assets/img/project_pdfs/salog/salog-userrequirment_sheet.pdf)

사용자 요구사항 정의서이다.

여기서는 기본적인 기능뿐 아니라 추가적으로 생각나는 기능과 비기능들을 적어 넣었고,
구분을 우선순위로 표현했다.

우선순위가 높을 수록 기본 기능이고, 낮을 수록 추후 추가할 내용이다.

마지막 월별 예산 조회 페이지의 경우는 당장 내가 떠올린 아이디어를 적어놓은 것이기 때문에 변경될 여지가 있다.

wrieating 진행 당시에는 이 요구사항 정의서를 너무 간단하게 작성한 것 같아서, 상세 설명에 최대한, 누구든 이해할 수 있게 적었다.

특히 무엇에 대한 기능인지, 무엇이 필요할 것인지 등을 조금 더 적었다.

요구사항 ID의 경우는 각각 membership, financial ledger, diary 를 줄인 것이다.

또한, 에자일하게 프로젝트가 진행되기 때문에 내용이 수정, 추가가 될 가능성이 높다.

<br>
<br>
<br>

### **API 명세서**

![](/assets/img/project_pdfs/salog/salog-API_sheet1.png)

![](/assets/img/project_pdfs/salog/salog-API_sheet2.png)

**pdf** : [API 명세서](/assets/img/project_pdfs/salog/salog-API_sheet.pdf)

API 명세서인데, 이 명세서는 수시로 변경될 것이고 결국에는 개발 도중, 이후 문서화를 할 것이기 때문에 완성본은 아니다.

자세히 보면 프로퍼티 값이 이상하다던가, 불필요한 요소가 있다던가 할 수 있는데

이 명세서 작성 목적 자체가 프론트 팀원에게 "대충 이런 식으로 데이터가 오갈 것이다"를 알리기 위함이기 때문에 완벽하진 않다.

특히 에러 코드 부분을 보면 커스터마이징을 거쳐 조금 더 상세하게, 친절하게 변경될 예정이다.

이 부분에 대해 wrieating 프로젝트 진행 당시 피드백을 얻었기 때문이다.

<br>
<br>
<br>

### **테이블 명세서**

![](/assets/img/project_pdfs/salog/salog-table_sheet1.png)

![](/assets/img/project_pdfs/salog/salog-table_sheet2.png)

**pdf** : [테이블 명세서](/assets/img/project_pdfs/salog/salog-table_sheet.pdf)

가장 중요한 테이블 명세서이다.

사실 상 백엔드 개발에서 제일 핵심이 되는 테이블 명세서이다.

각 테이블에서 기존에는 고정 수입/지출과 태그에 대한 테이블 없이 진행하려고 했었는데,
회의를 거친 결과 회원 마다 태크가 저장되어 리턴될 수 있게 끔 해주어야하고, 자주 조회되어야 하는 만큼 테이블을 따로 두었다.

또한 고정 수입/지출은 처음 구상한 내용이 기본 수입/지출 테이블에 fixed라는 플래그를 두고서 true/false로만 판단하고자 했는데,
사용자에게 고정 수입/지출에 대한 자동 작성 기능을 제공하기 위해 구성하였다.

이 경우에서 데이터에 대한 구분이 힘들어서 나눈 것이라 할 수 있는데, 만약 사용자가 한 달안에 고정된 수입/지출에 대한 내용을 변경한다면
중복 작성될 가능성이 있기 때문이다.

해당 문제를 구분하고자 고정 지출/수입에 대한 테이블을 나누게 되었고, 이를 통해 날짜가 되면 작성되게끔하고,
회원에게 작성할 것인지를 묻는 알림을 띄울것이다.

<br>
<br>
<br>

### **코드 컨벤션**

![](/assets/img/project_pdfs/salog/salog-codeconvention_sheet.png)

**pdf** : [코드 컨벤션](/assets/img/project_pdfs/salog/salog-codeconvention_sheet.pdf)

나는 개발 시 가독성을 굉장히 중요하게 생각하는데, 이 코드 컨벤션 문서를 작성해 해당 부분을 굉장히 많이 개선할 수 있을 것이라 생각한다.

특히 사람이 많을 수록 커밋 내용, 혹은 브랜치, pr 내용 등 일관성 있게 작성해주어야 가독성이 높아질 것이고,
이를 달성하기 위해서는 회의를 거친 코드 컨벤션이 필수적이라고 생각한다.

기본적으로 깃 워크플로우 전략은 3개의 브랜치로 나뉘고,
각각 배포 main, 병합 dev, 개발 feat으로 구성되어 있다.

복잡하지 않은 만큼 실수가 적을 것이다.

또한 커밋과 pr시 어떻게 작성해야하는지 적어놓았기 때문에 일관성이 있어 가독성이 높아질 것이다.

<br>
<br>
<br>

***

직접 작성한 문서들은 위가 전부이다.

앞서 말했듯이 에자일하게 진행하는 만큼 추후 변경될 여지가 많고, 업데이트를 통한 문서 수정이 있을 수 있다.

이 기록은 문서의 구조와 필수 요소 등을 파악하기 위해 기록한다.
