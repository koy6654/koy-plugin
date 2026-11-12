---
name: close
description: 커밋한 뒤 scratch를 마무리한다. 물어본 것·용어·retro 제안을 저장소에 반영하고, ADR 후보를 ADR로 남기고, scratch 폴더를 지운다.
argument-hint: "[scratch 폴더 경로 (같은 세션이면 생략)]"
disable-model-invocation: true
allowed-tools: Read(/${CLAUDE_PLUGIN_ROOT}/skills/**)
---

# close

scratch를 끝내는 step. **커밋한 뒤** 실행한다. scratch 기록에서 오래 남길 것만 저장소로 옮기고, scratch 폴더를 지운다.

## 원칙

`${CLAUDE_PLUGIN_ROOT}/skills/RULES.md`를 읽고 따른다. 같은 대화에서 이미 읽었으면 건너뛴다.

## 실행

### 0. 확인

1. 이 세션의 scratch 폴더, 또는 인자로 받은 scratch 폴더 경로를 쓴다. 둘 다 없으면 멈추고 경로를 달라고 한다.
2. `git status`로 커밋되지 않은 변경이 있는지 본다. 있으면 멈추고, 커밋한 뒤 다시 실행하라고 안내한다.
3. scratch 폴더에 split.tmp.md가 있으면 issue 목록을 본다. "완료"가 아닌 issue가 있으면 멈추고 그 목록을 보여준다.

### 1. 저장소에 반영

scratch 폴더의 모든 기록에서 아래를 모아 항목마다 반영할 곳을 제안하고, 넣을지 묻는다. 승인한 것만 반영한다.

| 모을 것 | 어디서 | 반영할 곳 |
| --- | --- | --- |
| 물어본 것 | 모든 기록의 「물어본 것」 | close가 항목마다 제안한다 (CLAUDE.md, 저장소 문서, lint·CI, GLOSSARY·ADR, 저장소 skill) |
| 용어 | grill.tmp.md의 「용어」 | 저장소 용어집 (`GLOSSARY.md`, `CONTEXT.md` 등). 없으면 만들지 묻는다 |
| CLAUDE.md 후보 | resting.tmp.md의 「CLAUDE.md 후보」 (initial) | CLAUDE.md |
| retro 제안 | retro.tmp.md의 「문제점과 고칠 곳」 | 항목의 「고칠 곳」. koy 공통 skill이면 직접 고치지 않고 koy 저장소에 올릴 이슈 초안을 만든다 |

### 2. ADR

모든 기록에서 「ADR 후보」로 표시된 결정을 모아, 해당하는 조건이 많은 순서로 보여준다. 결정마다 ADR 초안(제목, 맥락, 결정, 고른 이유, 버린 대안)을 보여주고 남길지 묻는다. 승인한 것만 쓴다.

**위치**: 저장소에 이미 ADR 폴더가 있으면 그곳(CLAUDE.md나 기존 폴더로 확인). 없으면 `.adr/`. 번호와 파일 이름은 그 폴더의 기존 규칙을 따르고, 규칙이 없으면 `NNNN-<제목>.md`. ADR은 git에 올린다.

### 3. scratch 폴더 지우기

1단계와 2단계에서 반영한 것을 한 번에 보여준다. scratch 폴더를 지울지 묻고, 승인하면 지운다. 지우지 않기로 하면 그대로 둔다.

### 4. 종료

반영한 파일, 쓴 ADR, 만든 이슈 초안, scratch 폴더를 지웠는지를 보고한다. 반영한 파일은 커밋하지 않는다. 사용자가 커밋한다.
