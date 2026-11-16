---
name: initial
description: 초기 구축·과제. grill → resting → split을 실행하고, ticket마다 implement → ready를 차례로 실행한다.
argument-hint: "<기획서·과제 설명 | 이어할 scratch 폴더 경로>"
disable-model-invocation: true
allowed-tools: Read(/${CLAUDE_PLUGIN_ROOT}/skills/**)
---

# initial

## 원칙

- `${CLAUDE_PLUGIN_ROOT}/skills/RULES.md`를 읽고 따른다. 같은 대화에서 이미 읽었으면 건너뛴다.

## step 순서

1. grill
2. resting: 기존 코드가 거의 없으면 `기존 기능과 다른 점`을 건너뛰고, 설계 결정(폴더 구조, 상태 관리, API 연동 방식, 공통 컴포넌트, 테스트 방식)을 `설계`에 정리한다. 앞으로 계속 지킬 규칙은 `CLAUDE.md 후보`에 적는다. 반영은 close가 한다.
3. split
4. ticket마다 implement → ready (아래 `ticket 진행`)
5. close: 커밋한 뒤 사용자가 실행한다. flow는 4번에서 끝난다

## 실행

- 인자가 scratch 폴더 경로면 그 폴더에서 순서 중 기록이 없는 첫 step부터 실행한다. 아니면 새 scratch로 처음부터 실행한다.
- step마다 `${CLAUDE_PLUGIN_ROOT}/skills/steps/<step>/SKILL.md`를 읽고 따른다. 같은 세션이므로 모든 step이 이 scratch 폴더를 쓴다.
- 승인은 grill의 요구사항 확인, ticket마다 implement의 변경 계획 승인에서만 받는다. 그 외에는 멈추지 않고 다음 step으로 간다.

## ticket 진행

선행 ticket이 모두 "완료"인 ticket부터 하나씩 진행한다.

1. **새 컨텍스트로 시작하는 subagent**에 ticket 파일과 resting.tmp.md를 넘겨, implement의 변경 계획만 세우게 한다. 코드는 고치지 않는다. 물을 것이 있으면 계획과 함께 돌려받는다.
2. 계획과 물을 것을 사용자에게 보여주고 승인받는다.
3. **새 컨텍스트로 시작하는 subagent**에 ticket 파일과 승인된 계획을 넘겨 implement → ready를 실행하게 한다.
4. ready가 ✅면 split.tmp.md 목록에서 그 ticket의 상태를 "완료"로 바꾸고 다음 ticket으로 간다. ❌면 멈춘다.

## 종료

모든 ticket이 "완료"되면, 커밋한 뒤 `/koy-dev:close`를 실행하라고 안내한다.
