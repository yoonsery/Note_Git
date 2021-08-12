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

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```
