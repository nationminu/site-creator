---
doc_id: QA-TC
title: 테스트 케이스
phase: P5
owner: qa
version: 0.1
status: draft
reviewers: [developer, planner]
inputs: [planning/02_requirements.md, planning/02_storyboard.md, design/03_page-design.md]
updated: YYYY-MM-DD
---

# 테스트 케이스

## 1. 요구사항 추적 매트릭스 (RTM)
> 모든 REQ가 이 표에 있어야 한다. TC가 없는 REQ는 "미추적"으로 표시하고 사유를 적는다.

| REQ ID | 요구사항 | 우선순위 | SCR | CMP | 구현 위치 | TC ID | 최종 결과 |
|---|---|---|---|---|---|---|---|
| REQ-F-001 | | Must | SCR-001 | CMP- | | TC-001, TC-002 | Pass / Fail / 미추적 |

**커버리지**: 전체 REQ N건 / 추적 N건 / 미추적 N건

## 2. 테스트 케이스
결과: `Pass` · `Fail` · `Block`(실행 불가) · `N/A`(해당 없음)

| TC ID | 관련 REQ / SCR | 유형 | 사전 조건 | 절차 | 기대 결과 | 1차 | 2차 | 3차 | 결함 |
|---|---|---|---|---|---|---|---|---|---|
| TC-001 | REQ-F-001 / SCR-001 | 기능 | | 1. … 2. … | | | | | DEF- |

## 3. 비기능 점검 체크리스트
| 항목 | 대상 | 1차 | 2차 | 3차 | 증거 |
|---|---|---|---|---|---|
| 반응형 360px | 전 페이지 | | | | |
| 반응형 768px | 전 페이지 | | | | |
| 반응형 1280px | 전 페이지 | | | | |
| Lighthouse 성능 | 주요 페이지 | | | | |
| Lighthouse 접근성 | 주요 페이지 | | | | |
| Lighthouse SEO | 주요 페이지 | | | | |
| 깨진 링크 | 전체 | | | | |
| 콘솔 에러 | 전 페이지 | | | | |
| `[TBD` 잔존 | 전체 | | | | |

## 변경 이력
| 버전 | 일자 | 작성자 | 내용 | 관련 리뷰/CR |
|---|---|---|---|---|
| 0.1 | | qa | 최초 작성 | |
