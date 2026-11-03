# koy 설정: <프로젝트 이름>

<!--
koy plugin의 skill이 읽는 프로젝트별 설정 파일이다. 위치는 프로젝트의 `.claude/koy-config.md`.
보통 `/koy-dev:sync`가 저장소를 살펴보고 만들거나 갱신한다. 손으로 고쳐도 되며, sync는 바뀐 부분만 고친다.
쓸 일이 없는 섹션은 지우지 말고 "없음"이라고 적는다. skill은 "없음"인 항목을 건너뛰고, 건너뛴 사실을 보고서에 남긴다.
계속 지킬 코딩 규칙(기술 스택, 레이어 규칙, 네이밍)은 여기가 아니라 CLAUDE.md에 둔다.
-->

## 분야

<FE | BE | INFRA | 기타>

## 산출물 위치

koy skill이 요구사항, 명세, 작업 파일, 보고서를 쓰는 곳.

- 위치: `.scratch/` (git에 올리지 않으려면 `.git/info/exclude`에 추가)
- ADR 위치: `docs/adr/`
- 용어집: `GLOSSARY.md` (없으면 "없음")

## 이슈 트래커

- 종류: <로컬 파일만 | Jira | GitHub | 없음>
- 규칙: <예: Jira AVIKO, 컴포넌트 FRONTEND 필수, 라벨 사용 안 함>

## 이미 정해진 것

grill이 다시 묻지 않는 전제. 기술 제약, 고정된 결정, 꼭 따를 참고 문서.

- <예: 모노레포(pnpm workspaces), 상태 관리는 기존 store 패턴을 따른다>

## 대조 대상

resting이 요구사항과 하나씩 대조하는 원천. 항목마다 어디에 있고 어떻게 보는지 적는다.

- <예: Figma 시안: 이슈 본문의 Figma 링크>
- <예: API 명세: Swagger>
- <예: DB 스키마: postgres>

## 놓친 점 체크리스트

resting이 요구사항에 없더라도 매번 확인하는 항목.

- <예: 화면 상태: 로딩 / 빈 화면 / 에러 / 권한 없음>
- <예: 권한·소유권 검증>

## 검증 명령어

implement와 ready가 실행하는 명령어. 실행 순서대로 적는다.

- <예: pnpm lint>
- <예: pnpm build>
- <예: 테스트: 없음>

## 실제 동작 확인

명령어 통과와 별개로, 결과물이 실제로 동작하는지 직접 관찰하는 방법.

- <예: Storybook 또는 브라우저에서 Figma 시안과 실제 화면을 비교한다>

## 사람만 실행하는 명령

koy skill이 실행하지 않고 사람에게 넘기는 명령. 없으면 "없음".

- <예: terraform apply, DB migration 적용, 배포>

## 커밋 규칙

- <예: Conventional Commits, 이슈 키를 앞에 붙인다 (AVIKO-1234: ...)>

## 단계별 연결 skill

koy 단계가 실행될 때 함께 쓰는 이 저장소의 skill. 부르는 방법은 `Skill tool`(모델이 실행 가능한 skill) 또는 `파일 읽기`(사용자만 실행 가능한 skill, 경로를 적는다).

| koy 단계 | 저장소 skill | 언제 | 부르는 방법 |
| --- | --- | --- | --- |
| <resting> | </policy-check> | <결제·Seat 작업일 때> | <Skill tool> |

## 겹침 결정

koy 단계와 같은 일을 하는 저장소 skill과, 어느 쪽을 쓸지에 대한 결정. 기본은 **저장소 우선**이다.

| koy 단계 | 겹치는 저장소 skill | 결정 | 저장소 skill의 결과물 위치 |
| --- | --- | --- | --- |
| <grill> | </development:grill> | <저장소 우선> | <.docs/aviko-be/works/grill/> |

## sync 기준

`/koy-dev:sync`가 자동으로 쓴다. 손으로 고치지 않는다.

- 날짜:
- koy 버전:
- 저장소 지문:
