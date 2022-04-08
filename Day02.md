# Day02_2022-04-08

## Git 특강 2일차 정리

### 1. Git의 특수한 파일

 	1. README.md
     - 대문역할을 한다.
 	2. .gitignore
     - 숨기고 싶은 파일 저장한다.
     - 이미 git init 파일은 .gitignore할 수 없다.

> 이 두개의 파일은 프로젝트 과제 전 가장 먼저 만들기!

### 2. Git에서 버전 가져오기 (Clone, Pull)

- 내 컴퓨터에 파일이 없거나 동료와 협업을 할 때 Git에서 버전을 가져올 수 있다.

1. Clone

   - 실행하는 방법

     - 원하는 폴더 - git bash here - git clone 주소

   - 내 컴퓨터에 버전이 없어 새로 만들어야하는 경우에 사용하는 명령어이다.

   - 다운로드와 같다.

   - git clone을 하게되면 아래와 같은 작업이 실행된다.

     1. 폴더를 만든다.
     2. git init
     3. commit 받아오기
     4. git remote add

     > clone을 한 다음 init을 해서 중첩되지 않게 주의하기!

2. Pull

   - 실행하는 방법

     - 터미널 - git pull origin master

   - 내 컴퓨터에 버전이 있어서 추가된 버전만 가져올 경우에 사용하는 명령어이다.

   - 업데이트와 같다.

     >  다른 사람이 버전을 추가하였는데 pull을 하지 않고 내가 추가할 경우 충돌이 나기 때문에 추가하기 전에 pull하기!

### 3. 동시에 작업하기 (branch, merge)

- 협업을 하다보면 동시에 작업할 일이 많은데, pull을 이용하면 충돌이 날 수 있기 때문에 branch와 merge를 이용한다.

1. Branch
   - 실행하는 방법
     1. 브랜치 만들기
        - git branch 이름
     2. 브랜치 목록 조회
        - git branch
     3. 브랜치 삭제
        - git branch -D 이름
     4. 브랜치 바꾸기
        - git switch 이름
   - 브랜치를 만들어 동시에 작업할 다른 공간을 만든다.
2. Merge
   - 실행하는 방법
     - git merge 이름
   - merge를 실행해 추가한 버전을 합친다.
   - merge의 종류
     1. Fast-Forword
        - 빨리 감기 - 앞서가는 버전을 따라간다.
     2. Auto-merging
        - 자동 - 벌어진 버전을 자연스럽게 합체한다.
     3. conflict
        - 충돌 - 벌어진 버전을 부자연스럽게 합체한다.

### 4. 다른 사람의 프로젝트 수정하기 (Pull Request, Fork)

- 회사의 프로젝트 같은 경우 소유권이 없어 수정할 수 없기 때문에 pull request를 이용해 수정을 요청한다.
- 실행하는 방법
  1. 회사의 깃허브에서 프로젝트를 내 깃허브로 fork(복제) 한다.
  2. 내 깃허브의 버전을 내 로컬저장소로 clone한다.
  3. 내 pc에서 작업한다.
  4. 저장한 버전을 내 깃허브로 push한다.
  5. pull request로 회사에 내 버전 반영을 요청한다.

