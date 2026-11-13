---
name: feature
description: 기능 개발. grill → resting → implement → ready를 차례로 실행한다.
argument-hint: "<요청 | 이어할 scratch 폴더 경로>"
disable-model-invocation: true
allowed-tools: Read(/${CLAUDE_PLUGIN_ROOT}/skills/**)
---

# feature

## 원칙

- `${CLAUDE_PLUGIN_ROOT}/skills/RULES.md`를 읽고 따른다. 같은 대화에서 이미 읽었으면 건너뛴다.

## step 순서

1. grill
2. resting
3. implement
4. ready
5. close: 커밋한 뒤 사용자가 실행한다. flow는 4번에서 끝난다

## 실행

- 인자가 scratch 폴더 경로면 그 폴더에서 순서 중 기록이 없는 첫 step부터 실행한다. 아니면 새 scratch로 처음부터 실행한다.
- step마다 `${CLAUDE_PLUGIN_ROOT}/skills/steps/<step>/SKILL.md`를 읽고 따른다. 같은 세션이므로 모든 step이 이 scratch 폴더를 쓴다.
- 승인은 grill의 요구사항 확인, implement의 변경 계획 승인에서만 받는다. 그 외에는 멈추지 않고 다음 step으로 간다.
- ready가 ❌면 멈춘다.
- grill.tmp.md를 보고 한 세션에 끝나지 않을 크기면 멈추고 `/koy-dev:initial <scratch 폴더 경로>`를 안내한다.

## 종료

커밋한 뒤 `/koy-dev:close`을 실행하라고 안내한다.
