---
title: "Git 과 GitHub 그리고 Git 명령어"
author: Jeremiah Lee
date: 2022-12-20
categories: [ Git/GitHub ]
tags: [Git/GitHub, 부트캠프]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Academic_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

**Git 은 로컬에서 버전을 관리해주는 프로그램**   
**Github 는 Git이 설치되어 있는 클라우드 저장소**

### **Git**
- 분산형 버전 관리 시스템
- 버전 관리 / 백업 / 협업
- 날짜 별로 파일 수정본을 스냅샷 형태로 백업 해줌 << 커밋을 의미

### **Github**
- Git Repository 클라우드 기반 서비스 > 오픈 소스 협업 가능
- Local Repository : 로컬 작업
- Remote Repository : 작업 공유

### **협업 과정**
- 다른 유저 리모트 리포지터리에서 작업물을 내 리모트 리포지터리로 복사해옴 (FORK) => 해당 작업물을 내 로컬 리포지터리로 다운로드 (CLONE) => 작업물 수정 작업 후 커밋 대기실로 올림 (스테이징 영역) => 작업 완전 종료 후 작업물들을 버전으로 백업 (COMMIT) => 커밋된 작업물을 내 리모트 리포지터리 혹은 협업 상대 지포지터리로 올림 (PUSH) => 상대방에게 완성된 작업물을 확인, 취합, 수정 해달라고 요구함 (PULL REQUEST (PR)) => 상대방이 해당 작업물을 수정하기 위해 자신의 로컬 리포지터리로 다운로드 (PULL) => 클론 작업 이후 부터 반복~

### **개인 작업**
- 내 github에 리포지터리 생성 => 작업물을 git 관리 하에 두도록 함 => 작업 후 작업물을 스테이징함 => 작업물을 커밋해서 버전 백업 => push로 깃헙 리포지터리 주소로 업로드 => 이후 작업 시 pull로 다운로드 후 스테이징부터 반복~

### **과정 중 파일의 상태**
- Untracked -> 커밋 후 -> Tracked - Unmodified : 기존에 커밋된 파일이 수정 안된 상태
- Modified : 기존에 커밋된 파일이 수정된 상태
- Staged : 파일이 Staging area에 존재
+ 깃헙 페이지 기능 중 Compare & pull request 버튼 클릭하면 내가 push한 내용 파악 쉬움 & 리뷰 & 취합 가능


### **터미널 Git 명령어**
- git config --global init.defaultBranch <변경할 브랜치 이름> : 디폴트 브랜치 이름 변경
- git branch -m <변경할 브랜치 이름> : 현재 디렉토리 브랜치 이름 변경
- git init : git으로 해당 폴더 안 관리 시작
- git add <파일 이름 혹은 디렉터리> : Staging area로 이동
- git rm --cached <파일> : 스테이징된 것을 다시 WorkSpace로 이동
- git status : 파일 상태 확인
- git restore <파일> : 커밋 안된 로컬의 변경 사항 취소
- git commit -m <지정 이름> : 지정 이름으로 커밋
- git reset HEAD^ : 아직 리모트에 올리지 않은 커밋 취소
- git log : 커밋 내역
- git remote -v : 현재 연결된 리모트 주소 확인
- git remote add <별칭> <원격 저장소 URL> : 해당 URL로 리모트 연결
- git push <별칭> <현재 브랜치 이름> : 현재 디렉토리 내 해당 브랜치 이름으로 설정된 작업물을 연결된 리모트 주소로 전송

### **충돌시 선택지**
- git rebase --continue : 취합 후 수동 수정 // 메뉴얼로 하게되면 구분 지우고 넣어주면됨
- git rebase --skip : 상대방 걸로 대체
- git rebase --abort : 이전 걸로 대체


참고

![](/assets/img/bootcamp/2022_12_(boot)Git_Github.png)

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/222961154405)에서 옮겨짐
