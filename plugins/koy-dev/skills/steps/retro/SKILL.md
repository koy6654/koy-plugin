---
name: retro
description: scratch가 끝난 뒤 세션과 기록을 돌아보고, 문제점과 고칠 곳(저장소 본래의 자리, koy 공통 skill)을 retro.tmp.md에 남긴다. 반영은 close가 한다.
argument-hint: "[scratch 폴더 경로 (같은 세션이면 생략)]"
disable-model-invocation: true
allowed-tools: Read(/${CLAUDE_PLUGIN_ROOT}/skills/**)
---

# retro

만든 코드가 아니라 **개발 환경**을 고칠 거리를 찾는다. 다음 scratch에서 같은 실수가 나지 않게 하는 것이 목적이다. 커밋한 뒤, close 전에 실행한다.

## 원칙

`${CLAUDE_PLUGIN_ROOT}/skills/RULES.md`를 읽고 따른다. 같은 대화에서 이미 읽었으면 건너뛴다.

## 실행

### 1. 원천 읽기

- 돌아볼 세션의 기록. 지정이 없으면 현재 세션.
- 그 scratch의 `<scratch 폴더>`의 기록. 특히 각 문서의 「물어본 것」과 `ready*.tmp.md`
- 이 프로젝트의 `CLAUDE.md`와 그것이 가리키는 문서

### 2. 문제점 찾기

세션에서 이런 순간을 찾는다.

- 에이전트가 같은 실수를 반복했다
- 필요한 정보를 찾는 데 오래 걸렸거나 끝내 못 찾았다
- skill이 지시한 step이 이 프로젝트에 맞지 않았다 (없는 대상을 찾았다, 불필요한 질문을 했다)
- 사용자가 끼어들어 방향을 바로잡았다
- 자동 검사가 잡을 수 있었던 실수가 ready까지 갔다

### 3. 고칠 곳 정하기

문제마다 고칠 곳을 하나 고른다.

| 고칠 곳 | 기준 |
| --- | --- |
| **자동 검사** (lint 규칙, 타입, 테스트, pre-commit, CI) | 정해진 패턴으로 잡을 수 있는 기계적인 실수. 규칙 문장보다 검사를 먼저 고려한다. 이미 있는데 연결이 안 된 검사가 있는지 먼저 본다 |
| **CLAUDE.md와 저장소 문서** | 계속 지킬 규칙, 자주 찾는 정보의 위치(시안 기준, 정책서 위치, 검증 명령어, 실제 동작 확인 방법). CLAUDE.md에는 짧게 위치만 가리키고 내용은 문서에 둔다 |
| **GLOSSARY·ADR** | 도메인 용어, RULES.md 「ADR 규칙」에 해당하는 결정 |
| **저장소 skill** (`.claude/skills/`) | 이 저장소에서 반복되는 절차 |
| **koy 공통 skill** | 어느 프로젝트에서나 맞는 방법론: step 순서, 원칙, 완료 기준, 질문 방식 |

### 4. retro.tmp.md 쓰기

기록 `<scratch 폴더>/retro.tmp.md`를 쓴다. 인자가 없으면 이 세션의 scratch 폴더에 쓴다.

```markdown
# <기능 이름> 회고

## 문제점과 고칠 곳
### <문제 한 줄>
- 근거: <세션이나 기록에서 일어난 일>
- 고칠 곳: <자동 검사 | CLAUDE.md·문서 | GLOSSARY·ADR | 저장소 skill | koy 공통 skill>
- 제안: <추가·수정할 문장이나 검사>

## 물어본 것
<RULES.md 「scratch 기록 규칙」의 「물어본 것」 형식. 없으면 "없음">
```

### 5. 보고

심각한 순서로 보여주고 멈춘다. 반영은 하지 않는다. `/koy-dev:close`가 retro.tmp.md의 항목을 하나씩 묻고 반영한다.
