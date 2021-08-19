# GIT

## Git의 중요 컨셉 이해하기

### 1. Git Workflow

git에는 크게 3가지의 작업환경이 나눠져 있다

- `working directory`
  - 프로젝트의 파일들을 수정하는, 작업하고 있는 워킹 디렉토리
- `staging area` (add)
  - 어느정도 작업하다가 버전 히스토리에 저장할 준비가 되어있는 파일들을 옮겨놓는 스테이징 에리어
- `.git directory` (commit)
  - 버전의 히스토리를 가지고 있는 깃 리파짓토리 (깃 디렉토리)

각각의 commit에는 스냅샷된 정보들을 기반으로 해서 고유한 hash code가 부여됨 <br>
이 해쉬코드를 이용해서 버전정보를 참조할 수 있다

커밋에는 아이디뿐만아니라 어떤 버전인지, 버전의 관련된 메시지와 누가 작성했는지 <br>
날짜와 시간같은 정보들도 함께 포함되어 있다

message, author, date, time...

#### 1-1. `working directory`

워킹 디렉토리는 2가지로 나눌 수 있다

- `untracked` : 새로 만들어진 파일 또는 기존에 존재하던 프로젝트에서 깃을 초기화 하게 된 파일 (기존 정보가 없음)
- `tracked` : git이 이미 트랙킹하고 있는 파일
  - `unmodified`, `modified` 수정 여부에 따라 둘로 또 나뉨
  - 수정된 `modified` 파일만 working directory에서 staging area로 옮겨 갈 수 있다

.git directory에 있는 우리의 커밋들은 사실 history의 창고이다
작업들을 버전별로 나눠서 관리할 수 있는 창고같은 아이 <br>
기능별로 작은 단위로 만들어 나가는 게 중요하다, 그렇다고 무의미하게 잘게 쪼개진 마라

- 예시)
  프로젝트를 초기화 하는 커밋 하나 Initialise project ⟶ Add LoginService module <br>
  ⟶ Add UserRepository module ⟶ Add Welcome page ⟶ Add About page <br>
  ⟶ Add light theme 이런 식으로 작은 단위로 의미있는 이름을 지정해서 커밋해나감 (의미있는 단위로)

  - 작업한 내용 빠르게 확인할 수 있다
  - 원하는 변경사항을 빠르게 찾아서 자세히 확인할 수 있다
  - 원치않는 커밋을 취소하기 편하다

- 커밋메시지는 동사, 현재형으로 작성 (init, add, fix...)

⚠️ﾠ커밋 시 주의사항

- 만약 fix crashing on login module 이라면 해당하는 사항만 커밋해야함 <br>
  (하는 김에 fix another bug, refactoring, new feature..하면 🙅🏻)

#### 소스트리로 커밋하기

파일을 수정한 후 add로 staged area에 올린 뒤 <br>
수정 사항의 부분이 맘에 들지 않으면 해당 코드만 선택해서 unstage할 수 있다 <br>
그럼 그 언스테이지한 부분만 워킹 디렉토리에 남아있는 걸 볼 수 있다 <br>
(좀 더 세분화해서 조정하고 싶을 때는 소스트리가 더 직관적이라 편함)

이렇게 수정하고 아래부분 name, email의 아래 입력창에 커밋메시지 작성하고 커밋하면 된다 <br>
그 아래 origin/main으로 바뀐 내용 즉시 push 체크하면 바로 리모트에 반영됨

#### log

`a ⟵ b ⟵ c ⟵ d ` 라는 커밋이 있다면 (이 줄기가 main 브랜치)

- `head`는 지금 내가 있는, 내가 바라보는 시점의 버전을 가리킴
- `head~1`은 지금 있는 현재 헤드의 이전 버전을 가르킴
- `head~2`는 지금 헤드가 있는 곳에서 두번째 부모를 찾아가는 것과 동일

이렇게 만들어진 커밋들은 내가 원하는 곳(시점)으로 언제든지 돌아갈 수 있다 <br>
만약 commit b로 돌아가고 싶다면 `git checkout b`로 갈 수 있다, head는 b 가 됨 <br>
`git checkout 해시코드` : 여기서 해쉬코드는 돌아가고싶은 커밋의 해시코드 입력 <br>
그럼 switching to 해시코드라고 알려주고 head가 바뀐게 보임

다시 git checkout main/master로 돌아오면 된당

### 태그는 왜 필요할까?

git 히스토리에 커밋들이 점점 더 많아진다면? <br>
git log가 굉장히 길어진다면? <br>
⟶ 어떤 특정한 부분으로 돌아가고 싶을 때 어려울 수 있다

특정 커밋을 북마크해두고 싶을 때 쓸 수 있는 것이 바로 `git tag`

원하는 부분으로 빠르게 전환할 수 있고 tag별로 관리가 쉽다 <br>
주로 릴리즈 하는 제품의 버전을 tag로 한다

대부분 `semantic versioning` 시스템을 따른다

`v2.0.0` : 3가지 숫자로 major 버전 | minor 업데이트 | fix 버젼을 구분해서 정함

- major 버젼: 특정한 기능이 추가되었을 때, 전체적인 업데이트가 발생했을 때 하나씩 올림
- minor 버젼: 커다란 기능중에서 조금의 기능들이 업데이트 되거나 개선이 되었을 때
- fix : 기존에 존재하는 기능에서 오류수정을 했을 때, 성능이 조금 개선되었을 때

얼마나 의미있게, 일관적으로 사용하는지가 중요하다

---

## 브랜치의 모든 것

별도로 브랜치를 따로 만들지 않으면 main 브랜치 한줄기에서 계속 커밋이 발생함 <br>
보통 main branch에는 기능에 문제가 없는, 제품에 포함이 되어도 되는, 검증된 코드만 들어감 <br>
만약 새로운 기능인 Feature A를 개발한다면 feature A 라는 브랜치를 생성해서 그곳에서 커밋을 쌓아감

feature A의 브랜치가 완성이 되고, 검증팀의 검증이 완료되고, 소스코드 리뷰도 완료됐다면 <br>
즉, 제품에 포함될 준비가 되었다면 feature A의 커밋들을 main branch로 merge를 할 수 있다 <br>
merge가 완벽하게 됐다면 더이상 필요없는 feature A 브랜치는 삭제함

feature A에 있는 모든 커밋을 다 merge할 필요가 없다 <br>
feature A에 있는 커밋들을 합해서 새로운 하나의 커밋을 만든 다음에 <br>
그 커밋만 깔끔하게 메인 브랜치로 가져온다 (회사/팀마다 다름)

### Merge

`Fast-forward merges`: merge중 가장 깔끔하고 간단 <br>
새로운 브랜치가 생성된 이후에 main 브랜치에 변동사항이 없다면, <br>
merge를 할 때 main이 가리키고 있는 포인터를 옮겨주기만 하면 된다 (프린트물 인쇄참조) <br>
그리고 브랜치 feature A를 삭제해주면 끝

단점: 히스토리에 merge가 되었다는 사실이 남지 않음

만약 히스토리에 merge된 흔적을 남기고 싶으면 <br>
`git merge --no-ff 브랜치명` <br>
별도의 merge commit을 만들어서 main에 merge함

`Three-way merges` ff merge가 되지 않을 때 merge commit 이 이뤄진 후 merge된다

#### merge conflict

git이 merge를 할 때 충돌이 났을 때 발생

- 두 가지의 branch에서 동일한 파일 수정했다면? 어떤 내용을 반영해야하지?

- 수동으로 해결
- vscode을 이용해 해결
- P4Merge로 해결하기

`git mergetool` p4merge가 실행되게 설정 후 <br>
실행하면 창이 뜬다 오른쪽의 초록색 LOCAL은 대개 내가 지금 작업중인 branch를 의미 <br>
왼쪽의 파란색 REMOTE는 merge하고자 하는 branch

아래창 수정하려는 부분은 파란색 클릭하면 리모트, 녹색 클릭하면 로컬 내용을 반영하고 <br>
둘다 선택하고 싶으면 `shift`키를 누른채 클릭하면 된다 <br>
그리고 저장하고 나가면 터미널에서 여전히 명령서 수행중 -> `control` + `C`로 종료함 <br>
`git st`하면 modified되었다고 나옴 <br>
`git merge --continue` 해주면 커밋메시지창 뜨고 저장해서 닫으면 merge완료됨

### Rebase란 무엇일까?

a - b - c - d - g <br>
ﾠﾠﾠﾠﾠﾠﾠﾠﾠﾠ\ e - f

three-way merge 라서 ff merge를 못한다면 <br>
파생된 커밋이 가리키는 지점을 main 브랜치의 최신버전으로 rebase를 한다 <br>
그럼 fast-forward가 가능함

⚠️ﾠ혼자서 feature-A(파생된) branch 에서 작업하고 있다면 rebase해도 괜찮지만 <br>
다른 작업자와 함께 feature-A 에서 작업하고 있다면 위험하다

`e`가 가리키고 있는 포인터를 `d`가 아니라 `g`로 변경해야하는데 <br>
이렇게 포인터의 정보를 변경하게 되면 기존의 커밋을 유지하는 게 아니라
새로운 커밋을 만들게 됨 <br>
(커밋은 불변의 진리이므로, 변경사항이 발생하면 새로운 커밋을 만듦) <br>
그래서 겉으로는 똑같아 보이지만 실제로는 다른 e, f의 커밋이 생김 <br>
so, 다른 개발자가 동일한 브랜치에서 작업중이라면 그 사람이 가지고 있는
feature A의 e, f는 전혀 다른 커밋이기 때문에 merge conflict 발생할 수 있다

🙅🏻 rebase 를 쓰면 안되는 경우 🙅🏻

- 다른 개발자와 같은 브랜치에서 작업중인 경우
- 이미 히스토리가 서버에 업로드되어 있는 경우

업로드된 히스토리는 리베이스 노노 <br>
나의 로컬에 있는 커밋에 한해서만 해도 된다

`rebase --onto` <br>
브랜치위에 또 다른 브랜치를 만들고..여러가지를 체이닝해서 작업할 때 유용함 <br>
파생된 브랜치에서 한번 더 파생된 브랜치를 main(또는 부모의 부모 브랜치)에 rebase하는 것 <br>
(파생되는 단계를 하나 없앤다고 보면된다) <br>
쓰면 안되는 경우도 rebase와 마찬가지

### Cherry picking

다른 브랜치에 있는 특정 커밋만 쏙 뽑아 가져오는 것

- branch에서 작업기간이 굉장히 오래 걸리거나
- 특정한 커밋을 가지고 오고 싶을 때 사용

## Stash

Stash Stack이 있어서 (pop을 할 수 있음) git history에 저장하지 않고도 작업하는 내용들을 잠시 저장해 둘 수 있다 <br>
아직 커밋할 단계는 아니라 staging area에 추가할 상황이 아닌데 다른 작업자의 브랜치를 확인해야 할 때 유용하다 <br>
stash command를 이용해 stash stack에 잠시 저장을 해두고 다른 작업자의 브랜치를 checkout해서 확인한 다음 <br>
stash stack에 있는 내용들을 다시 working directory로 가지고 올 수 있다

- 잠시 작업중이던 내용을 stash stack에 저장해두고, 다른 브랜치로 전환을 위해 사용 <br>
- 버그를 고칠 때 여러가지 방법으로 고치던 중에, 각각의 시도를 잠시 저장해두고 싶을 때 사용

## Undo

- 모든 수정사항을 함께 commit하지 못했거나
- commit의 메시지를 수정해야하거나
- commit을 너무 큰 단위로 해서 잘게 나눠서 다시 커밋하고 싶을 때
- 반대로 commit을 너무 자주해서 하나로 묶고 싶을 때
- 버그가 도입된 커밋을 삭제하거나 되돌릴 때 ... etc

⚠️ﾠgit의 history를 수정하는 것은 나의 local에 있는 것만 변경해야 한다

만약 서버에 이미 업로드한 경우라면 (push를 했고 다른 개발자와 공유가 된 히스토리라면) 절대 수정하면 안된다 <br>
push 했더라도 혼자하는 프로젝트라면 상관없다 <br>
rebase와 마찬가지로 history를 다시 작성하는 것은 새로운 커밋을 만드는 것과 같으므로 신중하게 사용해야 한다

`WIP` : Working In Progress

브랜치를 전환할 때 checkout 쓰던걸 switch로 바꿈, checkout 남용되던걸 세분화함

### Reset

특정한 커밋을로 모든 것을 초기화 시켜주는 명령어

`git reset` 은 `git reset --mixed` 와 동일하다

- `git reset --mixed HEAD~n` : 커밋을 히스토리에서 삭제하고 작업하던 내용들은 working directory로 이동
- `git reset --soft HEAD~n`: 커밋을 히스토리에서 삭제하고 작업하던 내용들은 staging area로 이동
- `git reset --hard HEAD~n`: 커밋을 히스토리에서 삭제하고 작업하던 내용들은 local에도 가져오지 말고 완전히 삭제

- `git reset --hard HEAD` : 내가 local에서 작업하고 있는 내용들을 깔끔하게 초기화해 준다

### Reflog ( reference log )

- `git reflog`
- `git reset --hard 해시코드`
- 언제든지 돌아갈 수 있지만! 이전에 commit이 된 상태에서만 가능하다 (커밋하기 전 이라면 reset --hard는 주의하기)

#### 커밋을 하지 않았는데 실수로 `git reset hard`를 했을 경우 되돌리기

- vscode extension 중에 Local History를 설치해서 사용

### Revert

해당하는 커밋의 변경사항을 다시 없애주는, 삭제해주는, 취소해주는, 예전으로 되돌려주는 커밋을 만듦

- revert 하는 상황:

  - main branch에 심각한 문제가 있는 커밋들
  - 다시 돌리고 싶은 것들
  - history에 남겨놓고 싶은 것들

- 이미 서버에 업데이트 된 커밋이라면 ~~`reset`~~, ~~`rebase`~~ 보다는 `revert`를 사용하자
- `revert`는 새로운 커밋을 만들어서 이미 추가된 내용을 변경하는 것이므로 기존 히스토리를 수정하는 것이 아니라서 언제든 자유롭게 이용가능
- 커밋을 따로 만들지 않고도 `revert`가능함

### Interactive Rebashing

- 기존에 존재하는 history를 소리 소문없이 간편하게 변경하는 방법 (서버에 업로드된 커밋이라면 사용하지 않는게 좋다)
- 혼자서 작업하는 경우, 커밋된 history를 변경하는 경우에 추천 👍

- 어떤 커밋을 rebasing을 하는 순간, 그 커밋 뒤에 있는 모든 포인터들이 업데이트 돼야 하기 때문에 그 이후로는 전부 다 새로운 아이템이 된다

- `git rebase -i 해시코드` 입력하면

```bash
pick 1d11be8 WIP
pick fa7bbd6 Add payment client
# 이런식으로 창이 뜨는데 해당 커밋의 해시코드 pick에 아래 명령어중 사용하려는 명령어를 쓰고 저장 후 닫는다

r 1d11be8 WIP
pick fa7bbd6 Add payment client

# r이라서 메시지 변경하는 창이 뜬다 ⟶ 변경 후 저장 닫기
```

- `p`, `pick` : use commit
- `r`, `reword` : use commit but 메시지를 변경
- `e`, `edit` : use commit but 변경사항을 바꾼다
- `s`, `squash` : use commit but 여러가지의 커밋을 하나로 묶어준다 (meld into)
- `f`, `fixup` : squash와 비슷하지만 커밋메시지를 남기지않음
- `x`, `exec` : 이 커밋부터 shell명령어를 직접적으로 이용하고 싶을 때
- `b`, `break` : 여기서 멈춤
- `d`, `drop` : 해당하는 커밋을 제거할 때, history에 남기고 싶지 않을 때 사용
- `l`, `label` : label current HEAD with a name

### [실습] 1. 필요없는 커밋 삭제하기

rebase를 할 때, 내가 rebase하려는 파일을 뒤에서 이어지는 commit에서 수정을 했다면 충돌이 발생한다 <br>
이런 상황에서는 매뉴얼하게 관리해줘야 함

만약 특정 커밋에서 파일을 삭제했는데 그 다음 커밋에서 삭제된 파일을 수정했다면?

```bash
git rebase -i 해시코드      #  수정하려는 커밋의 이전 해시코드

# 에디터가 나오면 commit을 히스토리에서 삭제를 원하므로 'pick 해시코드' 를 'd 해시코드'로 수정 - 저장 - 종료
# 알림창이 뜬다 - drop한 커밋에서 어떤 파일을 지웠는데 이어지는 다음 커밋에서 그 파일을 수정했어 so, 충돌발생
# 매뉴얼하게 수동적으로 관리해주기

git add .                # 충돌이 난 커밋에서 해당 파일을 그냥 사용하면 된다고 처리  (충돌된 커밋에서 새로운 파일이 생성됨)
git rebase --continue    # 변경사항이 발생했으므로 커밋 메시지를 수정할 수 있는 창이 뜬다. 상황에 맞게 수정하거나 그대로 두고 저장 - 닫기
```

### [실습] 2. 코끼리 커밋을 분할하기

commit 하나에는 한 가지만 하는 것이 중요하다  
라이브러리 하나 추가 | 기능 하나 추가 | 버그 하나 수정  
두 가지의 버그를 같이 수정하거나 두 가지의 dependency를 동시에 추가..는 🙅🏻  
그래야 히스토리 관리가 쉽고, 문제발견도 쉽고, revert도 용이함

```bash
git rebase -i 해시코드     # 수정하려는 커밋의 이전 해시코드

# 에디터가 뜨면 해당하는 해시코드 앞에 있는 pick을  e 로 수정하고 저장 - 종료
# git hist로 확인해보면 HEAD가 수정하고자 하는 커밋에 머물러 있음
# 해당 커밋을 쪼개려면 우선 commit을 나의 working directory로 가져와야 함

git reset HEAD~1          #  --mixed는 생략가능 & HEAD가 있는 이전으로 포인터를 돌려야 하므로 HEAD~1
git add 해당파일.text               # 해당파일을 staging area 로 보냄
git commit -m "message"           # 해당파일만 커밋함
git add 또다른파일.text              # 또다른 파일을 staging area로 보내서 커밋할 수 있게 준비
git commit -m "another message"   # 커밋하기

# 하나의 커밋을 쪼개서 두 가지의 커밋을 만듦 - 변경사항 확인 후 맘에 들면
git rebase --continue

git hist                          # 하나의 커밋이 2개로 쪼개져서 커밋된 내용 확인가능함 신기방기
```

### [실습] 3. history를 squash하기

```bash
git rebase -i hash        # 수정하려는 커밋의 이전 해시코드

# 에디터가 나오면,
# squash 는 제일 처음에 나오는 해시코드는 대표로 pick을 해주고 그 이후의 커밋은 s로 변경한다 - 저장 - 닫기
# 커밋 메시지 작성하는 창이 뜬다, 대표 커밋메시지 타이틀을 작성하고 저장 - 닫기
```

🚨 다시 강조! rebase는 서버에 push된 history를 rebase하면 안돼 (혼자 작업할 땐 상관 🙅🏻‍♀️)

---

## Remote

- server에 나의 git repository를 업로드
  - 내 pc에 문제가 생겼을 때 | 다른 환경에서 작업하고 싶을 때 repository에 접근하기 위해
  - 다른 개발자들과 함께 일을 할 수 있도록

Distributed Version Control 분산형 시스템

- `clone` : server에 이미 존재하고 있는 git repository를 가져올 때
- `push` : local에서 커밋한 내용을 서버에 올릴 때
- `pull` : 서버에 업데이트 된 내용을 나의 local에 업데이트 하고 싶다면
- `merge conflict` : 서버에서 업데이트 된 내용이 내가 로컬에서 작업하고 있는 동일한 파일일 때 `pull` 사용하면 발생

만약 나에게 접근권한이 없고, 오픈소스인 프로젝트라면?

### 오픈소스 프로젝트 참여하기

`fork` : 나의 repository에 동일하게 복사해서 가지고 오는 것  
오픈소스 프로젝트에 참여하여 코드를 작성하고 fork된 repository에 commit을 한다  
`PR`, `pull request`를 작성해서 오픈소스 프로젝트에 제출  
오픈소스 관리자가 나의 PR을 리뷰하고 제안 | 승인 | 거절을 할 것이다

승인 이후 만약 오픈소스 프로젝트에 다른 업데이트 된 커밋이 있다면  
`rebase`를 이용해서 나의 repository를 최신버전으로 한 다음 (오픈소스 프로젝트와 sync 한다)  
`merge`를 할 수 있다

### 깃허브 프로젝트를 내 PC에 가져오기

깃헙에서 repository를 만든다 - repository 주소를 복사한다  
터미널에서 `git clone URL` 실행  
repository에 있는 이름과 같은 폴더가 생성된다 만약 다른 폴더명을 원한다면  
`git clone URL 원하는폴더명` 이렇게 원하는 폴더명을 뒤에 입력하면 된다

`git remote` : 서버에 관련된 정보를 확인  
기본적으로 서버에 있는 것의 이름은 `origin`으로 설정된다  
`git remote -v` : origin이 어떤 걸 가리키고 있는지 정확한 정보 확인 가능

#### 다수의 원격 추가

현재 있는 폴더에서 다른 깃헙 링크를 추가하고 싶다면?  
(fork한 repository에는 다수의 origin을 설정할 수 있다)  
`git remote add server URL` : origin 대신 `server`라고 설정함

`git remote show` : 원격 리스트를 보여줌  
`git remote show origin` : origin의 관련된 정보를 자세히 볼 수 있다

### 나의 커밋을 서버에 저장하기

`SSH` (secure shell protocol)로 안전 | 간편하게 push하기
`git push`

rebase 등을 이용해서 history를 변경했을 경우에 서버와 충돌이 일어나면, 서버의 내용은 무시하고 내 로컬의 변경사항을 강제로 push하는  
`git push -f` 를 사용한다

### 이미 만들어진 프로젝트를 깃허브에 추가하기

깃헙에서 새로운 리파지토리 만들고 터미널에서 `git remote add URL`로 추가해준다  
로컬에 있는 커밋들을 서버로 push 해준다

### `fetch` vs `pull`

git에서 로컬과 서버에서 함께 연동해서 작업하는 경우에  
나의 로컬에서는 서버에 있는 브랜치를 origin이라고 표기한다

server에 새로운 commit이 발생했다면

- `fetch`: 서버에 있는 git history를 받아와서 나의 git history를 업데이트 한다, 작업환경 HEAD는 그대로 유지됨  
   즉 local main branch는 여전히 그대로 (서버에서 받아온 새로 추가된 커밋을 가리키고 있지 않음)
- `pull`: 서버에 있는 history를 가지고 오면서 나의 로컬에 있는 내용을 함께 merge를 한다,  
   새로운 커밋과 함께 origin main과 나의 main이 동시에 새롭게 추가된 커밋을 가리키고 있다

### fetch

fetch는 server의 업데이트 된 history를 가지고 온다  
origin(서버)에 있는 main과 origin의 HEAD는 최신 커밋을 가리키지만  
local의 HEAD는 여전히 업데이트 전의 커밋을 가리키고 있다  
So, 현재 작업하고 있는 HEAD는 그대로 유지하면서 서버에 업데이트 된 history 정보만 가지고 오고싶을 때 사용  
서버에 여러개의 branch가 있다면 특정한 브랜치만 가져올 수도 있다 (like `git fetch origin branch`)

서버에 있는 history 정보를 업데이트 해서 server에서 어떤 일들이 발생하고 있는지, 누가 어떤 일을 했는지 확인하고 싶은 경우에 많이 사용한다

### pull

서버에 있는 내용을 받아와서 나의 local 버전도 서버와 동일하게 만들고 싶을 때 사용

**만약 local과 server에서 동일한 파일을 수정해서 서로 새로운 commit이 있는 경우라면?**

이 상태에서 `git pull`을 사용하면 merge conflict 발생한다  
merge conflict: ( 다른 branch들 사이에서 || server 와 local에서 ) 동일한 파일을 수정했을 때 발생

해결방법은 앞에서 배운 `git mergetool` 사용해서 원하는 내용을 병합한 후 저장 - 닫기  
터미널에서도 `⌘ + c`로 종료 후 `git add .`로 모두 staging area로 옮긴 후 `git merge --continue`  
새로운 커밋 메시지 작성창이 뜬다 - 저장 - 종료  
이렇게 하면 기본적으로 merge가 발생한다

~~`merge`~~ 대신 `rebase`를 사용하고 싶다면?

`git pull --rebase` 을 사용하면 server에 있는 커밋을 가지고 온 상태에서 local에서 만든 커밋을 적용하려고 할 때 conflict이 발생한다  
`git mergetool` 실행 - 수정 - 저장 - 종료 - 터미널에서도 `⌘ + c`로 종료 - `git rebase --continue`  
커밋 메시지 작성창 - 저장 - 종료 - `git push` 로 서버에도 업데이트
merge보다 히스토리가 깔-끔✨

서버에 있는 커밋들은 그대로 가지고 와서 기존에 내 local에서 커밋이 되었던 것만 rebase를 했기 때문에  
로컬의 커밋만 새로운 커밋이 되었고 서버에서 받아온 커밋은 그대로 유지된다

---

### 오픈소스 프로젝트 참가방법

[자세한 방법은 여기서 👀](https://github.com/yoonsery/learning-git/blob/main/cheat-sheet.md/#list)

### 프로처럼 사용하기 Tip

#### 디버깅을 빠르게 하는 방법

프로젝트를 하다보면 누가 작성한 코드인지 추적하거나 || 언제 ・ 무엇때문에 이 코드가 도입되었는지 디버깅을 해야하는 경우가 있다

#### 1. Blame

`git blame file.text` 사용하거나 intelliJ에는 기본 기능으로 포함되어있다 vscode에는 `git lens` 익스텐션 설치  
 커밋된 해시코드와 누가 도입했는지 날짜, 시간도 확인가능

#### 2. Bisect | 문제의 원인을 빨리 찾는 법

원래 정상적으로 잘 동작하는 기능이었는데 `최근들어 조금씩 이상한 문제가 발생한 경우`라면  
 개발툴을 활용하거나 `디버깅툴을 이용`해서 `소스코드로 분석`하거나 `성능을 분석`해서  
 문제상황에 맞게 적절한 디버깅 툴을 이용하는 게 좋다 But! `디버깅툴 || 성능을 분석했는데도 문제가 잘 파악되지 않는다`면??  
 `git bisect` 사용할 수 있다

커밋 히스토리에서 어떤 부분까지는 괜찮았는데, 특정 커밋 이후부터 문제가 생긴거 같을 때  
 문제의 커밋을 빠르게 찾도록 도와준다  
 이진탐색 알고리즘을 이용해서 중간에 많은 커밋이 있더라도 빠르게 찾아낼 수 있다

`git checkout 해시코드`로 문제없이 잘 동작하는 것 같은 커밋으로 이동해서 프로그램을 실행해서 정말 문제가 없는지 확인한 다음  
 그곳에서 `git bisect start` 실행 - B라는 심볼이 생긴다 - 여기에 `git bisect good`이라고 마크를 해준다  
 마크한 뒤 다시 최신 main 포인터로 돌아가서(`git checkout main`) 여기에서도 다시한번 최신버전에서 문제가 발생하는지 확인  
 문제가 발생한다면 `git bisect bad`로 마크를 한다  
 이렇게 good, bad 지점을 설정해 놓으면 bisecting을 시작한다 ( 이진탐색 알고리즘이므로 빠름 )

중간지점 어딘가에 다른 커밋에 체크아웃된 상태이다 ( 여기서 git hist로 보면 HEAD가 중간에 있는 거 확인가능 )  
 이 상태에서 다시 프로그램을 돌려보면서 이슈가 발생하는지 확인한다 - 발생하지 않으면 `git bisect good`이라고 마크하면  
 좀 더 최근의 다른 커밋으로 이동한다 - 프로그램을 다시 돌려서 문제 발생하는지 확인 - 발생하지 않으면 `git bisect good`  
 발생하면 `git bisect bad`를 작성한다

그렇다면 good이라고 한 커밋과 bad라고 한 커밋의 중간 지점 어딘가에 나쁜 commit이 도입이 되었을 것  
 그 사이의 중간에 해당하는 커밋에 체크아웃이 된다 - 다시 프로그램 돌려서 문제 발생여부 확인 - good || bad 마크  
 결과물이 나오면 어떤 커밋이 문제인지 찾아서 알려준다  
 bisect를 이용해 해당하는 커밋을 찾았다면 `git bisect reset`을 이용하면 원래 브랜치로 돌아간다

#### 터미널 UI 인터페이스 툴
