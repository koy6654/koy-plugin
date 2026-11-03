---
name: bugfix
description: 버그 수정 흐름. diagnose → ready를 한 세션에서 이어 실행한다.
argument-hint: "<증상, 재현 방법, 기대한 동작 | 이어할 slug>"
disable-model-invocation: true
---

# bugfix

버그 수정 흐름. 이 skill의 기준 폴더에서 세 단계 위가 koy-dev plugin 루트(`<koy>`)다. **`<koy>/reference/running-a-flow.md`를 읽고 그 규칙대로 실행한다.** 이 파일에는 단계 목록과 이 흐름만의 판단만 있다.

## 단계

1. diagnose
2. ready

## ready에 넘길 명세

버그 수정에는 별도 명세가 없다. ready의 명세 축에는 diagnose-report.md를 명세로 넘기고, 다음을 보게 한다.

- 회귀 테스트가 사용자가 말한 증상을 단언하는가 (테스트 지점이 없으면 그 사실이 적혀 있는가)
- 원래 재현이 더 이상 재현되지 않는가
- 원인을 고쳤는가, 증상만 막았는가

## 이 흐름이 끝나면

ready 결과가 통과면 커밋 메시지 초안(맞았던 가설과 원인 포함)과 함께 끝낸다.
