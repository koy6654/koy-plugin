---
name: split
description: spec.tmp.md를 새 컨텍스트 하나에 끝나는 ticket으로 나누고, ticket마다 선행 ticket을 적은 파일을 만든다.
argument-hint: "[scratch 폴더 경로 (같은 세션이면 생략)]"
disable-model-invocation: true
allowed-tools: Read(/${CLAUDE_PLUGIN_ROOT}/skills/**)
---

# split

작업 나누기 전용 step. spec.tmp.md를 **ticket**으로 나눈다. 코드는 고치지 않는다.

## 원칙

- `${CLAUDE_PLUGIN_ROOT}/skills/RULES.md`를 읽고 따른다. 같은 대화에서 이미 읽었으면 건너뛴다.

## 실행

### 1. 불러오기

`<scratch 폴더>/spec.tmp.md`를 읽는다.

### 2. 나누기

ticket을 나눌 때 다음을 지킨다.

- **먼저 바꿔 둘 구조는 맨 앞에 둔다.** 이번 변경을 쉽게 만들려면 기존 코드에서 먼저 바꿔 둘 구조가 있는지 본다.
- **vertical slice로 나눈다.** vertical slice는 기능 하나를 온전한 흐름으로 구현하는 ticket이다. 일의 종류별로(데이터, 화면, 로직, 인터페이스, 테스트 등) 나누지 않는다.
  - 예외: 코드 여러 곳에서 쓰는 이름 하나를 바꾸는 것처럼 변경 하나가 코드 전체에 퍼지면, 새 이름 추가 → 쓰는 곳을 몇 번에 나눠 새 이름으로 바꿈 → 옛 이름 삭제 순서로 나눈다.
- **독립적으로 확인할 수 있게 나눈다.** ticket은 선행 ticket 말고 다른 ticket에 기대지 않는다. 끝나면 그 ticket만으로 동작을 보여주거나 검증할 수 있다.
- **컨텍스트 하나에 끝나게 나눈다.** 컨텍스트는 에이전트가 한 번에 기억하며 일할 수 있는 분량이다. ticket 파일과 spec.tmp.md를 읽고 시작하여, 그 분량 안에서 끝낼 수 있는 크기로 나눈다.
- **선행 ticket을 정한다.** 선행 ticket은 이 ticket보다 먼저 끝나야 하는 ticket이다. 없으면 바로 시작할 수 있다.

### 3. ticket 파일 쓰기

`<scratch 폴더>/tickets/<ticket 번호>-<ticket 제목>.tmp.md`에 ticket 하나당 파일 하나를 쓴다. ticket 파일 안에는 코드 경로와 코드 조각을 넣지 않는다.

- `<ticket 번호>`: 두 자리 번호. 선행 ticket이 앞에 오도록 01부터 매긴다.
- `<ticket 제목>`: 무엇을 만드는지 드러나는 간결한 이름. kebab-case로 쓴다.

```markdown
# <ticket 번호>: <ticket 제목>

**만들 것:** <사용자 관점에서 이 ticket이 동작하게 만드는 것. 일의 종류별 작업 목록이 아니다>

**선행:** <번호와 제목 | 없음 (바로 시작 가능)>

## 완료 기준

- [ ] <확인 가능한 기준>
```

### 4. 기록 쓰기

`tmp.md 기록 규칙`대로 split.tmp.md를 쓰고, `ticket 목록`을 덧붙여 아래를 채운다.

- **ticket 목록**
  - ticket마다 번호, 제목, 선행, 상태를 한 줄씩 표로 적는다.
  - 상태는 `대기`와 `완료` 두 가지다. 처음에는 모두 `대기`다.
  - ticket을 끝낼 때마다 `완료`로 바꾼다.

### 5. 종료

split.tmp.md 경로와 내용을 간결하고 이해하기 쉽게 보고하고 멈춘다.
