# 2022-04-07

##  Git 특강 1일차 정리

### 1. 버전 관리 방법

1. 파일의 변경사항을 기록한다.

		\- 어떤 부분을 수정하였는지 변경사항을 기록하여 따로 저장한다.

2. 파일의 용량을 줄인다.

   \- 파일의 용량이 너무 큰 경우 최종 수정된 파일과 각 파일의 변경사항만 두고 지운다.

   > 이런 작업을 버전 관리 프로그램(Git)이 해준다.

### 2. 버전 관리 프로그램을 사용하는 이유

- 백업
- 복구
- 협업

### 3. Git을 시작하기 전 필요한 사전지식

- CLI
- VSCode
- Mark down

### 4. CLI / GUI

- CLI(Command Line Interface) - Git bash here을 눌러 실행시킨 터미널이다.
- GUI(Graphic User Interface) - 우리가 평소에 보는 폴더이다.

### 5. Mark down

- Mark up - 글에 역할을 부여하는 것이다. ex) <제목> Git 특강 <제목>
- Mark down - 마크업에 포함되는 경량 마크업이다.
- Typora를 이용해 마크다운을 할 때 주의할 사항
  - 디자인 하지 않기 - 마크다운은 역할을 부여하는 것이지 디자인이 목적이 아니다.

### 6. Git

- Git을 하는 순서
  1. 사용자가 누구인지 알려주는 명령어
     - git config --global user.name 이름
     - git config --global user.emil 이메일
     - git config --global --list (잘 입력되었는지 확인하기)
  2. Git에게 폴더 관리를 부탁하는 명령어
     - git init
  3. 파일의 현재 상태 확인하는 명령어
     - git status
  4. 파일을 Staging Area로 올리는 명령어
     - git add .
  5. 버전(commit)을 생성하는 명령어
     - git commit -m "수정하는 이유"
  6. 버전이 잘 생성되었는지 확인하는 명령어
     - git log
