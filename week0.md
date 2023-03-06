# 0주차
- Gitbook training
- 개발환경 설정 완료

## Git & Github
앞으로 깃과 깃허브를 사용할 때 참고할 내용들을 알아보자.

### Git 기본 명령어들.
#### git init
- git으로 해당 폴더를 관리하게 하는 명령어.
- .git 폴더가 있는 프로젝트를 관리함.
- .gitignore -> 보안상 민감한 내용들, 업로드 필요 없는 파일들을 명시하면 git이 더 이상 관리하지 않는다.
#### git add
- 변경 사항을 스테이징 영역으로 이동시킨다.
#### git commit 
- 스테이징 영역에 저장된 변경 사항들을 로컬 저장소에 적용시킨다.
- 작업 영역과 스테이징 영역을 구분함으로써 변경 내용을 나누어 별개로 기록할 수 있음.
- commit 기준으로 버전이 생김.
#### git push
- 로컬 저장소의 내용을 원격 저장소에 적용시킨다.
#### git log
- 소스트리 확인
#### git reset vs git revert
- 전자는 중간 커밋 내용들을 무시한 채 해당 시점으로 복구
- 후자는 중간 커밋 내용 하나만을 무효화

### Branch?
- 한 프로젝트의 신기능을 실험하거나, 여러 용도로 프로젝트를 수정할 수 있게 분기처리를 하는 것.
#### 브랜치 생성/이동/삭제
- git branch add-coach(생성) -> git branch(브랜치 목록 확인) -> git switch <branchname>(브랜치 변경)
#### git branch --all
- 브랜치들 확인.

#### merge & rebase
- merge : 두 브랜치를 한 커밋에 이어 붙인다.
    - 합병을 주도하는 브랜치에서 git merge <합병당할 브랜치> 작성.
    - reset 명령어로 복구 가능.
- rebase : 브랜치를 다른 브랜치에 이어 붙인다.(브랜치 베이스 재설정)

### Github
### git remote add origin (원격 저장소 주소)
- 로컬 git 저장소 <-> 원격 저장소 연결
### git branch -M main
- branch 이름 설정.
### git push -u <브랜치 이름> 
- 로컬 저장소 내역들을 원격 저장소에 업로드

### git clone(원격 저장소 주소)
- 원격 저장소를 로컬에 복제.