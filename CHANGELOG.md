# 변경 이력 (Changelog)

ai-agent-setup 자체의 변경 이력입니다. toolkit 원본을 고치면 새로 세팅하는 프로젝트부터 반영되고, 이미 세팅한 프로젝트에는 자동으로 반영되지 않습니다 ([README](README.md)의 "toolkit 원본 고치기").

프로젝트의 진행 이력(프로젝트 루트의 `CHANGELOG.md`)과는 다른 파일입니다. 그쪽은 `write-changelog` 스킬이 씁니다.

- **버전 표기**: `vMAJOR.MINOR.PATCH`(예: `v0.1.0`)로 구분합니다. 독립 저장소로 옮기면 같은 이름의 git 태그를 붙입니다.
- **내 프로젝트에 어디까지 반영됐나**: 프로젝트에서 toolkit을 적용한 커밋 메시지의 버전을 봅니다(`git log --oneline --grep "ai-agent-setup"`). 그 버전까지의 항목이 반영된 것입니다.
- **쓰는 법**: 새 버전은 맨 위에 `v0.1.0` 항목과 같은 형식의 코드 블록으로 추가합니다. `summary` 끝에 관련 이슈 ID를 `[ ]`로 적고, `items`는 `added` · `updated` · `fixed` · `removed`로 나눈 뒤 그 안을 `[core]` · `[adapters]` · `[ETC]`로 묶습니다. 이미 세팅한 프로젝트도 고쳐야 하는 변경이면, 프로젝트에서 어떤 파일을 고치면 되는지 함께 적습니다.

---

```
version: v0.1.0
summary: ai-agent-setup의 기본적인 기능 작성 [DS-63]
items:
    added:
        - [core]
            - AGENTS.md: 모든 AI 도구에 공통인 작업 규약
            - docs/: 프로젝트 `docs/`로 복사할 뼈대 — 문서 목록(README), 문서화 기준, 코드 작성 기준(『읽기 좋은 코드가 좋은 코드다』 기반, 프로젝트마다 고쳐 씀), 용어집, how-to 목록, 결정 기록(안내 README, 0000 프로젝트 정의, TEMPLATE), 그림 첨부 안내와 예시 이미지
            - skills: 표준 SKILL.md 형식의 도구 중립 스킬 — commit-message·write-changelog(기본), write-release-note(SETUP에서 고르는 선택 설치)
        - [adapters]
            - AI Agent 별 command 정의 할 수 있도록 분리
            - finish-issue: PR 전에 테스트·문서 점검 → 변경 이력 → 커밋을 묶은 작업 흐름 (Claude Code 커맨드, Codex 직접 부르는 스킬)
        - [ETC]
            - SETUP.md 및 README.md 작성
```