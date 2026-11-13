---
name: bugfix
description: 버그 수정. diagnose → ready를 차례로 실행한다.
argument-hint: "<증상, 재현 방법, 기대한 동작 | 이어할 scratch 폴더 경로>"
disable-model-invocation: true
allowed-tools: Read(/${CLAUDE_PLUGIN_ROOT}/skills/**)
---

# bugfix

## 원칙

- `${CLAUDE_PLUGIN_ROOT}/skills/RULES.md`를 읽고 따른다. 같은 대화에서 이미 읽었으면 건너뛴다.

## step 순서

1. diagnose
2. ready: 기준 문서는 diagnose.tmp.md다. 기준 문서 축에서 다음을 본다
   - 회귀 테스트가 사용자가 말한 증상을 단언하는가 (테스트 지점이 없으면 그 사실이 적혀 있는가)
   - 원래 재현이 더 이상 재현되지 않는가
   - 원인을 고쳤는가, 증상만 막았는가
3. close: 커밋한 뒤 사용자가 실행한다. flow는 2번에서 끝난다

## 실행

- 인자가 scratch 폴더 경로면 그 폴더에서 순서 중 기록이 없는 첫 step부터 실행한다. 아니면 새 scratch로 처음부터 실행한다.
- step마다 `${CLAUDE_PLUGIN_ROOT}/skills/steps/<step>/SKILL.md`를 읽고 따른다. 같은 세션이므로 모든 step이 이 scratch 폴더를 쓴다.
- 승인은 받지 않는다. 멈추지 않고 다음 step으로 간다.
- diagnose가 확인 루프를 만들지 못해 멈추면 멈춘다. ready가 ❌면 멈춘다.

## 종료

커밋한 뒤 `/koy-dev:close`을 실행하라고 안내한다.
