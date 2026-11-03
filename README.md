# koy

Koy의 Claude Code plugin 마켓플레이스.

| plugin | 설명 |
| --- | --- |
| [`koy-dev`](./plugins/koy-dev/README.md) | 공통 개발 프로세스: 기능 개발·과제·버그 흐름과 그 단계들. 프로젝트별 차이는 `.claude/koy-config.md`로 맞춘다 |

## 설치

```bash
claude plugin marketplace add <github-계정>/koy     # 로컬 개발 중이면: ~/Koy/koy
claude plugin install koy-dev@koy
```

외부 마켓플레이스는 자동 업데이트가 기본으로 꺼져 있다. 새 버전은 `claude plugin update koy-dev@koy`로 받거나, `/plugin` → Marketplaces 탭에서 자동 업데이트를 켠다.

## 저장소 구조

```
.claude-plugin/marketplace.json   마켓플레이스 정의. 배포할 plugin 목록과 위치
plugins/koy-dev/                  배포되는 plugin. 사용자 컴퓨터에는 이 폴더만 복사된다
ROADMAP.md                        관리자용 메모 (배포 안 됨)
```

## 고치고 배포하기

1. `plugins/koy-dev/` 아래를 고친다.
2. `plugins/koy-dev/.claude-plugin/plugin.json`의 `version`을 올린다.
3. `claude plugin validate . --strict`와 `claude plugin validate plugins/koy-dev --strict`로 확인한다.
4. push한다.

피드백은 GitHub Issues로 받는다. 각 사용자의 `/koy-dev:retro`가 공통 skill 개선 제안을 이슈 초안으로 만들어준다.
