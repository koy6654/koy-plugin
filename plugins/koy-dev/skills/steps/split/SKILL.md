---
name: split
description: resting.tmp.md를 새 세션 하나에 끝나는 issue로 나누고, issue마다 선행 issue를 적은 파일을 만든다.
argument-hint: "[scratch 폴더 경로 (같은 세션이면 생략)]"
disable-model-invocation: true
allowed-tools: Read(/${CLAUDE_PLUGIN_ROOT}/skills/**)
---

<!-- 나누기 규칙은 mattpocock/skills 의 to-tickets 를 바탕으로 고쳐 썼다. MIT License, Copyright (c) 2026 Matt Pocock. 출처는 ORIGIN.md. -->

# split

resting.tmp.md를 **issue**(새 세션 하나에 끝나는 작업 하나를 적은 파일)로 나눈다. 나누기만 한다. 구현은 사용자가 issue마다 새 세션에서 `/koy-dev:implement`로 진행한다.

## 시작 전 실행

`${CLAUDE_PLUGIN_ROOT}/skills/RULES.md`를 읽고 따른다. 같은 대화에서 이미 읽었으면 건너뛴다.

## 1. 불러오기

`<scratch 폴더>/resting.tmp.md`를 읽는다. 코드가 아직 낯설면 관련 영역을 훑는다. 먼저 구조를 고쳐두면 이후 구현이 쉬워지는 지점(선행 리팩터)이 있는지 본다.

## 2. 나누기

issue마다 다음을 지킨다.

- **세로로 자른다.** 한 issue가 필요한 모든 층(데이터, API 연동, 화면, 검증)을 좁게 관통한다. 한 층만 하는 issue는 만들지 않는다.
- **혼자서 확인할 수 있다.** issue가 끝나면 그 issue만으로 동작을 보여주거나 검증할 수 있다.
- **새 세션 하나에 끝난다.** issue 파일과 resting.tmp.md만 읽고 시작해도 일할 수 있다.
- **선행 리팩터가 있으면 맨 앞에 둔다.**

issue마다 **선행 issue**(이 issue보다 먼저 끝나야 하는 issue)을 정한다. 선행 issue가 없으면 바로 시작할 수 있다.

예외: 이름 바꾸기처럼 기계적인 변경 하나가 코드 전체에 퍼지는 경우는 세로로 자를 수 없다. 이때는 새 형태를 옆에 추가 → 호출부를 묶음 단위로 옮김 → 옛 형태 삭제 순서로 나눈다.

## 3. 보여주기

나눈 결과를 번호 목록으로 보여준다.

```
01. <제목>
    선행: 없음
    결과: <이 issue가 끝나면 처음부터 끝까지 동작하는 것>
02. ...
```

보여준 뒤 기다리지 않고 4번으로 간다. issue 파일을 쓴 뒤 사용자가 크기·선행 관계·합치기·나누기를 고치라고 하면 파일을 고친다.

## 4. issue 파일 쓰기

`<scratch 폴더>/issues/<NN>-<issue 이름>.tmp.md`에 issue 하나당 파일 하나를 쓴다. 선행 issue가 앞에 오도록 01부터 번호를 매긴다. 파일 경로와 코드 조각은 넣지 않는다.

```markdown
# <NN>: <issue 제목>

**만들 것:** <사용자 관점에서 이 issue가 동작하게 만드는 것>

**선행:** <번호와 제목 | 없음 (바로 시작 가능)>

**상태:** 대기

## 완료 기준
- [ ] <확인 가능한 기준>

## 참고
참고: `<scratch 폴더>/resting.tmp.md`
```

issue 파일을 다 쓰면 기록 `<scratch 폴더>/split.tmp.md`를 쓴다.

```markdown
# <기능 이름> issue 나누기

## issue 목록
| 번호 | 제목 | 선행 | 상태 |
| --- | --- | --- | --- |
| 01 | <제목> | 없음 | 대기 |

## 물어본 것
<RULES.md 「scratch 기록 규칙」의 「물어본 것」 형식. 없으면 "없음">
```

이 step에서는 로컬 파일만 쓴다. 저장소가 Jira·GitHub 같은 트래커를 쓰면, 올릴지 사용자에게 묻고, 올린다면 저장소 문서에 적힌 규칙을 따른다. 규칙을 찾지 못하면 묻는다.

## 5. 종료

issue 파일 목록을 보여주고 멈춘다. 진행 방법을 안내한다.

```
issue마다 새 세션에서:
/koy-dev:implement <scratch 폴더>/issues/01-<issue 이름>.tmp.md
→ /koy-dev:ready
→ 커밋 후, split.tmp.md issue 목록의 상태를 "완료"로 바꾼다
선행 issue가 모두 "완료"인 것부터 진행한다.
```
