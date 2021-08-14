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

각각의 commit에는 스냅샷된 정보들을 기반으로 해서 고유한 hash code가 부여됨
이 해쉬코드를 이용해서 버전정보를 참조할 수 있다

커밋에는 아이디뿐만아니라 어떤 버전인지, 버전의 관련된 메시지와 누가 작성했는지
날짜와 시간같은 정보들도 함께 포함되어 있다

message, author, date, time...

#### 1-1. `working directory`

워킹 디렉토리는 2가지로 나눌 수 있다

- `untracked` : 새로 만들어진 파일 또는 기존에 존재하던 프로젝트에서 깃을 초기화 하게 된 파일 (기존 정보가 없음)
- `tracked` : git이 이미 트랙킹하고 있는 파일
  - `unmodified`, `modified` 수정 여부에 따라 둘로 또 나뉨
  - 수정된 `modified` 파일만 working directory에서 staging area로 옮겨 갈 수 있다

.git directory에 있는 우리의 커밋들은 사실 history의 창고이다
작업들을 버전별로 나눠서 관리할 수 있는 창고같은 아이
기능별로 작은 단위로 만들어 나가는 게 중요하다, 그렇다고 무의미하게 잘게 쪼개진 마라

- 예시)
  프로젝트를 초기화 하는 커밋 하나 Initialise project ⟶ Add LoginService module
  ⟶ Add UserRepository module ⟶ Add Welcome page ⟶ Add About page
  ⟶ Add light theme 이런 식으로 작은 단위로 의미있는 이름을 지정해서 커밋해나감 (의미있는 단위로)

  - 작업한 내용 빠르게 확인할 수 있다
  - 원하는 변경사항을 빠르게 찾아서 자세히 확인할 수 있다
  - 원치않는 커밋을 취소하기 편하다

- 커밋메시지는 동사, 현재형으로 작성 (init, add, fix...)

⚠️ 커밋 시 주의사항

- 만약 fix crashing on login module 이라면 해당하는 사항만 커밋해야함
  (하는 김에 fix another bug, refactoring, new feature..하면 🙅🏻)

#### 소스트리로 커밋하기

파일을 수정한 후 add로 staged area에 올린 뒤
수정 사항의 부분이 맘에 들지 않으면 해당 코드만 선택해서 unstage할 수 있다
그럼 그 언스테이지한 부분만 워킹 디렉토리에 남아있는 걸 볼 수 있다
(좀 더 세분화해서 조정하고 싶을 때는 소스트리가 더 직관적이라 편함)

이렇게 수정하고 아래부분 name, email의 아래 입력창에 커밋메시지 작성하고 커밋하면 된다
그 아래 origin/main으로 바뀐 내용 즉시 push 체크하면 바로 리모트에 반영됨

#### log

`a ⟵ b ⟵ c ⟵ d ` 라는 커밋이 있다면 (이 줄기가 main 브랜치)

`head`는 지금 내가 있는, 내가 바라보는 시점의 버전을 가리킴
`head~1`은 지금 있는 현재 헤드의 이전 버전을 가르킴
`head~2`는 지금 헤드가 있는 곳에서 두번째 부모를 찾아가는 것과 동일

이렇게 만들어진 커밋들은 내가 원하는 곳(시점)으로 언제든지 돌아갈 수 있다
만약 commit b로 돌아가고 싶다면 `git checkout b`로 갈 수 있다, head는 b 가 됨
`git checkout 해시코드` : 여기서 해쉬코드는 돌아가고싶은 커밋의 해시코드 입력
그럼 switching to 해시코드라고 알려주고 head가 바뀐게 보임

다시 git checkout main/master로 돌아오면 된당

### 태그는 왜 필요할까?

git 히스토리에 커밋들이 점점 더 많아진다면?
git log가 굉장히 길어진다면?
⟶ 어떤 특정한 부분으로 돌아가고 싶을 때 어려울 수 있다

특정 커밋을 북마크해두고 싶을 때 쓸 수 있는 것이 바로 `git tag`

원하는 부분으로 빠르게 전환할 수 있고 tag별로 관리가 쉽다
주로 릴리즈 하는 제품의 버전을 tag로 한다

대부분 `semantic versioning` 시스템을 따른다

`v2.0.0` : 3가지 숫자로 major 버전 | minor 업데이트 | fix 버젼을 구분해서 정함

- major 버젼: 특정한 기능이 추가되었을 때, 전체적인 업데이트가 발생했을 때 하나씩 올림
- minor 버젼: 커다란 기능중에서 조금의 기능들이 업데이트 되거나 개선이 되었을 때
- fix : 기존에 존재하는 기능에서 오류수정을 했을 때, 성능이 조금 개선되었을 때

얼마나 의미있게, 일관적으로 사용하는지가 중요하다

## 브랜치의 모든 것

별도로 브랜치를 따로 만들지 않으면 main 브랜치 한줄기에서 계속 커밋이 발생함
보통 main branch에는 기능에 문제가 없는, 제품에 포함이 되어도 되는, 검증된 코드만 들어감
만약 새로운 기능인 Feature A를 개발한다면 feature A 라는 브랜치를 생성해서 그곳에서 커밋을 쌓아감

feature A의 브랜치가 완성이 되고, 검증팀의 검증이 완료되고, 소스코드 리뷰도 완료됐다면
즉, 제품에 포함될 준비가 되었다면 feature A의 커밋들을 main branch로 merge를 할 수 있다
merge가 완벽하게 됐다면 더이상 필요없는 feature A 브랜치는 삭제함

feature A에 있는 모든 커밋을 다 merge할 필요가 없다
feature A에 있는 커밋들을 합해서 새로운 하나의 커밋을 만든 다음에
그 커밋만 깔끔하게 메인 브랜치로 가져온다 (회사/팀마다 다름)

### Merge

`Fast-forward merges`: merge중 가장 깔끔하고 간단
새로운 브랜치가 생성된 이후에 main 브랜치에 변동사항이 없다면,
merge를 할 때 main이 가리키고 있는 포인터를 옮겨주기만 하면 된다 (프린트물 인쇄참조)
그리고 브랜치 feature A를 삭제해주면 끝

단점: 히스토리에 merge가 되었다는 사실이 남지 않음

만약 히스토리에 merge된 흔적을 남기고 싶으면
`git merge --no-ff 브랜치명`
별도의 merge commit을 만들어서 main에 merge함

`Three-way merges` ff merge가 되지 않을 때 merge commit 이 이뤄진 후 merge된다

#### merge conflict

git이 merge를 할 때 충돌이 났을 때 발생

- 두 가지의 branch에서 동일한 파일 수정했다면? 어떤 내용을 반영해야하지?

- 수동으로 해결
- vscode을 이용해 해결
- P4Merge로 해결하기

`git mergetool` p4merge가 실행되게 설정 후
실행하면 창이 뜬다 오른쪽의 초록색 LOCAL은 대개 내가 지금 작업중인 branch를 의미
왼쪽의 파란색 REMOTE는 merge하고자 하는 branch

아래창 수정하려는 부분은 파란색 클릭하면 리모트, 녹색 클릭하면 로컬 내용을 반영하고
둘다 선택하고 싶으면 `shift`키를 누른채 클릭하면 된다
그리고 저장하고 나가면 터미널에서 여전히 명령서 수행중 -> `control` + `C`로 종료함
`git st`하면 modified되었다고 나옴
`git merge --continue` 해주면 커밋메시지창 뜨고 저장해서 닫으면 merge완료됨

### Rebase란 무엇일까?

a - b - c - d - g
ﾠﾠﾠﾠﾠﾠﾠﾠﾠﾠﾠﾠﾠﾠﾠﾠﾠ\ e - f

three-way merge 라서 ff merge를 못한다면
파생된 커밋이 가리키는 지점을 main 브랜치의 최신버전으로 rebase를 한다
그럼 fast-forward가 가능함

⚠️ 혼자서 feature-A(파생된) branch 에서 작업하고 있다면 rebase해도 괜찮지만
다른 작업자와 함께 feature-A 에서 작업하고 있다면 위험하다

`e`가 가리키고 있는 포인터를 `d`가 아니라 `g`로 변경해야하는데
이렇게 포인터의 정보를 변경하게 되면 기존의 커밋을 유지하는 게 아니라
새로운 커밋을 만들게 됨
(커밋은 불변의 진리이므로, 변경사항이 발생하면 새로운 커밋을 만듦)
그래서 겉으로는 똑같아 보이지만 실제로는 다른 e, f의 커밋이 생김
so, 다른 개발자가 동일한 브랜치에서 작업중이라면 그 사람이 가지고 있는
feature A의 e, f는 전혀 다른 커밋이기 때문에 merge conflict 발생할 수 있다

🙅🏻 rebase 를 쓰면 안되는 경우 🙅🏻

- 다른 개발자와 같은 브랜치에서 작업중인 경우
- 이미 히스토리가 서버에 업로드되어 있는 경우

업로드된 히스토리는 리베이스 노노
나의 로컬에 있는 커밋에 한해서만 해도 된다
