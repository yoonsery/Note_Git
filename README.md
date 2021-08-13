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
