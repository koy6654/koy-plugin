# koy-dev

Koy의 공통 개발 프로세스를 담은 Claude Code plugin.

**공통 skill은 어느 프로젝트에서나 같고, 프로젝트마다 다른 것은 그 저장소의 `.claude/koy-config.md` 하나에 적는다.** skill은 무엇을 할지만 정하고, 무엇과 대조할지, 어떤 명령어로 검증할지, 어떤 저장소 skill을 함께 쓸지는 설정 파일에서 읽는다. 설정 파일은 `/koy-dev:sync`가 저장소를 살펴보고 만든다.

## skill

모두 사용자가 직접 입력해야 실행된다.

| 구분 | 명령어 | 하는 일 |
| --- | --- | --- |
| 준비 | `/koy-dev:sync` | 저장소를 살펴보고 koy-config.md를 만들거나 갱신. 저장소 skill 연결, 겹침 정리 |
| 흐름 | `/koy-dev:feature` | 기능 개발: grill → (resting) → implement → ready |
| 흐름 | `/koy-dev:build` | 과제·초기 구축: grill → resting → split → 멈춤 |
| 흐름 | `/koy-dev:bugfix` | 버그: diagnose → ready |
| 단계 | `/koy-dev:grill` | 요구사항 정리 → `requirements.md` |
| 단계 | `/koy-dev:resting` | 코드·대조 대상과 맞대어 빈틈 찾기 → `spec.md` |
| 단계 | `/koy-dev:split` | 새 세션 단위로 작업 나누기 → `issues/NN-*.md` |
| 단계 | `/koy-dev:implement` | 승인된 계획대로 최소 구현, 실제 동작으로 증명 |
| 단계 | `/koy-dev:ready` | 커밋 전 규칙 축과 명세 축 검증 → `ready-report.md` |
| 단계 | `/koy-dev:diagnose` | 버그 진단 → `diagnose-report.md` |
| 단계 | `/koy-dev:retro` | 교훈을 설정·자동 검사·공통 skill 중 어디에 넣을지 제안 |

결과물은 「산출물 위치」(기본 `.scratch/<slug>/`) 아래에 쌓이고, 흐름은 진행 상태를 `flow-state.md`에 남긴다. `/koy-dev:<흐름> <slug>`로 이어서 진행할 수 있다.

## 무엇을 쓸까

```
처음 붙일 때:  /koy-dev:sync
작은 수정:     /koy-dev:implement → /koy-dev:ready
기능 개발:     /koy-dev:feature
과제:          /koy-dev:build → (작업마다 새 세션) /koy-dev:implement → /koy-dev:ready
버그:          /koy-dev:bugfix
끝난 뒤:       /koy-dev:retro (필요할 때)
```

흐름 안에서 사용자가 확인하는 곳은 두 곳이다: grill 마지막의 요구사항 확인, implement의 변경 계획 승인. 나머지는 보고만 하고 진행한다.

## 구조

```
.claude-plugin/plugin.json   plugin 정의, skill 목록
skills/flows/                흐름 skill: 단계 skill과 저장소 skill을 순서대로 실행
skills/steps/                단계 skill: 흐름 안에서도, 단독으로도 사용
skills/setup/                sync
reference/                   skill들이 함께 읽는 규칙 (koy-config 사용법, 흐름 실행 규칙)
templates/                   koy-config.md 양식
ORIGIN.md                    외부 원본 출처와 라이선스
```

## 프로젝트에 생기는 파일

```
.claude/koy-config.md               /koy-dev:sync가 만드는 이 프로젝트의 koy 설정
<산출물 위치>/<slug>/                작업별 결과물 (requirements, spec, issues, 보고서, flow-state)
<산출물 위치>/koy-feedback.md        /koy-dev:retro가 쌓는 사용 지표, 문제점, 개선 제안
```

## 고쳐나가기

작업이 끝나면 `/koy-dev:retro`를 실행한다. 이 프로젝트에만 맞는 것은 koy-config, CLAUDE.md, 자동 검사로 바로 반영하고, 어디서나 맞는 것은 koy 저장소에 올릴 이슈 초안으로 만든다.
