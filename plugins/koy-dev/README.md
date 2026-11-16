# koy-dev

Koy의 개발 방법론을 담은 Claude Code plugin. 프로젝트 정보와 skill은 붙인 저장소에서 가져온다. 공통 규칙은 [`skills/RULES.md`](./skills/RULES.md).

## skill

모두 사용자가 직접 입력해야 실행된다.

| 구분 | 명령어 | 하는 일 |
| --- | --- | --- |
| flow | `/koy-dev:feature` | 기능 개발과 초기 구축: grill → resting → split, ticket마다 implement → ready |
| flow | `/koy-dev:bugfix` | 버그 수정: diagnose → ready |
| step | `/koy-dev:grill` | 요구사항 정리 |
| step | `/koy-dev:resting` | 구현 명세 |
| step | `/koy-dev:split` | 새 컨텍스트 하나에 끝나는 ticket으로 나누기 |
| step | `/koy-dev:implement` | 승인된 계획대로 구현 |
| step | `/koy-dev:ready` | 커밋 전 검증 |
| step | `/koy-dev:diagnose` | 버그 진단 |
| step | `/koy-dev:retro` | 회고 |
| step | `/koy-dev:close` | 커밋한 뒤 저장소 반영, ADR, scratch 정리 |

## 무엇을 쓸까

```
기능 개발:  /koy-dev:feature <요청>
버그 수정:  /koy-dev:bugfix <증상>
작은 수정:  /koy-dev:implement → /koy-dev:ready
이어하기:   /koy-dev:<flow> <scratch 폴더 경로>
커밋한 뒤:  /koy-dev:retro (필요할 때) → /koy-dev:close
```

## 저장소에 생기는 파일

```
.scratch/               scratch. 처음 만들 때 .gitignore에 자동으로 추가됨
.claude/koy-dev.md      저장소별 flow 순서 (선택). 없으면 기본 순서
```

`.claude/koy-dev.md` 예시.

```markdown
---
name: <저장소 이름>
description: <저장소 한 줄 설명>
---

## feature
grill → resting → split → implement → ready

## bugfix
diagnose → ready
```
