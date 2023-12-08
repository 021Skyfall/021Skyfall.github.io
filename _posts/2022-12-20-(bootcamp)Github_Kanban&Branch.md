---
title: "Github 칸반과 브랜치"
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

### **GitHub Project Kanban**
- 칸반은 팀과 조직이 작업을 시각화하고, 업무의 병목 현상과 리소스 낭비를 해결하는 업무 관리 방법



### **칸반 보드를 통한 시각화**
- 칸반의 대표적인 특징은 칸반 보드를 통한 업무 시각화
- 칸반 보드는 아래 사진처럼 업무를 하나의 티켓으로 표현하고, 업무 단계를 하나의 열로 표현함
- 새로운 업무가 생기면 가장 왼쪽 열에 업무가 쌓이고, 업무가 잘 진행되면 가장 오른쪽으로 전달되어 쌓이는 방식
- 어떤 업무가 현재 어느 업무 단계에 있는지 한눈에 파악할 수 있음

![](/assets/img/bootcamp/(boot)KanBan_board.png)

-> 이렇게 한눈에 업무를 파악할 수 있게 되면   
각 팀원이 다른 팀원이 어떤 일을 하고 있는지 투명하게 확인할 수 있고,   
문서 파일에 쌓여있는 업무 현황보다 훨씬 더 종합적이고 빠르게 업무 흐름을 파악할 수 있음



### **Work In Progress(WIP)로 진행중인 업무 제한 및 흐름 관리**
- WIP는 현재 진행하고 있는 작업을 의미 (Work In Progress)
- 칸반에서는 각 업무 단계에 WIP 제한(WIP limit)을 둘 수 있음
- WIP 제한이 2이면, 두 개 이상의 카드가 해당 열에 위치할 수 없게 됨

![](/assets/img/bootcamp/(boot)KanBan_board2.png)



### **업무 흐름 관리**
- WIP를 두는 이유는 업무가 과도하게 쌓이지 않는 원활한 업무 흐름을 위해서
- 모든 팀원이 100%의 리소스를 사용하고 있는 상태에서 계속 새로운 업무가 쌓이게 되면, 업무가 과부하되어서 업무 효율이 나지 않게 됨



### **진행중인 업무 제한**
- 특히 개발 프로젝트는 타 업무와 달리 맥락 전환(context switching)이 없이 집중할 수 있어야 업무 효율이 증가함
- 즉, WIP 제한은 한 번에 처리하는 업무의 양을 최소화하여 팀원이 한 번에 여러 업무를 동시에 진행해서 생기는 맥락 전환의 문제를 방지할 수 있고, 업무 흐름을 적당하게 유지시켜 업무가 차근차근 처리될 수 있도록 함


### **명확한 팀 정책 설정**
- 칸반을 잘 활용하기 위해서는 정책을 설정해야 함
- 칸반의 열을 정의하고 WIP limit을 세우는 것부터, 언제 회의를 하고 어떻게 의사결정을 할지 명확한 정책을 수립해야 함
- 정책 수립 시에는 팀원이 모두 모여서 합의를 이뤄야 함
- 이렇게 합의한 정책은 향후 업무 진행 상황에 따라서 팀원들이 모여서 언제든지 다시 조정할 수 있음


### **프로젝트가 본격적으로 시작하기 전에 정하면 좋을 정책**
1. 회의 시간 및 해당 회의에서 논의할 내용
2. 팀원 간 소통 원칙
3. 칸반 티켓을 언제, 어떻게, 누가 추가할지
4. WIP 제한


### **회의와 리뷰를 통해 함께 업무를 개선**
- 보통 칸반을 사용하는 경우, 데일리 칸반 회의와 주간 보충 회의를 진행함
- 데일리 칸반 회의는 업무의 상태 및 흐름을 관찰하고 추적함
- 어떻게 하면 구현하고자 하는 기능을 좀 더 빠르게 구현할 수 있을지, 업무가 끝난 인원이 다른 업무를 당겨올 수 있는지 등을 정할 수 있음
- 주간 보충 회의에서는, 칸반 보드에 추가할 만한 업무가 있는지 확인하고, 다음 주에는 어떤 업무를 할 것인지 정할 수 있음
- 추가로 격주, 혹은 월간으로 KPT 회고를 진행할 수도 있는데, 지금까지의 진행 상황에 대해서 솔직하게 회고하고, 어떻게 하면 좀 더 잘할 수 있을지 개선점을 찾을 수도 있음


### **칸반 실천법**
1. 업무 시각화
2. 진행 중인 업무 제한
3. 흐름 관리
4. 명확한 프로세스 정책
5. 피드백 루프 구현
6. 협력적인 개선, 실험적인 발전


### **칸반 원칙**
- 칸반 실천법은 모두 칸반 원칙을 잘 지키기 위해서 만들어졌음
- 즉, 해당 구성 요소를 기계적으로 충족시키려고 하기보다는, 종종은 칸반 원칙으로 되돌아와서 지금 하고 있는 작업이 칸반 원칙에 잘 맞는지 고민을 해봐야 함

1. 하던 업무를 칸반 보드에 올려두는 것부터 시작
   - 당장 하고 있던 업무를 칸반 보드에 올려두는 것부터 시작임
   - 앞으로의 Pre-Project도 당장 할 일부터 칸반 보드에 올려두고 시각화하는 것부터 시작

2. 점진적인 변화를 추구
   - 칸반은 당장의 업무 내용이나 방향성을 갑자기 뒤엎고 화려하고 멋진 기획을 만들기 위한 수단이 아님
   - 현재 하고 있던 일이나 업무를 잘 수행하기 위한 하나의 수단
   - 칸반을 위해서 갑작스럽게 업무 방식을 극적으로 변화시키지 말아야 함

3. 직위에 관계 없이 리더십을 발휘
   - 칸반은 팀장만 관리하는 것은 아님
   - 각 팀원도 칸반을 보고 WIP 제한을 늘리거나 줄이는 것을 제안할 수 있고, 새로운 업무 티켓을 발행하거나 기존 업무 티켓의 진행을 도울 수 있음


<br>


### **튜토리얼1**
- GitHub는 본래 원격 코드 리포지토리의 역할만 담당했었음
- 많은 개발자가 모여서 협업을 하다 보니 개발 업무를 관리할 수 있는 다양한 기능이 포함되어서 이제는 간단한 개발 업무 관리는 GitHub에서도 충분히 가능하게 되었음
- 칸반과 같이 가볍게 사용할 수 있는 GitHub Issue와 GitHub Milestone

### **GitHub Issue**
- https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues
- 실제 Pre-Project, Main-Project 진행 시에는 팀원 간의 긴밀한 소통을 통해서 팀원 다수가 동의하는 태스크 생성 정책을 만들기를 권장함
- 아래는 프로토타입

1. 이슈 (task card) 생성
   - 저장소 첫 페이지에서 Issues탭을 선택하고 New issue를 클릭

2. 테스크 작성
   - 이슈 템플릿의 제목과 본문을 만들고자 하는 태스크 카드에 맞게 수정
   - 제목과 본문을 작성했다면, 우측 탭을 이용해 세부 설정을 진행
   - a. Assigness: 해당 태스크를 맡은 사람을 지정
   - / assign yourself를 누르면 자신의 태스크로 만들 수 있음
   - b. Labels: 태스크 카드에 라벨링을 할 수 있음
   - c. Projects: Projects를 지정할 수 있음
   - d. Milestone: 마일스톤을 지정할 수 있음

3. 테스크 일정 완료 후 이슈 닫기
   - 테스크 일정이 완료됐다면 해당 이슈를 닫음



### **GitHub Issue 템플릿 카드**
- 이슈 반복 생성을 편하게 할 수 있는 이슈 템플릿 카드 기능

1. Settings 클릭
2. Issues 의 Set up templates 를 클릭
   - 아래와 같이 템플릿을 만들면 향후 새롭게 이슈를 생성할 때 아래 템플릿을 그대로 사용할 수 있음

```
### 만들고자 하는 기능이 무엇인가요?
ex) Todo 생성 기능

### 해당 기능을 구현하기 위해 할 일이 무엇인가요?
1. [ ] Job1
2. [ ] Job2
3. [ ] Job3

### 예상 작업 시간
ex) 3h
```


### **GitHub Milestone**
- Bare Minimum Requirement, Advanced Challenge에 해당하는 요구사항을 마일스톤으로 만들어보고, 얼마나 되는 기간 안에 끝낼 수 있을지 한 번 고민을 먼저 해보기를 권장
- 필요하면 하나의 요구사항을 여러 개의 마일스톤으로 나눠도 되고, 여러 개의 마일스톤을 하나로 합쳐도 괜찮음
- 여러 개의 이슈들을 각각의 마일스톤으로 그룹화하는 작업

1. GitHub Milestone 생성
   - a. Issue탭 -> Milestones
   - b. Create a Milestone혹은 New milestone

2. GitHub Milestone 세부 내용 작성
   - 마일스톤의 이름을 Title 에 작성
   - Due date를 설정
   - Due date는 마일스톤의 마지막 날을 의미

3. 마일스톤 생성 확인
![](/assets/img/bootcamp/(boot)KanBan1.png)

<br>


### **튜토리얼2**

#### **Github Project Kanban**
- GitHub에서는 2022/07/27 GitHub issues를 기반으로 하는 GitHub 프로젝트를 리뉴얼했음
- 지금까지 생성한 GitHub 이슈와 마일스톤을 칸반으로 쉽게 관리할 수 있음

1. Project 생성
   - a. Projects 탭을 선택하고 Add project 를 클릭하고 빨간 박스 부분을 클릭
   - b. New project 를 클릭
   - c. 탬플릿을 고르라는 모달창이 나타나면 테이블 또는 보드를 선택한 후 Create 버튼을 클릭

2. Project 이름 및 접근 설정
   - a. 오른쪽 상단의 버튼을 눌러 Settings 를 클릭
   - b. 프로젝트의 이름과 간단한 설명을 추가하고 Save 버튼을 클릭
   - c. 프로젝트의 이름이 변경된 걸 확인
   - d. 설정 페이지 왼쪽 탭에 Manage access 를 클릭
   - e. Base Role을 Read로 -> 이거는 소속되어 있는 그룹 레포지터리에서 가능함
   - f. Admin 권한으로 같은 팀원들을 초대

3. Issue 연결하기
   - a. # 으로 자신의 리포지토리를 찾음
   - b. 리포지토리를 선택 후 이슈나 PR을 선택
      + 맨 아래 Add item으로 모든 아이템들을 추가
   - c. Add selected items 버튼을 눌러 선택한 모든 item 들을 추가
   - d. 리포지토리에 작성한 이슈들이 프로젝트의 추가된 것을 확인

4. 프로젝트 설정
   - a. 각 이슈들의 상태를 설정할 수 있음
      + 기본적으로 Todo , In Progress , Done 세가지 상태
   - b. Labels, PR, Reviewers, Repository, Milestone 등 새로운 칼럼도 넣을 수 있음
      + (우상단 + 버튼 이용)
   - c. 그룹으로 나눠서 볼 수 있음 (상단 View 이용)
   - d. 위에서 그룹을 선택하면
      + Assignees, Status, Milestone, Repository 등으로 나눌 수 있음
   - e. 칸반보드로도 볼 수 있음 (QnA 때 했던거 < 이게 편함)
   - f. 설정이 끝났다면 우측 상단에 있는 Save changes 버튼을 클릭

5. 프로젝트 적용
   - a. Projects 탭에서 Add project 를 클릭 후 만든 ToDo App 프로젝트를 클릭
      + (사실 뭐 따로 안해도 생성할 때 #으로 리포지터리 연동하면 알아서 등록됨)
  - b. 이슈 생성시 만든 Projects를 지정하면 자동으로 트래킹



<br>


### **Git flow**

#### **Git branch**
- 브랜칭(branching)은 기존 개발중인 메인 개발 코드를 그대로 복사하여 새로운 기능 개발을 메인 개발 코드를 건드리지 않고 할 수 있는 버전 관리 기법
- 처음에 GitHub Repository를 생성하면 나오는 main 브랜치에서만 작업을 하다가 새로운 기능 개발을 위해 feature 브랜치를 새로 생성하는 경우, 기존 main 브랜치에서의 작업은 유지하고 새로운 feature 브랜치에서 자유롭게 코드를 추가 및 삭제할 수 있음



#### **브랜치 생성하기 / 변경하기 (git switch)**- 
- 이 때, 새로운 브랜치로 Git이 바라보는 곳, HEAD를 변경하는 작업을 switch라고 부름
- 브랜치를 생성할 때는 생성(create)의 의미로 -c 를 붙여줘야 하고, 기존에 있는 브랜치로 옮길 때는 붙이지 않아도 됨

```
# feature라는 브랜치를 새로 생성하는 경우, -c를 붙입니다.
git switch -c feature
```

```
# checkout이라는 명령어도 사용할 수 있습니다.
git checkout -b feature
```

```
# 기존에 있던 main 브랜치로 HEAD를 변경하려면, -c를 붙이지 않습니다.
git switch main
git checkout main
```

![](/assets/img/bootcamp/(boot)gitbranchex.png)


#### **브랜치 합치기 (git merge)**
- 기능 개발이 끝나면 브랜치를 main 브랜치와 합칠 수 있음

```
# 기능 개발이 진행되었습니다.
git commit -m "기능1의 세부 기능1"
git commit -m "기능1의 세부 기능2"
git commit -m "기능1 개발 완료"
```

```
# 머지를 위해 main 브랜치로 전환
git switch main
```

```
# main 브랜치로 feat 브랜치를 병합
git merge feat
```

- 실제 프로젝트 개발 시에는 브랜치를 로컬에서 합치기 보다는 GitHub의 pull request 기능을 이용하여 변경 내역을 충분히 확인하고 난 다음에 머지하는 경우가 더 많기 때문에, 로컬에서 머지하지 않고 feature 브랜치를 push하여 pull request를 요청하는 것을 권장

```
# 기능 개발이 진행되었습니다.
git commit -m "기능1의 세부 기능1"
git commit -m "기능1의 세부 기능2"
git commit -m "기능1 개발 완료"
```

```
# GitHub 리포지토리로 푸시합니다.
git push origin feat
```

```
# GitHub에서 Pull Request를 합니다.
```


#### **브랜치 삭제하기 (git branch -d)**
- merge된 feature 브랜치는 이미 dev 브랜치에 기록이 완벽하게 남아있기 때문에 굳이 남겨둘 이유가 없어 삭제를 권장
- 원격 리포지토리에서 pull request가 성공적으로 마무리되면, 아래 스크린샷처럼 브랜치를 삭제하는 버튼을 눌러 쉽게 삭제할 수 있음
![](/assets/img/bootcamp/(boot)gitbranchex2.png)

- 로컬 리포지토리에서 브랜치 삭제는 git branch -d <브랜치명> 으로

```
git branch -d feat/todo
```

- Git은 원활한 버전 관리를 위해서, 브랜치가 합쳐지지 않으면 삭제하지 못하도록 설정이 되어있지만 종종 다 만들지 못한 기능의 기록을 삭제하고 싶을 수 있음, 이 때 -D 옵션을 쓰면 삭제할 수 있음

```
git branch -D feat/todo
```


!
다만, 머지되지 않은 브랜치 삭제는 버전 기록 시스템의 사용 목적과는 잘 맞지는 않음   
잘 못 만들었던 기능이지만, 해당 기능으로 돌아가고 싶을 수도 있기 때문에   
돌아갈 여지를 만들어두는게 좋을 수도 있음   

이런 경우는 팀 및 회사 정책에 따를 것





### **Git branch 예제**

#### **Git 설정**
- 로컬 리포지토리와 연결할 유저 정보를 설정

```
# 버전 히스토리를 식별할 때 사용할 이름을 설정합니다.
$ git config --global user.name "[firstname lastname]"
# 각 기록과 연결할 이메일 주소를 설정합니다.
$ git config --global user.email “[valid-email]”
```

- 도움말 보기
- help 명령어를 이용하여 각 명령어 및 옵셥의 기능에 대해 살펴볼 수 있음

```
# Git에서 제공하는 모든 명령어를 볼 수 있습니다.
$ git help -all
# 특정 command에서 사용할 수 있는 모든 옵션을 볼 수 있습니다.
$ git [command] -help
```

- 세팅 및 초기화
- 리포지토리를 초가화하거나 존재하는 리포지토리를 클론할 수 있음

```
# 현재 디렉토리를 기준으로 Git 저장소가 생성됩니다.
$ git init
# URL을 통해 리모트 리포지토리를 로컬 리포지토리에 복제합니다.
$ git clone [url]
```

#### **Stage & Commit**
- 스테이지 영역을 이용하여 커밋할 수 있음

```
# 다음 커밋을 위해 현재 디렉토리에서 수정된 파일을 확인할 수 있습니다.
$ git status
# 다음 커밋을 위해 파일을 추가(스테이지)합니다. (stage)
$ git add [file]
# 추가한 파일을 언스테이징합니다. 변경 사항은 유지됩니다.
$ git reset [file]
# 스테이지되지 않은 변경 사항을 보여줍니다.
$ git diff
# 스테이지했지만 커밋하지 않은 변경 사항을 보여줍니다.
$ git diff --staged
# 스테이지된 콘텐츠를 메시지와 함께 커밋합니다. (스냅샷 생성)
$ git commit -m “[descriptive message]”
```

#### **Branch & Merge**
- 작업 내역을 브랜치로 분리해 컨텍스트를 변경, 통합할 수 있음

```
# 브랜치 목록을 보여줍니다. * 표시로 현재 작업중인 브랜치를 확인할 수 있습니다.
$ git branch
# 현재 커밋에서 새로운 브랜치를 생성합니다.
$ git branch [branch-name]
# 다른 브랜치로 전환합니다.
$ git switch [branch-name]
$ git checkout [branch-name]
# 새로은 브랜치를 생성하고 해당 브랜치로 전환합니다.
$ git switch -c [branch-name]
$ git checkout -b [branch-name]
# 현재 브랜치에 특정 브랜치의 히스토리를 병합합니다.
$ git merge [branch-name]
# 현재 브랜치의 모든 커밋 히스토리를 보여줍니다.
$ git log
```

#### **비교 및 검사**
- 로그 및 변경 사항을 검사할 수 있음

```
# 브랜치B에 없는 브랜치A의 모든 커밋 히스토리를 보여줍니다.
$ git log branchB..branchA
# 해당 파일의 변경 사항이 담긴 모든 커밋을 표시합니다. (파일 이름 변경도 표시)
$ git log --follow [file]
# 브랜치A에 있지만 브랜치B에 없는 것의 변경 내용(diff)을 표시합니다. (branch간 상태 비교)
$ git diff branchB...branchA
```

#### **공유 및 업데이트**
- 특정 리포지토리의 업데이트 사항을 검색하여 로컬 리포지토리를 업데이트할 수 있음

```
# url을 통해 특정 리모트 리포지토리를 별칭으로 추가합니다.
$ git remote add [alias] [url]
# 별칭으로 추가한 리모트 리포지토리에 있는 모든 브랜치 및 데이터를 로컬로 가져옵니다.
$ git fetch [alias]
# 리모트 브랜치를 현재 작업중인 브랜치와 병합하여 최신 상태로 만들 수 있습니다.
$ git merge [alias]/[branch]
# 로컬 브랜치의 커밋을 리모트 브랜치로 전송합니다.
$ git push [alias] [branch]
# 리모트 리포지토리의 정보를 가져와 자동으로 로컬 브랜치에 병합합니다.
$ git pull
```

#### **히스토리 수정**
- 브랜치 또는 커밋을 수정하거나 커밋 히스토리를 지울 수 있음

```
# 특정 브랜치의 분기 이후 커밋을 현재 작업중인 브랜치에 반영합니다.
$ git rebase [branch]
# 특정 커밋 전으로 돌아가며 스테이지된 변경 사항을 모두 지웁니다.
$ git reset --hard [commitish]
```

#### **임시 저장**
- 브랜치를 전환하기 위해 변경되었거나 추적중인 파일을 임시로 저장할 수 있음

```
# 수정하거나 스테이지된 변경사항을 스택에 임시 저장하고 현재 작업 내역에서 지웁니다.
$ git stash
# 스택에 임시 저장된 변경사항의 목록을 보여줍니다.
$ git stash list
# 스택에 임시 저장된 변경사항을 다시 현재 작업 내역에 적용합니다.
$ git stash apply
# 스택에 임시 저장된 변경사항을 다시 현재 작업 내역에 적용하고 스택에서 삭제합니다.
$ git stash pop
# 스택에 임시 저장된 변경사항을 삭제합니다.
$ git stash drop
```


<br>


### **프로젝트 Git flow**

#### **브랜칭 전략**
- 브랜칭 전략은 보다 효율적인 개발 프로젝트 코드 관리를 위해 브랜치의 종류를 나눠서 관리하는 전략을 말함
- Git이 널리 퍼지게 되면서 몇몇 유명한 브랜칭 전략이 생겨나게 되는데, 그 중 가장 유명한 브랜칭 전략이 Git flow



#### **Git flow**
- Git flow는 2010년 Vincent Driessen의 블로그 글로부터 시작되었고, 이후 많은 개발자의 사랑을 받아온 Git 브랜칭 전략
- Git flow는 대규모 개발 프로젝트를 제작하여 하나의 소프트웨어의 릴리즈 버전을 명확하게 나누고, 다양한 버전을 배포해야 하는 당시의 개발 환경과는 적합했지만, 빠르게 제작하고 배포하고 고객의 피드백을 받는 애자일한 개발 팀에 적용하기는 다소 복잡했음
- 주어지는 개발 현장의 상황에 맞게 Git flow를 정하는게 중요
- 당장 복잡한 Git flow가 필요 없는데, 애써서 Git flow를 복잡하게 만들 필요는 없음
- 필요하다면 Git flow를 써보는 것도 좋음

![](/assets/img/bootcamp/(boot)gitbranchstretage.png)



#### **Coz’ Git flow**
- “원조" Git flow에서 파생된 여러 Git flow가 있는데, 대표적인 Git flow는 Github flow, Gitlab flow가 있음
- 코드스테이츠가 제안하는 Git flow를 단순화한 Coz’ Git flow

![](/assets/img/bootcamp/(boot)gitflowcozpng.png)



### **핵심 브랜치**
- Coz’ Git flow는 중요 브랜치 두 개를 가지고 있음
- main 브랜치와 dev 브랜치

#### **main 브랜치**
- 사용자에게 언제든 제품으로 출시할 수 있는 브랜치
- main 브랜치는 사용자에게 언제든 배포할 수 있는 브랜치
- 회사에 따라서 master, prod, production등 다양하게 불림
- 회사 별로, 팀 별로 다를 수 있지만, 최소한의 기준을 세워볼 수 있음
1. 대표적인 기능이 완성
2. 기존 기획했던 레이아웃이나 전체적인 디자인이 얼추 완성
3. 클라이언트, 서버, 데이터베이스가 공개된 웹에서 정상적으로 통신 가능
4. 최소한의 보안이 마련됨
   - a. 브라우저에서 개발 버전에서 사용하던 secret이나 유저의 비밀번호가 노출되는가?
   - b. 유저의 기밀 정보 조회를 위해 인증 토큰, 세션이 꼭 필요한가?

#### **dev 브랜치**
- 다음 버전 배포를 위한 "개발 중!" 브랜치
- dev 브랜치는 다음 버전 배포를 위한 "개발 중!" 브랜치
- main 브랜치에서부터 브랜칭을 하는게 보통이며, 가능하면 프로젝트 팀원과 프론트엔드와 백엔드의 결과를 합쳐서 확인해볼 수 있을 정도로 준비가 되어야 함
- CI/CD 파이프라인이 잘 구축되어 있다면 dev 브랜치의 코드도 배포를 해두고 수시로 확인할 수도 있음
- main 브랜치와 dev 브랜치는 GitHub Repository에 늘 업데이트 되어있어야 하며, 팀원의 코드 리뷰를 받고 진행하는 것이 정석



### **보조 브랜치**

#### **feature 브랜치**
- feature 브랜치는 기능 개발, 리펙토링, 문서 작업, 단순 오류 수정 등 다양한 작업을 기록하기 위한 브랜치
- 분류를 세세하게 나누기를 원하는 회사에서는 refactor, fix, docs, chore와 같이 세세하게 커밋 메시지나 브랜치 명에 prefix를 달기도 함
- 아래는 feature 브랜치 이름과 커밋 메시지의 예시 (https://www.conventionalcommits.org/ko/v1.0.0/ 참고)

```
hash (브랜치 명) 커밋 메시지
2f85eea (feat/create-todo) feat: Todo 추가 기능
2ad0805 (fix/var-name) fix: 변수 네이밍 컨벤션에 맞게 변수명 변경 (ismale => isMale)
e7ce3ad (refactor) refactor: 불필요한 for 루프 삭제
```

- feature 브랜치는 보통 각 개인의 로컬 리포지토리에서 만들고 작업함
- feature 브랜치는 기능 개발을 위한 브랜치이기 때문에 2명 이상 같이 작업하는 경우가 드뭄
- 작은 기능이라도 브랜치를 새로 만들고, 자주 커밋하고, 자주 원격 GitHub Repository에 push하여 팀원들과 결과를 공유하는 것이 바람직
- 회사에 따라서 커밋 기록을 남기는 일반적인 rebase-and-merge, 기능마다 깔끔하게 커밋을 남기기를 원해서 커밋 기록을 정리하는 squash-and-merge등 다양한 머지 전략이 있음





### **튜토리얼**
- GitHub의 pull request
- dev 브랜치 작업을 모두 완료 -> 배포를 위해 master 브랜치로 Pull request를 보내고자 함

**1.** PR을 보내기 전, Upstream 의 브랜치 위치를 반드시 확인하고 Pull Request를 보내야함

![](/assets/img/bootcamp/(boot)gitflowtuto1.png)


**2.** 브랜치를 확인했다면, Create pull request 를 클릭

![](/assets/img/bootcamp/(boot)gitflowtuto2.png)


**3.** 버튼을 누르면 아래와 같이 화면이 나오게 되는데, 추가적으로 작성 할 부분이 없다면 그대로 Create pull request를 진행

![](/assets/img/bootcamp/(boot)gitflowtuto3.png)


**4.** 이제 Upstream 리포지토리 dev 브랜치에 PR된 이슈를 확인할 수 있음

![](/assets/img/bootcamp/(boot)gitflowtuto4.png)


**5.** 위 브랜치 위치 등을 확인했다면 이제 본격적으로 Merge를 진행

![](/assets/img/bootcamp/(boot)gitflowtuto5.png)


**6.** Confirm merge를 진행

![](/assets/img/bootcamp/(boot)gitflowtuto6.png)


**7.** 아래와 같이 나온다면 머지가 완료된 것

![](/assets/img/bootcamp/(boot)gitflowtuto7.png)





### **예제**
- 프로젝트에서 Git을 사용하다가 문제가 생기는 경우 대체법
- 아래 그림 참조

![](/assets/img/bootcamp/(boot)gitflowex.png)

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/223072438289)에서 옮겨짐
