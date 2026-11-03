# koy-config 사용 규칙

koy의 모든 skill이 공통으로 따르는 규칙. 각 SKILL.md가 이 파일을 가리킨다.

## 읽기

프로젝트의 `.claude/koy-config.md`를 읽는다.

- 파일이 없으면: 흐름 skill과 resting은 멈추고 `/koy-dev:sync`를 먼저 실행하라고 안내한다. 나머지 단계 skill은 그 사실을 한 줄로 알리고 기본값(산출물 위치 `.scratch/`)으로 진행한다.
- "없음"인 항목은 건너뛰고, 건너뛴 사실을 보고서에 남긴다.

## 겹침 결정

「겹침 결정」에서 지금 단계가 **저장소 우선**으로 되어 있으면:

- 단독 실행이면: 그 저장소 skill을 쓰라고 안내하고 멈춘다.
- 흐름 안이면: 흐름 skill이 그 저장소 skill을 대신 실행한다 (흐름 실행 규칙 참고).

## 단계별 연결 skill

「단계별 연결 skill」에 지금 단계와 연결된 skill이 있으면, 「언제」에 맞는 시점에 함께 쓴다.

- `Skill tool`: Skill tool로 호출한다.
- `파일 읽기`: 적힌 경로의 SKILL.md(또는 명령어 파일)를 읽고 그대로 따른다.

## 사람만 실행하는 명령

「사람만 실행하는 명령」에 있는 명령은 실행하지 않는다. 실행할 명령과 확인할 점을 사용자에게 넘기고, 결과를 알려달라고 한다.

## 저장소 지문

sync와 흐름 skill이 같은 방법으로 계산한다. 프로젝트 루트에서 실행한다.

```bash
{ find -L .claude/skills .claude/commands -type f -name '*.md' 2>/dev/null | LC_ALL=C sort | xargs shasum 2>/dev/null; shasum package.json 2>/dev/null; } | shasum | cut -c1-12
```

`-L`은 symlink로 연결된 skill 폴더까지 따라간다.

## koy 버전

이 skill의 기준 폴더(Base directory)에서 세 단계 위가 koy-dev plugin 루트다. 그 아래 `.claude-plugin/plugin.json`의 `version`을 읽는다.
