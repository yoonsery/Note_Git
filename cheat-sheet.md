# 기본적인 터미널 언어

```bash
# 디렉토리 생성
mkdir 디렉토리명

# 파일 생성
touch 파일명.확장자

# 파일 삭제
rm 파일명.확장자

# 파일 강제 삭제
rm -rf 파일명
# 예시) rm -rf .git

# 디렉토리 삭제
rm -r 디렉토리명

# 파일 위치 변경 (이동)
mv from.text /logs/from.text

# 파일 이름 변경
mv from.확장자 to.확장자

# 디렉토리 복사
cp -r 복사를원하는기존디렉토리 복사한디렉토리가위치할곳

# 내용있는 파일 생성

# 예시 1) 'hello world!'라는 내용의 a.txt 가 만들어짐
echo hello world! > a.txt

# 예시 2)
echo "console.log('Hello, World');" >> index.js`

# 기존에 존재하던 파일에 내용 추가

# 에시) 'add comment'가 a.txt에 추가됨
echo add comment >> a.txt

# 파일 훑어보기
cat 파일명.확장자

# 숨겨진 파일도 볼 수 있다
ls -al

# 터미널 클리어
⌘ + K

```

# git 명령어

## Set up

### Open config to edit

```bash
git config --global -e

```

---

# 기본 명령어

## Basic

### Git init

```bash
git init  # initialise git
rm -rf .git  # delete .git

```

### Ignoring files

```bash
*.js  # 모든 js 파일을 무시

!main.js  # 모든 js 파일을 무시하라고 했어도 main.js는 track 할 것 (예외적용)

/TODO  # '현재 디렉토리'안에서 TODO 파일을 무시할 것

build/  # build 디렉토리안의 모든 파일을 무시

doc/*.txt  # doc디렉토리 바로 안에 있는 모든 txt파일을 무시, doc/server/arch.txt는 해당안됨

doc/**/*.pdf  # doc안에 있는 (자식 디렉토리 포함) 모~든 pdf파일을 무시할 것
```

### Staging files

```bash
git add *.txt  # txt로 끝나는 모든 파일을 추가

git add b.txt c.txt  # b.txt 와 c.txt를 추가

# add . 과 add * 의 차이
git add *  # 삭제된 파일과 .으로 시작하는 파일(예: .gitignore) 제외한 모든 파일 추가

git add .  # stage everything

```

### Modifying files

```bash
git rm file.txt  # file.txt를 working directory, staging area에서 삭제

git rm --cached file.txt  # staging area에서만 삭제

git clean -fd  # remove all untracked files

```

```bash
rm file.text
# rm을 이용해 파일 삭제하면 삭제된 내용은 staging area에 반영되지 않으므로 'git add file.text'을 해주고 커밋해야 함

git rm file.text
# 파일 삭제한 게 자동으로 바로 staging area에 추가됨

mv b.txt c.txt
# 파일명을 b에서 c로 바꿈, 마찬가지로 git status에 바로 포함되지 않는다

git mv d.txt f.txt  # 파일명 변경한 사항이 바로 stage area에 반영 된다
```

### Viewing the Staged / Unstaged changes 파일 비교하기

```bash
# 어떤 파일이 수정되었고 staging area에 있는지 확인할 수 있다
git status  # full status
git status -s  # 간략하게 확인

# 하지만 정확하게 어떤 내용이 수정되었는지 확인하려면 git diff

# changes in working directory를 볼 수 있다, 확인 후 q를 눌러서 종료
git diff

# changes in stage area를 볼 수 있다
git diff --staged
git diff --cached  # git diff --staged와 동일함

```

터미널로 보는 게 힘들다면
.gitconfig에 diff tool 추가해서 `$ git difftool`, `$ git difftool --staged`로
vscode에서 확인할 수 있다 (자세한 건 노션 git cheat sheet참고)

### Commit

```bash
git commit  # commit staged files
git commit -m "commit message"  # 커밋메시지와 함께 commit staged files
git commit -am "commit message"  # staging area와 working directory에 있는 모든 파일들을 커밋메시지와 함께 커밋

```

### Log, History

```bash
git log  # list of commits,  q로 빠져나오기

# 수정된 파일의 내용도 확인할 수 있음 (git diff처럼..)
git log --patch
git log -p

# 간편하게 보려면, (해시코드와 커밋메시지만 나옴)
git log --oneline
git log --oneline --reverse  # 오래된 커밋부터 순서대로 보임

```

#### Formating

<!-- 원하는 포맷형식으로 로그를 볼 수 있다 -->

```bash
git log --pretty=oneline  # 해시코드 전체가 원라인으로 보여짐
git log --pretty=format: "%h %an"  # 해시코드와 저자의 이름만 보기(누가 커밋했는지)
git log --pretty=format: "%h %an %ar %s"  # 해시코드, 저자, 커밋된 날짜, 타이틀

# 어디서부터 어떤 커밋이 특정 브랜치에 해당하는지 확인하려면
git log (--oneline) --graph  -all  # oneline은 생략가능해서 괄호해놓음

# 📌  엘리의 커스텀 프리티포맷_ 날짜 | 해시코드 | 타이틀 | 저자 | 브랜치 정보
git log --graph --all --pretty=format:'%C(yellow)[%ad]%C(reset) %C(green)[%h]%C(reset) | %C(white)%s %C(bold red){{%an}}%C(reset) %C(blue)%d%C(reset)' --date=short

# hist 로 단축어 설정
git config --global alias.hist "log --graph --all --pretty=format:'%C(yellow)[%ad]%C(reset) %C(green)[%h]%C(reset) | %C(white)%s %C(bold red){{%an}}%C(reset) %C(blue)%d%C(reset)' --date=short"

```

#### Filtering

```bash
git log -3  # show only the last 3 commits
git log oneline -4
git log --author="sery"  # 커밋 작성자가 sery인 것만 보기

git log --before="2021-05-09"
git log --after="one week ago"

git log --grep="project"  # 커밋 타이틀중에 "project"가 포함된 커밋보기
git log -S "about"  # 소스코드 컨텐츠안에서 "about"이 포함된 것 찾기 (대문자 S 유의)
git log -S "about" -p  # --patch(-p)를 이용해 변경사항의 내용을 자세히 보여줌


# History of a file

# 해당 파일별로 log를 볼 수 있다
git log file.text
git log -p file.text  # 좀 더 자세한 내용을 볼 수 있음
git log -s file.text  # 간단한 상태만 확인


# HEAD, Hash code
git log HEAD  # git log와 동일
git log HEAD~n  # git log 헤드에서 n번째 부모부터 보기 (~1은 이전, ~2는 두번째 이전 커밋부터)


# Viewing a commit
git show 해시코드  # 해당하는 커밋의 내용을 정확하게 확인할 수 있다

# 만약 해당 커밋에 여러가지 파일이 있어서 특정 파일에 관련한 내용만 보고싶으면?
git show 해시코드:file.text


# Comparing
git diff 해시1 해시2  # 해시1, 해시2에 해당하는 두가지 커밋을 비교할 수 있다
git diff 해시1 해시2 file.text  # 해당 파일의 변경사항 비교
```

### Tagging

```bash
git tag 버전  # 버전이라는 태그 생성됨
git tag v1.0.0 해시코드  # 해시코드에 해당하는 커밋에 "v1.0.0"라고 태그를 지정

# 태그에 관련된 릴리즈 정보를 포함하고 싶다면
git tag v1.0.0 해시코드 -am "Write Release note..."

# 태그에 포함된 릴리즈 메시지를 확인하려면
git show 태그이름

# 리파지토리에 있는 모든 tag들을 확인하려면
git tag

# 태그 리스트중에 특정한 문자열이 있는 것만 확인하고 싶으면 (예시_ v1.0.  | v2.*)
git tag -l "v1.0.*"

# 실수로 만든 태그를 삭제하려면
git tag -d 태그이름

git checkout 태그이름  # 해당 태그로 이동

# 태그를 checkout 하면서 새로운 branch를 만들고 싶다면  (해당태그로 이동하면서 새로운 브랜치 생성)
git checkout -b 브랜치명 태그이름


# 만든 태그를 서버에도 업로드해서 연동하고 싶다면
git push origin 태그이름

# 모든 태그를 서버와 싱크하고 싶다면
git push origin --tags

# 특정태그를 삭제하고 싶으면
git push origin --delete 해당태그명
```

---

# 브랜치의 모든 것 ᛘ

## Branch

```bash
git branch  # repository에 있는 branch들을 확인할 수 있다
git branch --all  # 원격연결이 되어있다면 서버에 있는 브랜치까지 확인가능
git branch -v  # 각 브랜치들의 최신 커밋을 확인가능

git branch new-branch  # new-branch 생성
# (뉴브랜치라는 새로운 포인터가 현재 커밋(HEAD)을 가리키고 있음, 만들어지기만 했을 뿐 현재 브랜치는 main인 상황)


# 다른 브랜치로 이동하려면?
git switch 브랜치명
git checkout 브랜치명 # checkout은 원하는 버전(브랜치명 대신 해시코드 입력)으로도 이동 가능

# 새로운 브랜치를 만들고 그 브랜치로 이동하려면? (create & switch)
git switch -C 브랜치명
git checkout -b 브랜치명


# 현재 브랜치에 merge된 브랜치들을 확인할 수 있다
git branch --merged

# merge가 되지않은 브랜치 확인가능
git branch --no-merged


# 브랜치 삭제
git branch -d 브랜치명

# github에 있는 원격브랜치에서도 삭제할 경우
git push origin --delete 브랜치명


# 브랜치 이름 변경
git branch --move name new-name  # 이름을 name 에서 new-name으로 변경

# 이름 변경한 사항을 원격에도 업데이트하려면
git push --set-upstream origin new-name


# main브랜치와 new-branch 사이에 있는 커밋들만 확인가능
git log main..new-branch
git hist main..new-branch
git diff main..new-branch  # 코드들을 보고싶을 때

```

### Merge

#### Fast-forward merge

```bash
git merge featureA  # featureA를 지금 브랜치에 merge함
git branch -d featureA  # (merge 후 필요없어진) featureA 브랜치를 삭제함

# featureA를 fast-forward 방법 말고 commit을 남기고 merge해줘
git merge --no-ff featureC  # merge 커밋메시지 적는 창이 뜨고 그 창을 닫으면 실행됨
git branch -d featureC  # 브랜치 삭제
```

Three-way merges

```bash
# main브랜치에 커밋사항이 있어서 ff merge가 되지 않을 때
git merge featureB  # merge commit message를 입력하라는 창이 뜨고 커밋된 이후 merge된다
```

#### conflict

```bash
git merge 브랜치명  # 에러가 뜬다
git st # 확인해보면 양쪽에서 수정해서 충돌했다고 알려줌
cat 충돌한-파일.확장자  # 충돌한 파일을 열어보면 어느 부분이 충돌했는지 보여줌

# 충돌사항만 해결해야함! 다른 코드 수정하면 🙅🏻

# 해결방법 1. 수동
open file.text  # 텍스트 편집기가 열리고 직접 수정함 (<<<HEAD, =====, >>>feature 도 일일이 다 삭제)
# 파일 저장후 끄고 나서 ① 또는 ②를 실행함

# ① merge 를 취소하려면
git merge --abort

# ② merge를 계속 진행하려면, conflict를 해결했다고 알려주고
git add file.text

git merge --continue  # ff merge 가 아니므로 merge commit이 만들어지고 진행됨


# 해결방법 2. VS Code로 해결하기  (config에서 설정해줌)
git merge 브랜치명  # 충돌했다고 경고뜬다
git mergetool # vscode가 열리고  <<<<< HEAD 위에 버튼들로 간편히 해결가능

# 버튼클릭해서 해결하고 저장 후 창을 끄고, 이어서
git merge --continue

# P4Merge로 mergetool변경해놨음
# 둘다 선택은 shift키, 나가고 나서 터미널 원상태로 돌리려면 control + c
# 그리고 git merge --contuinue 해주면 된다
```

### Rebase

다른 개발자와 같은 브랜치를 작업하고 있거나
이미 히스토리를 서버에 업로드했다면 rebase 사용하면 안된다 (로컬에서만!)

```bash
# 우선 feature-b branch로 이동 후
git checkout feature-b

# rebase 실행
git rebase main

# 다시 메인 브랜치로 이동
git checkout main

# feature-b 를 fast-forward merge 한다
git merge feature-b

# merge된 branch를 모두 삭제해줌
git branch -d feature-a
git branch -d feature-b


```

```bash

```

```bash

```
