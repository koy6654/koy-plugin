---
name: initial
description: 초기 구축·과제. grill → resting → split을 차례로 실행하고 issue 파일까지 만든 뒤 멈춘다.
argument-hint: "<기획서·과제 설명 | 이어할 scratch 폴더 경로>"
disable-model-invocation: true
allowed-tools: Read(/${CLAUDE_PLUGIN_ROOT}/skills/**)
---

# initial

## 시작 전 실행

`${CLAUDE_PLUGIN_ROOT}/skills/RULES.md`를 읽고 따른다. 같은 대화에서 이미 읽었으면 건너뛴다.

## step 순서

1. grill
2. resting: 기존 코드가 거의 없으면 「비슷한 기능과 비교」를 건너뛰고, 설계 결정(폴더 구조, 상태 관리, API 연동 방식, 공통 컴포넌트, 테스트 방식)을 「구현 결정」에 정리한다. 되돌리기 어려운 결정은 「ADR 후보」에, 앞으로 계속 지킬 규칙은 「CLAUDE.md 후보」에 적는다. 반영은 close가 한다.
3. split
4. issue마다 새 세션에서 implement → ready (사용자가 실행한다. flow는 3번에서 끝난다)
5. close: 모든 issue가 "완료"된 뒤 사용자가 실행한다

## 실행

- 인자가 scratch 폴더 경로면 그 폴더에서 1번~3번 중 기록이 없는 첫 step부터 실행한다. 아니면 새 scratch로 1번부터 실행한다.
- step마다 `${CLAUDE_PLUGIN_ROOT}/skills/steps/<step>/SKILL.md`를 읽고 따른다. 같은 세션이므로 모든 step이 이 scratch 폴더를 쓴다.
- 승인은 grill의 요구사항 확인에서만 받는다. 그 외에는 멈추지 않고 다음 step으로 간다.

## 종료

issue 진행 방법을 안내한다.

```
issue마다 새 세션에서:
/koy-dev:implement .scratch/flow-<scratch 이름>/issues/<NN>-<issue 이름>.tmp.md
→ /koy-dev:ready
→ 커밋 후 split.tmp.md issue 목록의 상태를 "완료"로 바꾼다
선행 issue가 모두 "완료"인 것부터 진행한다.
모든 issue가 "완료"되면 /koy-dev:close .scratch/flow-<scratch 이름>/
```
