---
name: feature
description: 기능 개발 흐름. grill → (resting) → implement → ready를 한 세션에서 이어 실행하고, 저장소 skill을 단계에 맞게 끼워 넣는다.
argument-hint: "<기획서·이슈 링크 또는 요청 설명 | 이어할 slug>"
disable-model-invocation: true
---

# feature

한 세션에 끝나는 기능 개발 흐름. 이 skill의 기준 폴더에서 세 단계 위가 koy-dev plugin 루트(`<koy>`)다. **`<koy>/reference/running-a-flow.md`를 읽고 그 규칙대로 실행한다.** 이 파일에는 단계 목록과 이 흐름만의 판단만 있다.

## 단계

1. grill
2. resting (조건부, 아래 판단)
3. implement
4. ready

## resting을 넣을지 판단

grill이 끝난 뒤 requirements.md를 보고 정한다. 아래 중 하나라도 해당하면 넣는다.

- 권한, 결제, 데이터 삭제·변경처럼 틀리면 비싼 영역을 건드린다
- 여러 모듈이나 화면에 걸친다
- 「대조 대상」과 맞대어 볼 원천(시안, API 명세, 스키마)이 있고 요구사항이 그것에 기대고 있다
- requirements.md에 「열린 질문」이 남아 있다

넣을지 뺄지와 그 이유를 한 줄로 보고하고 진행한다.

## 이 흐름이 끝나면

ready 결과가 통과면 커밋 메시지 초안과 함께 끝낸다. 실패면 막는 항목을 보여주고, 고친 뒤 `/koy-dev:ready`를 다시 실행하라고 안내한다.

작업이 한 세션에 안 끝날 만큼 크다고 grill 단계에서 드러나면, 멈추고 `/koy-dev:build <slug>`로 바꾸라고 안내한다. requirements.md는 그대로 이어서 쓴다.
