---
name: scope
description: 모호한 요청을 design tree로 그려 라운드마다 질문하고, 공통 이해에 이르면 요구사항을 기록으로 남긴다.
argument-hint: "<기획서 경로·링크 또는 요청 설명>"
disable-model-invocation: true
allowed-tools: Read(/${CLAUDE_PLUGIN_ROOT}/skills/**)
---

# scope

요구사항 정리 전용 step. 이 step이 끝나면 **무엇을 만들지**가 정해져 있다. 코드는 고치지 않는다.

## 원칙

- `${CLAUDE_PLUGIN_ROOT}/skills/RULES.md`를 읽고 따른다. 같은 대화에서 이미 읽었으면 건너뛴다.
- 무엇을 만들지(동작, 범위, 완료 기준)만 묻는다. 어떻게 만들지(기술 선택, 구조, 데이터 설계, 테스트 방식)는 spec이 정한다.
- 질문에 필요한 정보를 찾지 못하거나 읽지 못하면, 그때 사용자에게 묻는다.

## 실행

### 1. 라운드

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

Format a round like so:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ **추천 답변**: <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ **추천 답변**: <your recommended answer>
```

Word each question so "yes" accepts your recommended answer.

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent with a fresh context window to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.

### 2. 기록 쓰기

`tmp.md 기록 규칙`대로 scope.tmp.md를 쓴다.

### 3. 종료

기록 경로와 결정 요약을 간결하고 이해하기 쉽게 보고하고 멈춘다.
