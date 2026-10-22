# 설치 절차

이 문서는 **에이전트가 읽고 따라 하는 절차**다. 사람은 에이전트에게 "`.ai-agent-setup/SETUP.md`를 읽고 초기 세팅해줘"라고만 하면 된다.

설치 스크립트는 두지 않는다. 어떤 AI 도구, 어떤 OS에서도 같은 절차로 설치되게 하기 위해서다.

아래에서 `<toolkit>`은 이 저장소를 받은 폴더(보통 프로젝트 루트의 `.ai-agent-setup/`)를, "프로젝트"는 그 바깥의 프로젝트 루트를 뜻한다.

## 설치

### 1. 사용할 AI 도구와 선택 스킬 정하기

- 지금 자신이 어떤 AI 도구인지 확인한다.
- `<toolkit>/adapters/`에 있는 어댑터 목록을 보여 주고, 이 프로젝트에서 쓰는 AI 도구를 **모두** 고르게 한다. 여러 개를 골라도 된다. 지금 설치를 진행하는 자신은 기본으로 골라 둔다.
  - 예: "이 프로젝트에서 쓸 AI 도구를 모두 골라 주세요: Claude Code(지금 사용 중), Codex"
- 고른 도구마다 `<toolkit>/adapters/<도구>/README.md`를 읽는다. 어댑터가 없으면, 그 도구는 core만 설치된다고 알린다.
- 여러 도구를 고르면 core 스킬이 도구마다 각자의 스킬 폴더로 복사된다(예: `.claude/skills/`, `.agents/skills/`). 같은 스킬이 두 곳에 생기는 것은 정상이다. 나중에 한쪽을 고치면 다른 쪽도 함께 고친다.
- [스킬](#스킬) 표에서 "선택"인 스킬을 보여 주고 쓸지 묻는다. AI 도구와 같은 질문에 함께 물어도 된다.
  - 예: "릴리스 노트 스킬(`write-release-note`)도 쓸까요? 두 태그 사이의 변경으로 배포용 릴리스 노트를 만듭니다."

### 2. 만들 파일 목록 보여 주기

- 아래 [파일 목록](#파일-목록)에서 core 절과 1단계에서 고른 도구의 절을 합친다.
- 프로젝트에 **이미 있는 파일은 목록에서 뺀다.** 폴더가 아니라 파일 단위로 확인한다. Sphinx·MkDocs 때문에 `docs/` 폴더가 이미 있을 수 있다.
- 이미 있는데 내용이 다른 파일(예: 기존 `CLAUDE.md`)은 따로 표시한다.
- 목록을 한 번에 보여 주고 **동의는 한 번만** 받는다.

### 3. 복사

- 동의받은 파일을 복사한다. 원본을 그대로 복사하고 내용을 추측해서 채우지 않는다.
- 고르지 않은 선택 스킬이 있으면, 복사한 결정 0000에서 그 스킬만 읽는 절을 지운다([스킬](#스킬) 표). 쓰지 않을 설정을 4단계에서 묻지 않기 위해서다.
- 이미 있는 파일과 겹치면 덮어쓰지 않고, 합칠 방법을 사용자와 정한다.
  - 예: 기존 `CLAUDE.md`에 내용이 있으면 맨 위에 `@AGENTS.md`를 추가한다. 나머지 중 프로젝트 정의에 해당하는 내용은 결정 0000으로 옮길지 묻는다.
- 프로젝트 `.gitignore`에 `.ai-agent-setup/`를 추가한다. 파일 목록에서 고른 도구의 절에 `.gitignore` 추가 항목이 있으면 함께 넣는다.
- 기존 파일(`.gitignore`, `CLAUDE.md` 등)을 고칠 때는 원래의 줄바꿈 형식(CRLF/LF)과 인코딩을 유지한다. 새로 넣는 줄도 같은 줄바꿈으로 쓴다.
- **복사한 파일이 커밋에서 빠지지 않는지 확인한다.** 기존 `.gitignore` 규칙에 걸리면 팀원에게 전달되지 않는다.

  ```
  git check-ignore -v <복사한 파일들>
  ```

  - 걸리는 파일이 있으면 어느 규칙 때문인지 보여 주고, 규칙을 좁힐지 묻는다. 예: `.claude/` 전체를 무시하고 있으면 `.claude/settings.local.json`만 무시하도록 바꾼다.
  - 동의 없이 `.gitignore`의 기존 규칙을 지우거나 바꾸지 않는다.
  - 일부러 무시하는 파일(`.ai-agent-setup/`, 파일 목록의 `.gitignore` 추가 항목)은 확인하지 않는다.
  - 프로젝트가 git 저장소가 아니면 이 확인은 건너뛰고, 그렇다고 알린다.

### 4. 결정 0000 채우기

`docs/decisions/0000-project-definition.md`의 항목을 **절 단위로 묶어 사용자에게 묻는다.** 한 번에 한 묶음씩, 아래 순서로 묻는다. 설정값은 규칙과 스킬이 읽는 값이라 먼저 묻는다.

1. 설정값
2. 배경·결정·개요
3. 코드와 문서·관련
4. 릴리스 노트 (`write-release-note`를 고른 경우)
5. 리포 고유 규칙

- 묻기 전에 "채울 묶음만 골라도 되고, 나머지는 `TODO`로 남겨 나중에 채워도 된다"고 알린다.
- 코드를 보고 짐작할 수 있는 값은 그 묶음의 초안으로 한꺼번에 보여 주고, "이대로 맞나요?"로 확인을 받은 뒤에 적는다.
- 모르는 항목과 고르지 않은 묶음은 비워 두고 `TODO`로 남긴다.
- 이 프로젝트에서 쓸 일이 없는 항목은 "이 프로젝트에서는 필요 없음"으로 적고 `TODO` 주석을 지운다. `TODO`는 "아직 정하지 않음"이고, "이 프로젝트에서는 필요 없음"은 "정했는데 쓸 일이 없음"이다.

### 5. 마무리

- 만든 파일과 비워 둔 항목을 요약한다. 3단계에서 `.gitignore`를 고쳤다면 무엇을 바꿨는지도 적는다.
- 커밋은 사용자에게 묻고 한다. 메시지 형식은 `AGENTS.md`의 "커밋"을 따르고, 어느 버전을 어떤 AI 도구용으로 적용했는지 git 이력으로 알 수 있게 toolkit 버전(`<toolkit>/CHANGELOG.md` 맨 위 항목의 `version`)과 고른 AI 도구를 넣는다. 예: `PROJ-12 CHORE: ai-agent-setup v0.1.0 적용 (claude-code, codex)`, 이슈가 없으면 `CHORE: ai-agent-setup v0.1.0 적용 (claude-code)`

## 파일 목록

설치하는 파일은 모두 여기에만 적는다. core 절은 항상 설치하고, 어댑터 절은 1단계에서 고른 도구만 설치한다. 프로젝트 파일이 toolkit의 어느 파일에서 왔는지 찾을 때도 이 목록을 본다.

### core

| toolkit 원본 | 프로젝트 위치 |
|---|---|
| `core/AGENTS.md` | `AGENTS.md` |
| `core/docs/documentation-standard.md` | `docs/documentation-standard.md` |
| `core/docs/coding-standard.md` | `docs/coding-standard.md` |
| `core/docs/glossary.md` | `docs/glossary.md` |
| `core/docs/how-to/README.md` | `docs/how-to/README.md` |
| `core/docs/decisions/README.md` | `docs/decisions/README.md` |
| `core/docs/decisions/TEMPLATE.md` | `docs/decisions/TEMPLATE.md` |
| `core/docs/decisions/0000-project-definition.md` | `docs/decisions/0000-project-definition.md` |
| `core/docs/README.md` | `docs/README.md` |
| `core/docs/images/README.md` | `docs/images/README.md` |
| `core/docs/images/example-doc-types.png` | `docs/images/example-doc-types.png` |

### 스킬

core 스킬은 고른 AI 도구마다 그 도구의 스킬 폴더로 복사한다(아래 어댑터 절). "선택"인 스킬은 1단계에서 쓰겠다고 한 경우에만 복사한다.

| 스킬 | 설치 | 하는 일 | 고르지 않으면 결정 0000에서 지울 절 |
|---|---|---|---|
| `commit-message` | 기본 | 변경을 커밋 규칙에 맞게 나누고 커밋 메시지를 만든다 | |
| `write-changelog` | 기본 | 프로젝트 `CHANGELOG.md`에 버전별 이력을 쌓는다 | |
| `write-release-note` | 선택 | 두 태그 사이의 변경으로 배포용 릴리스 노트를 쓴다 | "릴리스 노트" |

### claude-code

| toolkit 원본 | 프로젝트 위치 |
|---|---|
| `adapters/claude-code/CLAUDE.md` | `CLAUDE.md` |
| `adapters/claude-code/settings.json` | `.claude/settings.json` |
| `core/skills/<이름>/` | `.claude/skills/<이름>/` |
| `adapters/claude-code/commands/<이름>.md` | `.claude/commands/<이름>.md` |

`.gitignore`에 추가할 항목: `.claude/settings.local.json`

### codex

| toolkit 원본 | 프로젝트 위치 |
|---|---|
| `core/skills/<이름>/` | `.agents/skills/<이름>/` |
| `adapters/codex/skills/<이름>/` | `.agents/skills/<이름>/` |

`.gitignore`에 추가할 항목: 없음

## 설치한 뒤

- 복사된 파일은 모두 프로젝트 파일이다. 프로젝트에 맞게 자유롭게 고쳐 쓴다.
- `.ai-agent-setup/`는 설치가 끝나면 필요하지 않으므로 지워도 된다. AI 도구나 선택 스킬을 추가할 때만 다시 필요하다.
- 고치는 범위와 toolkit 원본에 반영하는 방법은 `AGENTS.md`의 "ai-agent-setup 원본 고치기"를 따른다.

## AI 도구나 선택 스킬 추가

- **AI 도구**: 다른 팀원이 새 AI 도구로 프로젝트를 처음 열면, `AGENTS.md`의 "세션 시작 점검"에 따라 그 도구의 어댑터가 설치됐는지 확인한다. 설치되지 않았으면 그 어댑터에 대해서만 설치 2~3단계를 한다. 커밋 메시지 예: `CHORE: ai-agent-setup v0.1.0 codex 어댑터 추가`
- **선택 스킬**: 처음에 고르지 않은 스킬이 필요해지면 그 스킬만 2~3단계로 설치한다. 설치할 때 지웠던 결정 0000의 절은 `<toolkit>/core/docs/decisions/0000-project-definition.md`에서 가져와 4단계처럼 채운다.
