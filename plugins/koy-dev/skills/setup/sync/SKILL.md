---
name: sync
description: 저장소를 살펴보고 .claude/koy-config.md를 만들거나 바뀐 부분만 갱신한다. 저장소 skill을 koy 단계에 연결하고 겹치는 skill을 정리한다.
argument-hint: "(인자 없음)"
disable-model-invocation: true
---

# sync

이 저장소에서 koy가 어떻게 일할지 `.claude/koy-config.md`에 맞춘다. 처음 붙일 때와 저장소가 바뀌었을 때 같은 skill을 쓴다. **설정만 만들고, 어떤 단계도 실행하지 않는다.**

이 skill의 기준 폴더에서 세 단계 위가 koy-dev plugin 루트(`<koy>`)다. 양식은 `<koy>/templates/koy-config.md`, 공통 규칙은 `<koy>/reference/koy-config-usage.md`.

**방향은 한쪽이다.** koy가 저장소를 읽어서 연결만 한다. 저장소 skill을 koy로 복사하거나, koy skill을 저장소로 복사하지 않는다.

## 1. 살펴보기

프로젝트 루트에서 다음을 읽는다. 추측하지 않는다.

- `.claude/koy-config.md`가 이미 있는가. 있으면 「sync 기준」과 지금의 저장소 지문, koy 버전을 비교해 **무엇이 바뀌었는지**부터 정리한다.
- `CLAUDE.md`, `AGENTS.md`: 이미 정해진 규칙, 참고 문서 위치
- `.claude/skills/*/SKILL.md`, `.claude/commands/**/*.md` (symlink도 따라간다): 이름, 설명, `disable-model-invocation` 여부
- `package.json`의 scripts (모노레포면 workspace 설정도): 검증 명령어 후보
- 테스트 도구와 테스트 파일이 있는가
- 기존 산출물 관례: `.scratch/`, `docs/adr/`, `GLOSSARY.md`·`CONTEXT.md` 같은 용어집, 팀 문서 폴더
- `git remote -v`, 이슈 키가 보이는 최근 커밋 메시지: 이슈 트래커와 커밋 규칙 후보

## 2. 대응시키기

저장소 skill과 명령어를 하나씩 보고 셋 중 하나로 분류한다.

| 분류 | 기준 | 기록할 곳 |
| --- | --- | --- |
| **연결** | koy 단계 안에서 보조로 쓸 수 있다 (예: 정책서 확인, API 명세 추출, Figma 구현, ERD) | 「단계별 연결 skill」 |
| **겹침** | koy 단계 하나와 같은 일을 한다 (예: 팀의 grill·implement·review 명령어) | 「겹침 결정」 |
| **무관** | koy 흐름과 상관없다 (예: 주간 보고, 워크트리 관리) | 기록하지 않음 |

연결마다 「부르는 방법」을 적는다. `disable-model-invocation: true`이거나 명령어 파일이면 `파일 읽기`와 경로, 아니면 `Skill tool`.

겹침마다 결정의 기본값은 **저장소 우선**이다. 그 저장소 skill이 결과물을 어디에 쓰는지 찾아 「저장소 skill의 결과물 위치」에 적는다.

## 3. 초안 보여주기

새로 만들 때는 전체 초안을, 갱신일 때는 **바뀌는 부분만** 보여준다. 겹침은 따로 모아 묻는다.

```
겹침:
1. grill ↔ /development:grill   추천: 저장소 우선
2. ready ↔ /review-feature      추천: 저장소 우선
→ 추천대로 / 항목별로 바꾸기
```

추측으로 채운 항목(예: 실제 동작 확인 방법)은 "추정"이라고 표시하고 확인을 받는다. 사용자가 승인할 때까지 고친다.

## 4. 쓰기

승인되면 `.claude/koy-config.md`를 쓴다.

- 갱신일 때는 사람이 손으로 고친 문장을 지우지 않는다. 바뀐 부분만 고친다.
- 「sync 기준」에 오늘 날짜, koy 버전, 저장소 지문을 쓴다 (계산 방법은 `koy-config-usage.md`).
- 「산출물 위치」가 `.scratch/`이고 git에 올리지 않기로 했으면 `.git/info/exclude`에 추가할지 묻는다.
- koy-config.md 자체를 git에 올릴지 묻는다. 팀원이 koy를 쓰지 않으면 올리지 않는 쪽을 추천한다.

## 5. 종료

무엇을 만들었거나 바꿨는지 요약한다. 다음 단계를 안내한다: `/koy-dev:feature`, `/koy-dev:build`, `/koy-dev:bugfix`, 또는 작은 수정이면 `/koy-dev:implement`.
