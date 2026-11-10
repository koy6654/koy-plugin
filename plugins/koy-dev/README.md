# koy-dev

Koy의 개발 방법론을 담은 Claude Code plugin. 프로젝트 정보와 skill은 붙인 저장소에서 가져온다. 공통 규칙은 [`skills/RULES.md`](./skills/RULES.md).

## skill

모두 사용자가 직접 입력해야 실행된다.

| 구분 | 명령어 | 하는 일 |
| --- | --- | --- |
| flow | `/koy-dev:feature` | 기능 개발: grill → (resting) → implement → ready |
| flow | `/koy-dev:initial` | 초기 구축: grill → resting → split |
| flow | `/koy-dev:bugfix` | 버그 수정: diagnose → ready |
| step | `/koy-dev:grill` | 요구사항 정리 |
| step | `/koy-dev:resting` | 코드와 원천(시안, API 명세, 스키마) 대조 |
| step | `/koy-dev:split` | 새 세션 하나에 끝나는 issue로 나누기 |
| step | `/koy-dev:implement` | 승인된 계획대로 구현 |
| step | `/koy-dev:ready` | 커밋 전 검증 |
| step | `/koy-dev:diagnose` | 버그 진단 |
| step | `/koy-dev:retro` | 회고 |
| step | `/koy-dev:close` | 커밋한 뒤 저장소 반영, ADR, scratch 정리 |
| setup | `/koy-dev:sync` | step별로 쓸 저장소 skill을 정해 koy-config에 남김 |

## 무엇을 쓸까

```
기능 개발:  /koy-dev:feature <요청>
초기 구축:  /koy-dev:initial <기획서> → (issue마다 새 세션) /koy-dev:implement → /koy-dev:ready
버그 수정:  /koy-dev:bugfix <증상>
작은 수정:  /koy-dev:implement → /koy-dev:ready
이어하기:   /koy-dev:<flow> <scratch 폴더 경로>
커밋한 뒤:  /koy-dev:retro (필요할 때) → /koy-dev:close
```

## 저장소에 생기는 파일

```
.claude/koy-config.md   koy-config
.scratch/               scratch. git에 올리지 않으려면 .git/info/exclude에 추가
```
