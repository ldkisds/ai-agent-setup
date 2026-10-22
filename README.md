# ai-agent-setup

AI 코딩 에이전트와 함께 일할 때 모든 프로젝트에 공통으로 쓰는 작업 규약과, 그 규약을 AI 도구별로 설치하는 묶음입니다.

## 사용 전에 알아 둘 점

`SETUP.md`와 스킬은 스크립트가 아니라 에이전트가 읽고 해석하는 문서입니다. 그래서 AI 도구나 세션에 따라 단계를 빠뜨리거나 다르게 해석하는 등 의도와 다르게 동작할 수 있습니다.

- 의도와 다르게 동작하면 쓰면서 고쳐 주세요. 그 프로젝트에서만 고치면 프로젝트 파일을, 다른 프로젝트에도 반영하려면 **toolkit 원본**(`core/`, `adapters/`, `SETUP.md`)을 고칩니다. 아래 "toolkit 원본 고치기"를 봅니다.
- 어떤 에이전트에서 어떻게 달랐는지 함께 남겨 두면, 다음에 같은 도구를 쓰는 사람이 덜 헤맵니다.

## 원칙

1. **규칙은 `core/`에만 둡니다.** `AGENTS.md`, 문서 뼈대, 스킬 본문은 특정 AI 도구·협업 플랫폼·도메인에 묶이지 않게 씁니다.
2. **AI 도구 전용 기능은 `adapters/<도구>/`에 둡니다.** 어댑터는 편의 기능일 뿐이고, 어댑터가 없어도 `AGENTS.md`만으로 같은 결과가 나와야 합니다.
3. **세팅은 한 번만 합니다.** 설치된 파일은 그 프로젝트의 것이 되어 자유롭게 고쳐 씁니다. 프로젝트 안의 `.ai-agent-setup/`(받아 온 원본)는 그대로 두고, 모든 프로젝트에 필요한 개선은 toolkit 원본을 직접 고칩니다.
4. **설치 스크립트 없이 에이전트가 설치합니다.** 에이전트가 [SETUP.md](SETUP.md)를 읽고 따라 하므로 OS나 AI 도구에 상관없이 같은 방법으로 설치됩니다.

## 구조

```
ai-agent-setup/
├─ README.md          ← 이 문서 (사람용)
├─ CHANGELOG.md       ← ai-agent-setup 자체의 변경 이력 (프로젝트로 복사하지 않음)
├─ SETUP.md           ← 설치 절차와 설치할 파일 목록 (에이전트가 따라 함)
├─ core/              ← 모든 AI 도구 공통
│  ├─ AGENTS.md
│  ├─ docs/           ← 프로젝트 docs/로 복사할 뼈대
│  └─ skills/         ← 표준 SKILL.md 형식, 도구 중립
└─ adapters/          ← AI 도구별로 골라 설치
   ├─ README.md       ← 새 어댑터 만드는 법, 스킬과 커맨드 구분
   ├─ claude-code/
   └─ codex/
```

어떤 파일이 어디에 설치되는지는 [SETUP.md의 파일 목록](SETUP.md#파일-목록)에 있습니다.

## 프로젝트에 적용하기

아래는 `my-project`라는 프로젝트에 ai-agent-setup을 적용하는 예입니다.

1. 프로젝트 루트에서 toolkit을 `.ai-agent-setup/` 폴더로 받습니다.

   ```
   cd my-project
   git clone <ai-agent-setup 저장소 주소> .ai-agent-setup
   ```

   아직 원격 저장소가 없으면 이 폴더를 `my-project/.ai-agent-setup/`으로 복사해도 됩니다. 나중에 toolkit 원본을 고칠 때 에이전트가 원본 위치를 물으면, 복사해 온 원본 폴더의 경로(예: `C:\work\ai-agent-setup`)를 알려 줍니다.

2. 프로젝트 루트에서 사용하는 AI 도구(예: Claude Code, Codex)를 열고 이렇게 요청합니다.

   ```
   .ai-agent-setup/SETUP.md를 읽고 초기 세팅해줘
   ```

3. 에이전트가 다음을 순서대로 합니다.
   - 이 프로젝트에서 쓰는 AI 도구를 모두 고르게 합니다. 여러 개를 골라도 됩니다 (예: Claude Code와 Codex). 선택 스킬(릴리스 노트)을 쓸지도 함께 묻습니다.
   - 만들 파일 목록을 보여 주고, 한 번에 동의를 받아 `.ai-agent-setup/` 안의 파일을 프로젝트 루트로 복사합니다.
   - 결정 0000을 절 단위로 묶어 물어 채웁니다. 일부만 채우고 나머지는 나중에 채워도 됩니다.

4. 결과를 확인하고 커밋합니다. 커밋 메시지에 toolkit 버전과 AI 도구가 들어가서(예: `CHORE: ai-agent-setup v0.1.0 적용 (claude-code)`) 어느 버전을 적용했는지 git 이력으로 알 수 있습니다. `.ai-agent-setup/`는 설치 원본이라 커밋하지 않습니다(`.gitignore`에 추가됨). 설치가 끝나면 필요하지 않고, AI 도구나 선택 스킬을 추가할 때만 다시 필요합니다. 팀원이 clone한 프로젝트에는 없으므로, 필요하면 원본 저장소에서 다시 받습니다.

적용 후 프로젝트 모습:

```
my-project/
├─ AGENTS.md          ← 공통 규약 (프로젝트에 맞게 고쳐 씀)
├─ CLAUDE.md          ← "@AGENTS.md" 한 줄 (Claude Code를 고른 경우)
├─ .ai-agent-setup/    ← 받아 온 toolkit 원본 (gitignore, 고치지 않음)
├─ .claude/           ← Claude Code 어댑터: 권한 설정, 스킬, 커맨드
├─ .agents/           ← Codex 어댑터: 스킬 (Codex를 고른 경우)
├─ docs/              ← 문서 뼈대: 문서화 기준, 코드 작성 기준, 결정 기록, 용어집, 그림 안내
├─ CHANGELOG.md       ← 프로젝트 진행 이력. /write-changelog를 처음 실행할 때 생김
└─ release-note/      ← /write-release-note를 처음 실행할 때 생김 (릴리스 노트 스킬을 고른 경우)
```

## SETUP.md는 언제 읽나

**어떤 AI 에이전트든 이 toolkit을 프로젝트에 적용하려면 `.ai-agent-setup/SETUP.md`를 한 번 읽어야 합니다.** 폴더를 넣어 두기만 해서는 적용되지 않습니다. AI 도구는 프로젝트 루트의 진입 파일(`AGENTS.md`, 또는 도구 전용 파일. 예: Claude Code의 `CLAUDE.md`)과 정해진 스킬 폴더만 읽기 때문입니다.

한 번 적용한 뒤에는 매번 읽을 필요가 없습니다.

| 상황 | SETUP.md를 읽어야 하나 |
|---|---|
| 프로젝트에 처음 적용 | **예.** 적용하는 사람이 에이전트에게 한 문장으로 요청합니다 |
| 적용 후 팀원이 clone해서 작업 | **아니요.** `AGENTS.md`, `CLAUDE.md`, `.claude/`가 커밋돼 있어 바로 적용됩니다 |
| 새 AI 도구 추가 (예: 팀원이 Codex로 열 때) | **예.** `AGENTS.md`의 세션 시작 점검이 안내합니다 |

- **예외**: `AGENTS.md`를 직접 읽지 않는 환경(예: Claude Code v2.1.277 이전 버전)에서 어댑터 없이 처음 열면 점검 안내가 나오지 않습니다. 이때는 사람이 "`.ai-agent-setup/SETUP.md`를 읽고 이 AI 도구의 어댑터를 설치해줘"라고 요청합니다.
- **`.ai-agent-setup/`가 없을 때**: 팀원이 clone한 프로젝트에는 `.ai-agent-setup/`가 없습니다. AI 도구나 선택 스킬을 추가해야 하면 먼저 원본 저장소에서 받습니다(`git clone <ai-agent-setup 저장소 주소> .ai-agent-setup`). 저장소가 아직 없으면 원본 폴더를 `.ai-agent-setup/`로 복사합니다.

## 매일 쓰는 흐름

스킬과 커맨드를 부르는 방법은 이 표에만 적습니다.

| 할 때 | Claude Code | Codex | 하는 일 |
|---|---|---|---|
| 커밋 | "커밋해줘" 또는 `/commit-message` | "커밋해줘" 또는 `$commit-message` | `commit-message` 스킬: 변경을 성격별로 나누고 TYPE을 붙여 커밋 제안 |
| 결정 기록 | (에이전트가 먼저 물어봄) | (같음) | `AGENTS.md` 규칙: 남길 만한 결정이면 에이전트가 기록할지 묻고, 동의하면 다음 번호로 작성 |
| 변경 이력 | `/write-changelog v0.3.3` | `$write-changelog v0.3.3` | `write-changelog` 스킬: 프로젝트의 이번 버전 변경을 프로젝트 루트 `CHANGELOG.md` 맨 위에 추가. 파일이 없으면 만들고, 버전을 비우면 물어봄 |
| 작업 마무리 | `/finish-issue v0.3.3` | `$finish-issue v0.3.3` | 커맨드(Codex는 직접 부르는 스킬): PR을 올리기 전에 테스트(결정 0000의 테스트 명령) → 문서 점검(결정 기록·가이드·용어집·TODO) → `write-changelog` → `commit-message` 순서로 정리. push와 PR 생성은 하지 않음 |
| 릴리스 노트 (선택 설치) | `/write-release-note v1.0.0 v1.2.0` | `$write-release-note v1.0.0 v1.2.0` | `write-release-note` 스킬: 두 태그 사이의 변경으로 배포용 릴리스 노트(구성 요소별 변경, 쿼리, 파일 목록)를 작성해 `release-note/release-note-<나중 태그>.md`로 저장 (아래 표) |

`/write-release-note`(Codex는 `$write-release-note`)는 태그를 0~2개 받습니다. 두 개를 줄 때 순서는 상관없고, git 이력을 보고 이전·나중을 정합니다.

| 입력 | 비교 범위 |
|---|---|
| `/write-release-note` | 직전 태그 → 가장 최근 태그 |
| `/write-release-note v1.2.0` | `v1.2.0` → 가장 최근 태그 |
| `/write-release-note v1.0.0 v1.2.0` | `v1.0.0` → `v1.2.0` |

스킬과 커맨드를 나누는 기준과 커맨드를 만드는 방법은 [adapters/README.md](adapters/README.md#스킬과-커맨드)를 봅니다.

프로젝트의 `CHANGELOG.md`와 릴리스 노트는 쓰임이 다릅니다. `CHANGELOG.md`는 프로젝트가 버전마다 무엇이 바뀌었는지 쌓아 가는 이력이고, 릴리스 노트는 두 버전 사이의 변경을 배포 단위로 모은 문서입니다. toolkit 폴더에 있는 `CHANGELOG.md`는 ai-agent-setup 자체의 이력이라 이 둘과 관계없습니다.

## toolkit 원본 고치기

세팅한 뒤 프로젝트에서 고친 내용 중 다른 프로젝트에도 필요한 개선은 toolkit 원본에도 반영합니다. 에이전트에게 "이 규칙은 ai-agent-setup 원본에도 반영해줘"라고 하면, 에이전트가 `AGENTS.md`의 "ai-agent-setup 원본 고치기" 절차대로 원본을 찾아 고칩니다.

원본을 고쳐도 이미 세팅한 다른 프로젝트에는 자동으로 반영되지 않습니다. 다음에 새로 세팅하는 프로젝트부터 반영됩니다.

## 다른 AI 도구 추가

지원하는 AI 도구는 [adapters/README.md](adapters/README.md#어댑터-목록)에 있습니다. 새 도구를 지원하려면 같은 문서의 "어댑터가 정해야 할 것"을 따릅니다.
