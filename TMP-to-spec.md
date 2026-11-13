# to-spec 정리

출처: mattpocock/skills `skills/engineering/to-spec/SKILL.md` (로컬: `~/Koy/skills/skills/engineering/to-spec/SKILL.md`)

## 한 줄 정의

> 지금까지의 대화를 spec으로 바꿔 이슈 트래커에 올린다. 인터뷰 없이, 이미 나눈 이야기를 정리만 한다.

## 위치

- 보통 grill 다음에 씀. grill에서 정한 것을 새 세션에서도 읽을 수 있는 문서로 남김
- 다음은 `to-tickets`(나누기) → `implement-spec`(구현)
- 사용자에게 다시 묻지 않음. 이미 아는 것을 정리만 함

## 절차

1. **저장소 파악**: 아직 안 봤으면 저장소를 훑어 현재 상태를 이해함. spec 전체에 저장소 용어집의 용어를 쓰고, 건드리는 영역의 ADR을 따름
2. **테스트 seam 잡기**
   - 기존 seam이 새 seam보다 나음
   - 가장 높은 seam을 씀
   - 새 seam이 필요하면 가능한 한 높은 곳에 제안함
   - seam은 적을수록 좋고, 이상적인 개수는 하나임
   - 이 seam이 사용자 기대와 맞는지 확인받음
3. **spec 쓰기**: 아래 양식으로 써서 이슈 트래커에 올림. `ready-for-agent` 라벨을 붙임

## spec 양식

| 절 | 내용 |
| --- | --- |
| Problem Statement | 사용자가 겪는 문제. 사용자 관점 |
| Solution | 해결책. 사용자 관점 |
| User Stories | 길고 번호 붙은 목록. "<누구>로서 <기능>을 원한다. <이유> 때문이다" 형식. 기능의 모든 면을 아주 넓게 다룸 |
| Implementation Decisions | 정한 구현 결정: 만들거나 고칠 모듈, 바뀌는 인터페이스, 기술적 설명, 아키텍처 결정, 스키마 변경, API 계약, 구체적인 상호작용 |
| Testing Decisions | 좋은 테스트의 기준(겉 동작만 테스트하고 구현 세부는 테스트하지 않음), 테스트할 모듈, 참고할 기존 테스트(prior art) |
| Out of Scope | 이 spec에서 다루지 않는 것 |
| Further Notes | 그 밖의 메모 |

## 규칙 하나

- **파일 경로와 코드 조각은 넣지 않음.** 금방 낡기 때문임
- 예외: prototype이 만든 코드 조각이 글보다 결정을 더 정확하게 담고 있으면(상태 머신, reducer, 스키마, 타입 모양) 해당 결정 안에 넣고, prototype에서 왔다고 짧게 적음. 결정이 담긴 부분만 남기고 줄임

## 없는 것

코드와 DB 대조, 근거와 출처 규칙, 비슷한 기능 비교, 빈틈 찾기, 놓친 점 점검, 설계 대안 비교, 리뷰
