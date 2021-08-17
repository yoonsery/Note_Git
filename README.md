# GIT

## Git의 중요 컨셉 이해하기

### 1. Git Workflow

git에는 크게 3가지의 작업환경이 나눠져 있다

- `working directory`
  - 프로젝트의 파일드을 수정하는, 작업하고 있는 워킹 디렉토리
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

해당하는 커밋의 변경사항을 다시 없애주는, 삭제해주는, 취소해주는, 예전으로 돌려주는 커밋을 만듦

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

- p, pick : use commit
- r, reword : use commit but 메시지를 변경
- e, edit : use commit but 변경사항을 바꾼다
- s, squash : use commit but 여러가지의 커밋을 하나로 묶어준다 (meld into)
- f, fixup : squash와 비슷하지만 커밋메시지를 남기지않음
- x, exec : 이 커밋부터 shell명령어를 직접적으로 이용하고 싶을 때
- b, break : 여기서 멈춤
- d, drop : 해당하는 커밋을 제거할 때, history에 남기고 싶지 않을 때 사용
- l, label : label current HEAD with a name
