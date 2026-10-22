# Claude Code 어댑터

## 진입 파일

Claude Code는 `CLAUDE.md`를 읽는다. v2.1.277부터는 `CLAUDE.md`가 없으면 `AGENTS.md`도 바로 읽지만, 그보다 앞선 버전, v2.1.281 전의 일부 환경(Bedrock 등), `CLAUDE.local.md`가 있는 경우에는 읽지 않는다. 그래서 어느 경우에나 같게 동작하도록 프로젝트 루트에 `@AGENTS.md` 한 줄만 있는 `CLAUDE.md`를 둔다. import로 불러와도 `AGENTS.md`가 두 번 읽히지는 않는다. ([공식 문서](https://code.claude.com/docs/en/memory.md), 2026-10-21 확인)

Claude에만 필요한 지시가 있으면 그 아래에 적을 수 있지만, 규칙은 넣지 않는다.

## 파일 목록

설치하는 파일과 `.gitignore` 추가 항목은 [SETUP.md의 파일 목록](../../SETUP.md#claude-code)에 있다.

## 설정 (`settings.json`)

- 팀이 같이 쓰는 권한만 둔다. 기본으로는 언어와 상관없는 차단 규칙만 넣었다.
  - 차단: 강제 push(`--force`, `--force-with-lease`, `-f`, `+<브랜치>`), 모든 폴더의 `.env`·`.env.*` 파일 읽기
- 허용 규칙은 기본으로 두지 않는다. 읽기 전용 git 명령은 Claude Code가 원래 묻지 않고 실행하고, `Bash(git diff:*)` 같은 허용 규칙은 `git diff --output=<파일>`처럼 파일을 쓰는 형태까지 묻지 않고 통과시키기 때문이다.
- 차단 규칙은 명령 문자열의 형태로만 맞추므로 `git -C . push --force`처럼 형태를 바꾼 명령은 막지 못한다. 실수를 막는 용도이고, 기본 브랜치를 확실히 보호하려면 저장소 호스팅(Bitbucket·GitHub·GitLab)의 브랜치 보호 설정을 쓴다.
- `--force-with-lease`도 막히므로, rebase한 브랜치를 다시 올릴 때는 사람이 터미널에서 직접 push한다.
- 프로젝트 언어에 맞는 명령(예: `poetry run pytest`)은 설치 후 프로젝트에서 추가한다.
- 개인 설정은 `.claude/settings.local.json`에 둔다.

## 스킬과 슬래시 커맨드

스킬은 커맨드 파일 없이 `/<스킬 이름> [인자]`로 바로 부를 수 있다. 스킬 본문에 `$ARGUMENTS`가 없으면 Claude Code가 본문 끝에 `ARGUMENTS: <입력값>`을 붙여 넘긴다. ([공식 문서](https://code.claude.com/docs/en/skills.md), 2026-10-21 확인)

커맨드는 `commands/<이름>.md`에 두고, 인자는 `$ARGUMENTS`로 받는다. 커맨드를 언제 만드는지는 [adapters/README.md](../README.md#스킬과-커맨드)를, 지금 있는 커맨드와 부르는 방법은 [README](../../README.md#매일-쓰는-흐름)를 본다.

## toolkit 원본 고치기

toolkit 원본이 프로젝트 밖의 폴더면, Claude Code에서 `/add-dir <원본 폴더 경로>`로 그 폴더에 접근을 허용한 뒤 고친다. 처음부터 `claude --add-dir <원본 폴더 경로>`로 열어도 된다. 무엇을 고칠지는 `AGENTS.md`의 "ai-agent-setup 원본 고치기"를 따른다.

## 권한 규칙 문법

[공식 문서](https://code.claude.com/docs/en/permissions.md)로 확인했다 (2026-10-21).

- `Bash(git diff:*)`와 `Bash(git diff *)`는 같다. `*`는 패턴 중간에도 쓸 수 있고, 공백을 포함한 어떤 글자와도 맞는다.
- `&&` 등으로 이어진 명령은 하위 명령 하나라도 deny 규칙에 맞으면 막힌다.
- `ls`, `cat`, `grep`, 읽기 전용 git 명령 같은 읽기 전용 명령은 모든 권한 모드에서 묻지 않고 실행된다. 이 목록은 바꿀 수 없고, 묻게 하려면 `ask`나 `deny` 규칙을 더한다. ([공식 문서](https://code.claude.com/docs/en/permissions#read-only-commands), 2026-10-22 확인)
- deny 규칙은 모든 권한 모드(자동 승인 포함)에서 적용된다.
- `Read(./.env)`는 Claude Code를 실행한 폴더 기준이고, `Read(**/.env)`는 모든 폴더의 `.env`와 맞는다.
