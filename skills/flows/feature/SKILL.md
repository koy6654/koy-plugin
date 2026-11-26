---
name: feature
description: 기능 개발과 초기 구축. scope → spec → split을 실행하고, ticket마다 build → verify를 차례로 실행한다.
argument-hint: "<요청 | 이어할 scratch 폴더 경로>"
allowed-tools: Read(/${CLAUDE_PLUGIN_ROOT}/skills/**)
---

# feature

## 원칙

- `${CLAUDE_PLUGIN_ROOT}/skills/RULES.md`를 읽고 따른다. 같은 대화에서 이미 읽었으면 건너뛴다.

## step 순서

1. scope
2. spec
3. split
4. ticket마다 build → verify (아래 `ticket 진행`)

## ticket 진행

선행 ticket이 모두 "완료"인 ticket부터 하나씩 진행한다.

1. **새 컨텍스트로 시작하는 subagent**에 ticket 파일과 spec.tmp.md를 넘겨, build의 변경 계획만 세우게 한다. 코드는 고치지 않는다. 물을 것이 있으면 계획과 함께 돌려받는다.
2. 계획과 물을 것을 사용자에게 보여주고 승인받는다.
3. **새 컨텍스트로 시작하는 subagent**에 ticket 파일과 승인된 계획을 넘겨 build의 `구현` 단계부터 실행하고 verify를 실행하게 한다.
4. verify가 ✅면 split.tmp.md 목록에서 그 ticket의 상태를 "완료"로 바꾸고 다음 ticket으로 간다. ❌면 멈춘다.

## 종료

모든 ticket이 "완료"되면, 커밋한 뒤 `/koy-dev:close`를 실행하라고 안내한다.
