# 기본적인 터미널 언어

```bash
# 디렉토리 생성
mkdir 디렉토리명

# 파일 생성
touch 파일명.확장자

# 파일 삭제
rm 파일명

# 파일 강제 삭제
rm -rf 파일명
# 예시) rm -rf .git

# 디렉토리 삭제
rm -r 디렉토리명

# 파일 위치 변경 (이동)
mv 파일명.확장자 옮기려는디렉토리주소(또는 디렉토리이름)

# 파일 이름 변경
mv 기존파일명.확장자 새로운파일명.확장자

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
cat 파일명

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

```bash


```
