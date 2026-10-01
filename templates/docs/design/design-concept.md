---
doc_id: DSN-CONCEPT
title: 디자인 컨셉
phase: P3
owner: designer
version: 0.1
status: draft
reviewers: [planner, developer]
inputs: [planning/02_requirements.md, planning/02_storyboard.md, pm/01_project-plan.md, pm/requests/CR-000_initial-request.md, pm/requests/QNA.md]
updated: YYYY-MM-DD
---

# 디자인 컨셉

## 1. 분석
| 항목 | 내용 |
|---|---|
| 고객 요청의 톤·선호 | |
| 고객 참고 사이트 · 선호/비선호 스타일 | (QNA Q ID · SUG ID — 참고할 점과 피할 점) |
| 벤치마킹 요약 | (요구사항 §1.2 — 구조·콘텐츠 관점 요지) |
| 대상 사용자 특성 | |
| 업종·경쟁사 시각 특징 (출처) | |
| 브랜드 자산 (로고·컬러 보유 여부) | |
| 디자인 목표 (한 문장) | |

## 2. 시안

> **시안 공통 범위**: 모든 시안은 같은 범위 — **메인 첫 화면 + 대표 섹션 1개** — 를 만들고 **화면정의서의 실제 문구**를 쓴다(Lorem 금지, 미확정은 `[TBD: Q-xxx]`). 1280·360 두 폭 스크린샷을 아래에 넣어 이 문서만으로 **고객에게 그대로 공유**할 수 있게 한다.

### 시안 A — {컨셉 키워드}
| 항목 | 내용 |
|---|---|
| 키워드 | |
| 무드 설명 | |
| 컬러 팔레트 | Primary `#` · Secondary `#` · Neutral `#` · Accent `#` |
| 타이포그래피 | 제목: {폰트} · 본문: {폰트} (라이선스: ) |
| 레이아웃·그래픽 스타일 | |
| 레퍼런스 | (URL · 참고 포인트) |
| 대표 목업 | `design/mockups/concept-a.html` |
| 스크린샷 | ![시안 A 1280](evidence/mockups/concept-a-1280.jpg) ![시안 A 360](evidence/mockups/concept-a-360.jpg) |
| 장점 | |
| 고려사항 | |

### 시안 B — {컨셉 키워드}
(시안 A와 동일 형식)

### 시안 C — {컨셉 키워드} (선택)

## 3. 시안 비교 및 권고
| 기준 | A | B | C |
|---|---|---|---|
| 요구사항·목적 부합 | | | |
| 대상 사용자 적합성 | | | |
| 브랜드 일관성 | | | |
| 구현 난이도 / 일정 영향 | | | |
| 접근성 (대비 등) | | | |

**디자인팀 권고안**:
근거:

## 4. PM 선택 결과
- 선택 시안: (시안이 1개면 "승인 / 수정 요청")
- 수정·조합 요청:
- 결정 기록: `shared/decisions/ADR-{nnn}_design-concept.md`

## 5. 시안 수정 회차 (계획서 §5.2 피드백 정책)
| 회차 | 일자 | 요청자 (PM·고객 / SUG ID) | 수정 내용 | 정책 내 여부 (n / 허용 m회) |
|---|---|---|---|---|

> 허용 횟수를 넘는 수정은 일정·비용 영향을 보고하고 PM이 진행 여부를 결정한다.

## 변경 이력
| 버전 | 일자 | 작성자 | 내용 | 관련 리뷰/CR |
|---|---|---|---|---|
| 0.1 | | designer | 최초 작성 | |
