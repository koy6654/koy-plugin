---
name: feature
description: 기능 개발. grill → resting(조건부) → implement → ready를 차례로 실행한다.
argument-hint: "<요청 | 이어할 scratch 폴더 경로>"
disable-model-invocation: true
---

# feature

시작할 때 `${CLAUDE_PLUGIN_ROOT}/skills/RULES.md`를 읽고 따른다. 같은 대화에서 이미 읽었으면 건너뛴다.

## step 순서

1. grill
2. resting: grill.tmp.md에 아래 중 하나가 있으면 실행, 없으면 건너뛴다
   - 권한, 결제, 데이터 삭제·변경을 건드림
   - 여러 모듈이나 화면에 걸침
   - 원천(시안, API 명세, 스키마, 정책서)과 대조가 필요함
   - 「열린 질문」이 남아 있음
3. implement
4. ready
5. close: 커밋한 뒤 사용자가 실행한다. flow는 4번에서 끝난다

## 실행

- 인자가 scratch 폴더 경로면 그 폴더에서 1번~4번 중 기록이 없는 첫 step부터 실행한다. 아니면 새 scratch로 1번부터 실행한다.
- step마다 `${CLAUDE_PLUGIN_ROOT}/skills/steps/<step>/SKILL.md`를 읽고 따른다. 같은 세션이므로 모든 step이 이 scratch 폴더를 쓴다.
- 승인은 grill의 요구사항 확인, implement의 변경 계획 승인에서만 받는다. 그 외에는 멈추지 않고 다음 step으로 간다.
- ready가 ❌면 멈춘다.
- grill에서 한 세션에 끝나지 않을 크기로 드러나면 멈추고 `/koy-dev:initial <이 scratch 폴더 경로>`로 바꾸라고 안내한다.

## 종료

커밋한 뒤 `/koy-dev:close`을 실행하라고 안내한다.
