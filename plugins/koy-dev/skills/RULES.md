# koy-dev 공통 규칙

## 용어 설명

- **step**: flow를 이루는 작업 단위의 skill. `skills/steps/`
- **flow**: step들을 정해진 순서로 실행하는 skill. `skills/flows/`
- **koy-dev.md**: 저장소에 맞게 바꾼 flow 순서를 적는 파일. 저장소의 `.claude/koy-dev.md`
- **scratch**: 세션 하나의 기록을 모아 두는 임시 폴더. 저장소의 `.scratch/`
- **ticket**: split이 나눈 작업 하나를 적은 파일. 새 컨텍스트 하나에서 끝낼 수 있는 크기다. `<scratch 폴더>/tickets/`

## step 규칙

koy-dev step을 어떻게 실행하는지에 대한 규칙이다.

### step 실행 규칙

step은 flow 안에서도, 단독으로도 실행된다.
flow 안에서는 step이 끝나도 멈추지 않고 다음 step으로 간다.
step은 다음 step을 고르거나 안내하지 않는다. 결과를 `<step>.tmp.md`에 남기고 보고한 뒤 멈춘다. 다음 step은 flow의 순서가 정한다.
명령과 출력을 보여줄 때 비밀값은 `<REDACTED>`로 가린다.

### step 겹침 규칙

겹침은 step과 저장소 skill이 같은 일을 하는 경우다. 저장소 문서(ADR 포함)에 정해진 것이 있으면 따른다.
겹치는 step 대신 저장소 skill을 실행하고, 해당 skill에 대한 기록을 `tmp.md 기록 규칙`대로 쓴다.

## flow 규칙

koy-dev flow를 어떻게 실행하는지에 대한 규칙이다.

### flow 실행 규칙

flow는 `.claude/koy-dev.md`에 그 flow 섹션이 있으면 그 순서대로 실행한다. 없으면 flow의 `step 순서`를 따른다. 저장소 skill도 끝나면 `<skill 이름>.tmp.md`를 남긴다.
step을 시작할 때마다 진행 줄을 마지막에 출력한다. 현재 step은 굵게 쓴다. ticket마다 반복하는 step은 아래에 들여써 ticket마다 한 줄씩 쓴다.

진행 줄 예시:
```
---

**koy-dev 진행상황**

- [feature] scope → spec → split
  - (01 <ticket 제목>) build → verify
  - (02 <ticket 제목>) **build** → verify
```

## scratch 규칙

세션 하나가 scratch 하나다. 그 세션의 결정과 진행을 scratch 폴더에 임시로 기록한다.
scratch 진행 중에는 CLAUDE.md, 저장소 문서, 용어집, ADR을 고치지 않는다. 저장소 반영, ADR, scratch 폴더 정리는 커밋한 뒤 close가 담당한다.

### scratch 폴더 규칙

- flow로 시작한 세션: `.scratch/flow-<scratch 이름>/` (예: `.scratch/flow-seat-change-modal/`)
- step 단독 실행으로 시작한 세션: `.scratch/step-<scratch 이름>/` (예: `.scratch/step-date-format/`)
- 기존 scratch를 이어야 하면 사용자에게 경로를 물어보고 그 경로에 이어 쓴다.
- git에 올리지 않는다. scratch 폴더를 처음 만들 때 저장소 `.gitignore`에 `.scratch/`가 없으면 추가한다.

### tmp.md 기록 규칙

step 하나가 끝날 때마다 scratch 폴더에 `<step>.tmp.md`를 하나 남긴다 (예: `scope.tmp.md`). 같은 step을 다시 실행하면 덮어쓴다.
ticket을 입력으로 받은 step은 따로 기록을 만들지 않고, 그 ticket 파일 끝에 step 이름으로 덧붙인다. 다시 실행하면 그 step 부분만 바꾼다.
다음 step, 새 세션, close가 이 기록과 코드만 보고 이어갈 수 있게 쓴다. 아래 여섯 칸은 반드시 포함하고, 필요한 설명은 덧붙인다.

- **요청**: 받은 요청 원문. 링크와 이슈 키 포함. scratch를 시작한 step이 아니면 "없음"
- **결정**: 무엇을 왜 정했나
- **확인한 것**: 무엇을 확인했고 근거가 어디인가. 쓴 저장소 skill도 여기에 적는다
- **남은 것**: 아직 정하지 못한 것, 다음 step이 처리할 것
- **ADR 후보**: `ADR 규칙`에 해당하는 결정과 해당한 조건, 비교한 대안. 없으면 "없음"
- **물어본 것**: 저장소에서 못 찾아 물어본 것과 그 답, 추정으로 진행한 것. 없으면 "없음"

#### ADR 규칙

결정이 아래 조건 중 두 개 이상에 해당하면 ADR 후보다. 후보는 `<step>.tmp.md`의 `ADR 후보` 칸에 적고, close가 모아서 뒤집힌 결정은 빼고 같은 결정은 합친 뒤 ADR로 쓴다.

- **나중에 되돌리려면 여러 파일이나 데이터를 고쳐야 한다**
- **코드만 본 사람이나 모델이 "왜 이렇게 했지?" 하고 의아해한다**
- **다른 대안이 있었으나 비교해 보고 이유가 있어 골랐다**
- **다른 팀원, 모듈, 배포에 규칙을 강제한다**
