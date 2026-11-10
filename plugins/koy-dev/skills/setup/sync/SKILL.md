---
name: sync
description: 저장소 skill을 살펴보고 step별로 어떤 skill을 쓸지 제안해, 확인받은 결과를 .claude/koy-config.md에 남긴다.
argument-hint: "(인자 없음)"
disable-model-invocation: true
---

# sync

시작할 때 `${CLAUDE_PLUGIN_ROOT}/skills/RULES.md`를 읽고 따른다. 같은 대화에서 이미 읽었으면 건너뛴다.

이 저장소에서 step마다 **어떤 저장소 skill을 쓸지** 정해 `.claude/koy-config.md`에 남긴다. 그 밖의 프로젝트 정보(검증 명령어, 디자인 원천, 코딩 규칙)는 저장소에 이미 있으므로 다루지 않는다. 양식은 아래 「koy-config 양식」.

flow나 step이 시작할 때 자동으로 부르고, 사용자가 `/koy-dev:sync`로 직접 실행할 수도 있다. 다른 skill에서 불려 왔으면 끝난 뒤 그 skill로 돌아간다.

**방향은 한쪽이다.** koy가 저장소 skill을 읽어서 연결만 한다. 저장소 skill을 고치거나 복사하지 않는다.

## 1. 저장소 skill 목록 만들기

프로젝트 루트에서 다음을 읽는다. symlink로 연결된 폴더도 따라간다.

- `.claude/skills/*/SKILL.md`: 이름, 설명, `disable-model-invocation` 여부
- `.claude/commands/**/*.md`: 명령어 이름, 설명

## 2. 비교

`.claude/koy-config.md`가 있으면 「확인한 저장소 skill」의 목록과 비교한다.

- **같으면**: "변경 없음"을 돌려주고 끝낸다. 아무것도 쓰지 않는다.
- **다르면**: 새로 생긴 skill과 사라진 skill만 3번으로 넘긴다.
- **파일이 없으면**: 모든 skill을 3번으로 넘긴다. 저장소 skill이 하나도 없으면 3번을 건너뛰고, 4번에서 「확인한 저장소 skill」에 "없음"을 쓴다.

## 3. 제안

넘어온 skill을 하나씩 보고 셋 중 하나로 분류해 제안한다.

| 분류 | 기준 | 기록할 곳 |
| --- | --- | --- |
| **연결** | step 안에서 보조로 쓸 수 있다 (정책 확인, API 명세 추출, 디자인 구현, ERD 등) | 「step별 저장소 skill」 |
| **겹침** | step 하나와 같은 일을 한다 (팀의 기획·구현·리뷰 명령어 등) | 「겹침」. 묻지 않고 저장소 skill을 쓴다 |
| **무관** | koy flow와 상관없다 (주간 보고, 워크트리 관리 등) | 기록하지 않음 |

연결은 step, 「언제」, 「부르는 방법」(`disable-model-invocation: true`이거나 명령어 파일이면 `파일 읽기`와 경로, 아니면 `Skill tool`)을 함께 제안한다. 겹침은 그 저장소 skill이 문서를 어디에 쓰는지 찾아 함께 적는다. 사라진 skill은 표에서 지우자고 제안한다.

```
step별 저장소 skill (제안)
  resting   /swagger-extract   API를 쓰는 작업      Skill tool
  implement /figma-to-ui       Figma 컴포넌트 구현   Skill tool
겹침 (저장소 skill을 씀)
  grill+resting ↔ /plan-feature
무관: /weekly-report, /create-worktree
→ 이대로 / 고치기
```

사용자가 확인할 때까지 고친다.

## 4. 쓰기

확인되면 `.claude/koy-config.md`를 쓴다. 갱신일 때는 바뀐 줄만 고치고, 사람이 손으로 쓴 메모는 지우지 않는다. 「확인한 저장소 skill」에 오늘 날짜와 1번의 skill 이름 목록을 쓴다.

파일을 처음 만들 때는 git에 올릴지 묻는다. koy를 쓰지 않는 팀원이 있으면 `.git/info/exclude`에 추가하는 쪽을 추천한다.

## koy-config 양식

`.claude/koy-config.md`는 이 형식을 따른다.

```markdown
# koy 설정: <프로젝트 이름>

## step별 저장소 skill
| step | 저장소 skill | 언제 | 부르는 방법 |
| --- | --- | --- | --- |
| resting | /swagger-extract | API를 쓰는 작업 | Skill tool |
| implement | /dev-checklist | 구현 전 체크 | 파일 읽기: .claude/commands/dev-checklist.md |

## 겹침
| step | 겹치는 저장소 skill | 저장소 skill의 문서 위치 |
| --- | --- | --- |
| grill | /development:grill | .docs/aviko-be/works/grill/ |

## 확인한 저장소 skill
- 날짜: 2026-11-05
- skill: swagger-extract, figma-to-ui, plan-feature, ...
```

## 5. 종료

바뀐 내용을 짧게 보고한다. 다른 skill에서 불려 왔으면 그 skill로 돌아간다.
