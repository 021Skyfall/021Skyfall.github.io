---
title: "GUi & CLi 와 리눅스 명령어"
author: Jeremiah Lee
date: 2022-12-16
categories: [ 리눅스 ]
tags: [리눅스, 부트캠프]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Academic_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

### **GUI / CLI**
- GUI : Graphical User Interface
- CLI : Command-Line Interface

**리눅스** : 오픈소스 CLI로 무료에 그래픽적인 요소가 없어 IO 처리가 빠르다

### **Ubuntu 자주 쓰는 명령어**
- pwd : 현재 위치 확인 (print working directory)
- mkdir : 폴더 생성
- rmdir : 폴더 삭제 (내용물 없는)
- ls : 폴더나 파일의 목록 출력
- open (MacOs) : 현재 폴더를 파일 탐색기로 염
- cd : 폴더에 진입
- touch : 새로운 파일을 생성
- cat : 파일의 내용을 터미널에 출력
- rm : 파일을 삭제 (( -rf 조심해서 쓰자 / -r 은 내용물도 지워줌)) => recursive / force
- mv : 폴더나 파일의 위치 이동 or 이름 변경 // mv 파일or폴더 위치/이름
- cp : 폴더나 파일 복사 // cp 파일or폴더 경로
- sudo : 관리자 권한 일시 획득

```
경로 : . -> 현재 폴더
.. -> 이전 폴더
/ -> 폴더 내부
```


### **MacOS brew 명령어**
- brew update: 패키지의 업데이트 여부 확인
- brew outdated: 업데이트 필요한 파일 조회
- brew upgrade: 프로그램 업그레이드
- brew info: 프로그램의 정보 확인
- brew install: 프로그램 설치
- brew list: 설치된 프로그램 목록 보기
- brew uninstall: 프로그램 삭제

### **Linux Ubuntu apt 명령어**
- apt update: 패키지의 업데이트 여부 확인
- apt list --upgradable: 업데이트 필요한 파일 조회
- apt upgrade: 프로그램 업그레이드
- apt show: 프로그램의 정보 확인
- apt install: 프로그램 설치
- apt list --installed: 설치된 프로그램 목록 보기
- apt remove: 프로그램 삭제
- apt search : 패키지 검색


### **권한**
- r : read permission
- w : write permission
- x : execute permission
<span style="color:red">rwx</span><span style="color:green">rwx</span><span style="color:blue">rwx</span>   
  <span style="color:red">사용자</span>/<span style="color:green">그룹</span>/<span style="color:blue">나머지</span>   
- 맨 앞에 d = directory // - = folder
- chmod : 권한 변경 // chmod 변경값 파일or폴더

```
Symbolic method
Access class Operator Access Type
u (user) + (add access) r (read)
g (group) - (remove access) w (write)
o (other) = (set exact access) x (execute)
a (all: u, g, o)
```

```
Absolute form
7 read, write, execute
6 read, write
5 read and execute
4 read only
3 write, execute
2 write only
1 execute only
0 none
```


### **환경 변수**
- 환경에 따라 프로그램의 동작에 영향을 줄 수 있는 값
- 즉, A 프로그램 실행 시에 필요한 B 프로그램을 불러오는 경로를 지정한 것

### **임시 적용**
- 지역 : ~=~ (공백 있을 시 "")
- 전역 : export ~=~ (공백 있을 시 "")
- 값 확인 : echo $~

### **영구 적용**
- vim으로 bashrc(리눅스) && zshrc(맥) 편집해서 넣기 => 명령어는 임시랑 똑같음
- 아 근데 전역은 sudo로 관리자권한 얻어야함

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/222960086719)에서 옮겨짐
