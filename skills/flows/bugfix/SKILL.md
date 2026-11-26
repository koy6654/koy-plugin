---
name: bugfix
description: 버그 수정. debug → build → verify를 차례로 실행한다.
argument-hint: "<증상, 재현 방법, 기대한 동작 | 이어할 scratch 폴더 경로>"
allowed-tools: Read(/${CLAUDE_PLUGIN_ROOT}/skills/**)
---

# bugfix

## 원칙

- `${CLAUDE_PLUGIN_ROOT}/skills/RULES.md`를 읽고 따른다. 같은 대화에서 이미 읽었으면 건너뛴다.

## step 순서

1. debug
2. build
3. verify

## 종료

커밋한 뒤 `/koy-dev:close`를 실행하라고 안내한다.
