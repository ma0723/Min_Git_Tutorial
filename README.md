# Git



## Index

- [Git push](#git-push)   
- [Git Branch](#git-branch)   
- [Git Clone](#git-clone)   



## git push



### 1. 명령어

````bash
git init
# 초기설정

git force

git add .
# stage 추가

git commit -m "message"
# commit 명령

git commit --amend
# commit 수정

git remote origin https
# 원격저장소 등록

git push origin <branch name>
# push master branch

git log --oneline
# 깃 명령어 로그 한줄로 조회

git push --force <branch name>
# 강제로 이름 바꾸고 push
````



### 2. 로컬 저장소 및 원격 저장소 Conflict

#### (1) git pull

- 로컬과 원격 저장소(웹IDE) 동일한 수정 및 동일한 명령을 내리는 경우에는 내용은 같더라도 commit의 history가 다르다
- git pull 이후 다시 push
- Current Change (로컬)
- Incoming Change (원격에서 로컬로)
- 방법
  - Accept Current Change
  - Accept Incoming Change
  - Accept Both Change
  - Compare Change
- `(master|merging)`에서 `(master)`로 변경

`````bash
git pull origin <branch name>
# 원격저장소에 있는 내용 다운로드
`````



#### (2) git revert  

- Current Change (로컬)
- Incoming Change (원격에서 로컬로)
- 방법
  - Accept Current Change
  - Accept Incoming Change
  - Accept Both Change
  - Compare Change

- `(master|REVERTING)`

`````bash
git log --oneline
# 깃 명령어 로그 한줄로 조회

git revert b2ca1f8
# 코드만 과거 history에서 가져온다
# b2ca1f8에 해당하는 commit history는 삭제하지 않고 유지
`````



#### (3) git reset

- 권장하지 않는다 로컬과 원격의 history 충돌

`````bash
git log --oneline
# 깃 명령어 로그 한줄로 조회

git reset b2ca1f8
# b2ca1f8에 해당하는 commit 기록을 history에서 삭제
`````



## Git Branch

### 1. 명령어

`````bash
rm -rf .git
# git commit history 초기화

git init
# 초기설정

git branch
# branch 확인

git branch <branch name>
# branch 생성

git checkout <branch name>
git switch <branch name>
# branch 변경

git branch -d <branch name>
# branch 삭제
`````



### 2. 이동 Commit

#### (1) master

- 삭제 불가

#### (2) HEAD

- master branch 
  - git commit -m "update README" (선)
  - git log --oneline
  - `(master)`에서 fix README의 log는 검색 불가 (**시간적으로 미래**에 있는 brunch에서의 commit)
- change branch 
  - git commit -m "fix README" (후)
  - git log --oneline
  - `(change)`에서 fix README와 update README 모두 검색 가능

`````bash
git switch <branch name>
# branch 변경
# HEAD 변경

git pull origin <branch name>
# 원격저장소에서 로컬저장소로 다운로드
`````



### 3. Merge

#### (1) master

- main branch
- `(master)`로 전환 확인
- master branch 
  - git commit -m "update README" (선)
  - git log --oneline
  - `(master)`에서 fix README의 log는 검색 불가 (**시간적으로 미래**에 있는 brunch에서의 commit)
  - HEAD가 master
- change branch 
  - git commit -m "fix README" (후)
  - HEAD가 change
- git merge change
  - git log --oneline
  - fix README의 HEAD가 master, change

`````bash
git switch master
# master brunch로 이동

git merge <branch name>
# Fast-forward
`````



## Git Clone



### 1. 원격 clone branch 조회

`````bash
git branch --remote
git branch --r
# 원격저장소 branch 검색
# origin/HEAD -> origin/master
# origin/develop
# origin/master
`````



### 2. 로컬 branch 설정

`````bash
git branch develop origin/develop
# origin/develop의 branch를 develop이라는 branch로 설정
git branch
#   develop
# * master
git switch develop
# (develop)
git switch master
# (master)
`````



### 3. branch 생성 후 다른 brunch에 통합

`````bash
git branch nari

git branch
#   develop
# * master
#   nari

git switch nari
# Switched to branch 'nari'
# (nari) 

git add .
git commit -m "nari commit"
git push origin nari
# origin <branch name>

git log --oneline
# commit history 조회
# 968f0a8 (HEAD -> nari) nari commit
# b8ef097 (origin/master, origin/HEAD, master) Update README.md
# 3903b7b Initial commit
`````



### 4. Merge Request 

#### (1) Merge Request Branch 설정

- `Create Merge Request` 버튼 클릭
- `Change Branches` 밑줄 클릭 후 develop branch로 변경
- Title Description

#### (2) Merge Request 설정

- develop으로 Merge Request
- `Assignee` 본인으로 설정
- `Reviewer` 타인으로 설정

#### (3) Merge 전 변경 

- **commit을 다시 push**하면 **nari branch**에 하면 Merge Request에 **자동으로 반영되어** 들어간다

#### (4) Merge 후 pull

`````bash
git switch develop
# develop branch로 전환
git pull origin develop
# develop branch의 merge된 내용을 원격에서 가져온다
git rebase develop
# 내 브런치와 병합하는 과정
`````



### 5. branch들 통합

`````bash
git add .
git commit -m "commit"
git push origin <branch name>
# 모두 완성된 경우

git stash
# 작업중인 경우

git switch develop
# develop branch로 전환
git pull origin develop
# develop branch의 merge된 내용을 원격에서 가져온다

git rebase develop
# 내 브런치와 병합하는 과정
git rebase --continue
# 각 commit마다 충돌하는 과정 해결하기 위해

git stash apply
# 작업중인 경우 git stash를 했다면 모든 충돌 해결 후 적용
`````

