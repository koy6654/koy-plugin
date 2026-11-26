# koy-dev

Koy의 개발 방법론을 담은 Claude Code plugin.

## 설치

```bash
claude plugin marketplace add koy6654/claude-plugins-public
claude plugin install koy-dev@koy-public
```

새 버전은 `claude plugin update koy-dev@koy-public`로 받는다.

## skill

주로 flow 단위로 실행한다. 개별 step 사용도 가능하다.

| 구분 | 명령어             | 하는 일                                                                |
| ---- | ------------------ | ---------------------------------------------------------------------- |
| flow | `/koy-dev:feature` | 기능 개발과 초기 구축: scope → spec → split, ticket마다 build → verify |
| flow | `/koy-dev:bugfix`  | 버그 수정: debug → build → verify                                      |
| step | `/koy-dev:scope`   | 요구사항 정리                                                          |
| step | `/koy-dev:spec`    | 구현 명세                                                              |
| step | `/koy-dev:split`   | 새 컨텍스트 하나에 끝나는 ticket으로 나누기                            |
| step | `/koy-dev:build`   | 승인된 계획대로 구현                                                   |
| step | `/koy-dev:verify`  | 커밋 전 검증                                                           |
| step | `/koy-dev:debug`   | 버그 원인 분석                                                         |
| step | `/koy-dev:retro`   | 회고                                                                   |
| step | `/koy-dev:close`   | 커밋한 뒤 저장소 반영, ADR, scratch 정리                               |

### feature

scope → spec → split → build → verify

### bugfix

debug → build → verify

## 무엇을 쓸까

```

기능 개발: /koy-dev:feature <요청>
버그 수정: /koy-dev:bugfix <증상>
커밋한 뒤: /koy-dev:retro → /koy-dev:close

```

## 저장소에 생기는 파일

```

.scratch/ 작업 중 결정과 진행을 남기는 임시 기록

```
