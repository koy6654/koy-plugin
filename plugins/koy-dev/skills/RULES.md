# koy-dev 공통 규칙

## 용어 설명

- **step**: flow를 이루는 작업 단위의 skill. `skills/steps/`
- **flow**: step들을 정해진 순서로 실행하는 skill. `skills/flows/`
- **scratch**: 세션 하나의 기록을 모아 두는 임시 폴더. 저장소의 `.scratch/`

## scratch 규칙

세션 하나가 scratch 하나다. 그 세션의 결정과 진행을 scratch 폴더에 임시로 기록한다.
scratch 진행 중에는 CLAUDE.md, 저장소 문서, 용어집, ADR을 고치지 않는다. 저장소 반영, ADR, scratch 폴더 정리는 커밋한 뒤 close가 담당한다.

### scratch 폴더 규칙

- flow로 시작한 세션: `.scratch/flow-<scratch 이름>/` (예: `.scratch/flow-seat-change-modal/`)
- step 단독 실행으로 시작한 세션: `.scratch/step-<scratch 이름>/` (예: `.scratch/step-date-format/`)
- 기존 scratch를 이어야 하면 사용자에게 경로를 물어보고 그 경로에 이어 쓴다.
- git에 올리지 않는다.

### scratch 기록 규칙

step 하나가 끝날 때마다 scratch 폴더에 `<step>.tmp.md`를 하나 남긴다 (예: `grill.tmp.md`). 같은 step을 다시 실행하면 덮어쓴다.
다음 step, 새 세션, close가 이 기록과 코드만 보고 이어갈 수 있게 쓴다. 아래 다섯 칸은 반드시 포함하고, 필요한 설명은 덧붙인다.

- **요청**: 받은 요청 원문. 링크와 이슈 키 포함. scratch를 시작한 step이 아니면 "없음"
- **결정**: 무엇을 왜 정했나
- **확인한 것**: 무엇을 확인했고 근거가 어디인가. 쓴 저장소 skill도 여기에 적는다
- **남은 것**: 아직 정하지 못한 것, 다음 step이 처리할 것
- **물어본 것**: 저장소에서 못 찾아 물어본 것과 그 답, 추정으로 진행한 것. 없으면 "없음"

## skill 규칙

koy-dev skill과 저장소 skill이 함께 사용하는 규칙이다.

### skill 겹침 규칙

겹침은 step과 저장소 skill이 같은 일을 하는 경우다. 저장소 문서(ADR 포함)에 정해진 것이 있으면 따른다.
겹치는 step 대신 저장소 skill을 실행하고, 그 skill이 남긴 문서를 그 step의 기록으로 쓴다. 다섯 칸은 없어도 된다.

## step 규칙

koy-dev step을 어떻게 실행하는지에 대한 규칙이다.

### step 실행 규칙

step은 flow 안에서도, 단독으로도 실행된다.
flow 안에서는 step이 끝나도 멈추지 않고 다음 step으로 간다.

## ADR 규칙

결정마다 아래 네 조건을 확인한다. 두 개 이상 해당하면 `<step>.tmp.md`의 `결정` 칸에서 그 결정에 `ADR 후보`를 붙이고, 해당한 조건과 비교한 대안을 함께 적는다.

- **나중에 되돌리려면 여러 파일이나 데이터를 고쳐야 한다**
- **코드만 본 사람이나 모델이 "왜 이렇게 했지?" 하고 의아해한다**
- **다른 대안이 있었으나 비교해 보고 이유가 있어 골랐다**
- **다른 팀원, 모듈, 배포에 규칙을 강제한다**
