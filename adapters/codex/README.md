# Codex 어댑터

아래 내용은 Codex 공식 문서로 확인했다(2026-10-21).

## 진입 파일

필요 없다. Codex는 작업을 시작하기 전에 프로젝트 루트(보통 git 루트)부터 현재 폴더까지 각 폴더의 `AGENTS.md`를 자동으로 읽는다. ([공식 문서](https://learn.chatgpt.com/docs/agent-configuration/agents-md))

- 여러 폴더의 `AGENTS.md`는 위에서 아래로 이어 붙이고, 현재 폴더에 가까운 파일이 우선한다.
- 합친 크기가 기본 32 KiB를 넘으면 나머지는 읽지 않는다. 공통 규약을 짧게 유지해야 하는 이유이기도 하다.

## 스킬 위치

프로젝트의 `.agents/skills/`. Codex는 현재 폴더부터 저장소 루트까지 올라가며 `.agents/skills/`를 찾는다. ([공식 문서](https://learn.chatgpt.com/docs/build-skills))

설치한 뒤 새 스킬이 보이지 않으면 Codex를 다시 시작한다.

## 파일 목록

설치하는 파일과 `.gitignore` 추가 항목은 [SETUP.md의 파일 목록](../../SETUP.md#codex)에 있다.

## 설정

기본 설정으로 동작하므로 따로 두는 설정 파일은 없다. 팀이 같이 쓸 설정이 필요해지면 그때 추가한다.

Codex는 기본으로 샌드박스에서 네트워크를 막을 수 있다. 그래서 `git fetch` 같은 명령은 승인을 요청하거나 실패할 수 있다. `write-release-note` 스킬은 `git fetch --tags`가 실패하면 로컬 태그로 계속 진행한다.

## 커맨드 대신 "직접 부르는 스킬"

Codex에는 Claude Code의 `.claude/commands/`처럼 저장소에 넣어 팀이 같이 쓰는 커맨드가 없다.

- 예전 기능인 custom prompts(`/prompts:<이름>`)는 deprecated다. 게다가 사용자 폴더(`~/.codex/prompts/`)에만 둘 수 있어서 저장소로 공유되지 않는다. ([공식 문서](https://learn.chatgpt.com/docs/custom-prompts))
- 대신 스킬의 `agents/openai.yaml`에 `policy.allow_implicit_invocation: false`를 준다. 그러면 Codex가 알아서 고르지 않고, 사용자가 `$스킬이름`으로 부르거나 `/skills`에서 고를 때만 실행된다. 커맨드와 같은 역할이다.
- Claude Code의 `$ARGUMENTS` 같은 인자 치환 기능이 없다. 그래서 스킬 본문에 "사용자가 부를 때 함께 적은 값을 읽는다"고 쓴다.

직접 부르는 스킬을 언제 만드는지는 [adapters/README.md](../README.md#스킬과-커맨드)를, 지금 있는 스킬과 부르는 방법은 [README](../../README.md#매일-쓰는-흐름)를 본다.

## 직접 부르는 스킬의 구성

`finish-issue`를 예로 든다. 단계는 Claude Code의 `/finish-issue`와 같게 유지한다.

- 각 단계의 절차는 core의 `write-changelog`, `commit-message` 스킬에 있고, 이 스킬은 순서와 확인 지점만 정한다. 설치하면 모두 같은 `.agents/skills/` 폴더에 들어가므로 `../write-changelog/SKILL.md`, `../commit-message/SKILL.md`로 찾을 수 있다.
- `agents/openai.yaml`의 `policy.allow_implicit_invocation: false`로 직접 부를 때만 실행되게 했다.

```
finish-issue/
├─ SKILL.md            ← 표준: 이름·설명(frontmatter) + 절차(본문)
└─ agents/openai.yaml  ← Codex 전용: 화면 표시 정보, 호출 방식
```

| 위치 | 구분 | 우리 규칙 |
|---|---|---|
| `SKILL.md`의 `name`, `description`, 본문 | 표준 | core 스킬과 같은 형식으로 쓴다. 본문에 도구 전용 지시를 넣지 않는다 |
| `agents/openai.yaml`의 `interface` | Codex 전용 | 표시 이름, 짧은 설명, 기본 프롬프트. 어댑터에서만 쓴다 |
| `agents/openai.yaml`의 `policy` | Codex 전용 | 커맨드처럼 쓸 스킬에만 `allow_implicit_invocation: false`를 준다 |

스킬 폴더에는 `scripts/`(보조 스크립트), `assets/`(아이콘 등 파일)도 둘 수 있다. 이 두 폴더는 표준 형식에 포함된다.

## 참고

- Codex에는 스킬 작성을 돕는 `$skill-creator` 스킬이 기본으로 들어 있다.
- 공식 스킬 모음: [openai/skills](https://github.com/openai/skills)
