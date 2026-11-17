---
name: feature
description: 기능 개발과 초기 구축. requirement → design → split을 실행하고, ticket마다 build → verification을 차례로 실행한다.
argument-hint: "<요청 | 이어할 scratch 폴더 경로>"
disable-model-invocation: true
allowed-tools: Read(/${CLAUDE_PLUGIN_ROOT}/skills/**)
---

# feature

## 원칙

- `${CLAUDE_PLUGIN_ROOT}/skills/RULES.md`를 읽고 따른다. 같은 대화에서 이미 읽었으면 건너뛴다.

## step 순서

1. requirement
2. design
3. split
4. ticket마다 build → verification (아래 `ticket 진행`)
5. close: 커밋한 뒤 사용자가 실행한다. flow는 4번에서 끝난다

## 실행

- 인자가 scratch 폴더 경로면 그 폴더에서 순서 중 기록이 없는 첫 step부터 실행한다. 아니면 새 scratch로 처음부터 실행한다.
- step마다 `${CLAUDE_PLUGIN_ROOT}/skills/steps/<step>/SKILL.md`를 읽고 따른다. 같은 세션이므로 모든 step이 이 scratch 폴더를 쓴다.
- 승인은 requirement의 요구사항 확인, ticket마다 build의 변경 계획 승인에서만 받는다. 그 외에는 멈추지 않고 다음 step으로 간다.

## ticket 진행

선행 ticket이 모두 "완료"인 ticket부터 하나씩 진행한다.

1. **새 컨텍스트로 시작하는 subagent**에 ticket 파일과 design.tmp.md를 넘겨, build의 변경 계획만 세우게 한다. 코드는 고치지 않는다. 물을 것이 있으면 계획과 함께 돌려받는다.
2. 계획과 물을 것을 사용자에게 보여주고 승인받는다.
3. **새 컨텍스트로 시작하는 subagent**에 ticket 파일과 승인된 계획을 넘겨 build 3단계부터 실행하고 verification을 실행하게 한다.
4. verification이 ✅면 split.tmp.md 목록에서 그 ticket의 상태를 "완료"로 바꾸고 다음 ticket으로 간다. ❌면 멈춘다.

## 종료

모든 ticket이 "완료"되면, 커밋한 뒤 `/koy-dev:close`를 실행하라고 안내한다.
