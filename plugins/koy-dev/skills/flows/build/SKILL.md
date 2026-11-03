---
name: build
description: 과제·초기 구축 흐름. grill → resting → split을 한 세션에서 실행하고, 작업 파일까지 만든 뒤 멈춘다. 구현은 작업마다 새 세션에서 한다.
argument-hint: "<기획서·과제 설명 | 이어할 slug>"
disable-model-invocation: true
---

# build

여러 세션에 걸치는 과제와 초기 구축 흐름. 이 skill의 기준 폴더에서 세 단계 위가 koy-dev plugin 루트(`<koy>`)다. **`<koy>/reference/running-a-flow.md`를 읽고 그 규칙대로 실행한다.** 이 파일에는 단계 목록과 이 흐름만의 판단만 있다.

## 단계

1. grill
2. resting
3. split

## 초기 구축일 때

기존 코드가 거의 없으면 resting의 「비슷한 기능과 비교」는 건너뛰고, 대신 **설계 결정**(폴더 구조, 상태 관리, API 연동 방식, 공통 컴포넌트)을 명세의 「구현 결정」에 정리한다. 되돌리기 어려운 결정은 ADR로 제안하고, 앞으로 계속 지킬 규칙은 CLAUDE.md에 넣을 문장으로 제안한다.

## 이 흐름이 끝나면

split까지 끝나면 멈춘다. 작업 파일 목록과 진행 방법을 안내한다.

```
작업마다 새 세션에서:
/koy-dev:implement <산출물 위치>/<slug>/issues/01-<작업>.md
→ /koy-dev:ready
→ 커밋 후, 작업 파일의 상태를 "완료"로 바꾼다
선행 작업이 모두 "완료"인 파일부터 진행한다.
```
