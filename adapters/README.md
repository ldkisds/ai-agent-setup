# 어댑터 만들기

어댑터는 공통 규약(`core/`)을 특정 AI 도구에서 쓸 수 있게 연결하는 묶음입니다.

**어댑터에는 규칙을 넣지 않습니다.** 규칙은 `core/`에만 두고, 어댑터에는 그 도구에서만 필요한 연결과 편의 기능만 둡니다. 어댑터를 지워도 `AGENTS.md`의 규칙대로 일할 수 있어야 합니다.

## 어댑터 목록

| 도구 | 진입 파일 | 스킬 설치 위치 |
|---|---|---|
| [claude-code](claude-code/README.md) | `CLAUDE.md` (`@AGENTS.md`) | `.claude/skills/` |
| [codex](codex/README.md) | 없음 (`AGENTS.md`를 바로 읽음) | `.agents/skills/` |

### 스킬과 커맨드

core 스킬은 커맨드 파일 없이 바로 부를 수 있고, 인자도 함께 넘길 수 있습니다(Claude Code는 `/<스킬 이름>`, Codex는 `$<스킬 이름>`). 그래서 **스킬 하나를 감싸기만 하는 커맨드는 만들지 않습니다.**

커맨드는 **여러 단계를 순서대로 묶는 작업 흐름**처럼, 사용자가 시점을 정해 시작해야 하는 일에만 만듭니다. 각 단계의 절차는 core 스킬과 문서에 두고, 커맨드는 순서와 확인 지점만 정합니다. 같은 커맨드를 여러 도구에 만들면 각 파일의 단계를 같게 유지합니다.

| 도구 | 커맨드를 만드는 방법 | toolkit 안의 위치 |
|---|---|---|
| Claude Code | 슬래시 커맨드 | `adapters/claude-code/commands/<이름>.md` |
| Codex | 직접 부를 때만 실행되는 스킬 (`agents/openai.yaml`의 `policy.allow_implicit_invocation: false`) | `adapters/codex/skills/<이름>/` |

지금 있는 스킬·커맨드와 부르는 방법은 [README의 "매일 쓰는 흐름"](../README.md#매일-쓰는-흐름)에 있습니다. 새 커맨드를 추가하면 그 표에 한 줄 추가합니다.

## 어댑터가 정해야 할 것

`adapters/<도구>/README.md`에 아래 항목을 적습니다. 에이전트는 설치할 때(`SETUP.md`) 이 README를 읽고 그대로 따릅니다.

1. **진입 파일**: 그 도구가 `AGENTS.md`를 바로 읽는지, 연결 파일이 필요한지. 예: Claude Code는 `CLAUDE.md`에 `@AGENTS.md`를 적습니다.
2. **스킬 위치**: `core/skills/`를 프로젝트의 어느 폴더로 복사하는지. 예: `.claude/skills/`
3. **설정**: 권한, hooks 등 도구 전용 설정 파일이 있는지, 어디로 복사하는지.
4. **(선택) 커맨드**: 여러 core 스킬을 순서대로 묶는 작업 흐름. 절차는 core 스킬에 두고, 커맨드는 순서와 확인 지점만 정합니다. 예: 테스트·문서 점검 뒤 `write-changelog` → `commit-message`를 차례로 실행하는 흐름을 Claude Code는 `/finish-issue` 커맨드로, Codex는 직접 부르는 스킬 `$finish-issue`로 만듭니다.
5. **(선택) 도구 전용 스킬**: 그 도구에서만 동작하는 스킬은 `adapters/<도구>/skills/`에 둡니다. 설치할 때 core 스킬과 같은 폴더로 복사됩니다. 예: [codex/skills/finish-issue](codex/README.md#직접-부르는-스킬의-구성)
6. **파일 목록**: 설치할 파일(toolkit 원본 → 프로젝트 위치)과 `.gitignore` 추가 항목은 어댑터 README가 아니라 `SETUP.md`의 "파일 목록"에 이 도구의 절로 추가합니다. 파일 목록은 SETUP.md 한 곳에만 둡니다.
7. **(선택) 원본 고치는 방법**: 원본 폴더가 프로젝트 밖일 때 그 도구에서 접근을 허용하는 방법. 예: Claude Code의 `/add-dir`

## 만들 때 확인할 것

- 경로와 형식은 그 도구의 공식 문서로 확인하고, README에 출처와 확인 날짜를 적습니다. AI 도구는 자주 바뀝니다.
- 스킬은 표준 `SKILL.md` 형식(`name`, `description`, 본문)만 씁니다. 도구 전용 확장 필드가 꼭 필요하면 어댑터 쪽에서 덧붙입니다.
- 외부 스킬을 가져올 때는 원본 그대로 두고 라이선스 파일을 함께 넣습니다. 출처(URL, 커밋, 복사한 날짜)는 어댑터 README에 적습니다.
- 새 어댑터를 추가하면 위 "어댑터 목록" 표에 한 줄 추가합니다.
