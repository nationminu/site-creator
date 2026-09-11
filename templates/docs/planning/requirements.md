---
doc_id: PLN-REQ
title: 요구사항 정의서
phase: P2
owner: planner
version: 0.1
status: draft
reviewers: [designer, developer, qa]
inputs: [pm/requests/CR-000_initial-request.md, pm/01_project-plan.md]
updated: YYYY-MM-DD
---

# 요구사항 정의서

## 1. 개요
| 항목 | 내용 |
|---|---|
| 사이트 목적 | |
| 대상 사용자 | |
| 핵심 전환 목표 | 예: 문의 접수, 예약, 자료 다운로드 |

## 2. 사용자 정의
| 사용자 유형 | 목표 | 주요 행동 | 사용 환경 (모바일/PC 비중) |
|---|---|---|---|

## 3. 기능 요구사항 (REQ-F)
| ID | 요구사항 | 상세 설명 | 우선순위 | 출처 | 수용 기준 |
|---|---|---|---|---|---|
| REQ-F-001 | | | Must | CR-000 | Given … When … Then … |

## 4. 비기능 요구사항 (REQ-N)
`CLAUDE.md` §6 기본 품질 기준 기반. 고객 요구에 따라 조정한다.

| ID | 분류 | 요구사항 | 기준값 | 우선순위 | 수용 기준 |
|---|---|---|---|---|---|
| REQ-N-001 | 반응형 | 모든 페이지가 주요 뷰포트에서 레이아웃 붕괴 없이 표시 | 360 / 768 / 1280px | Must | 가로 스크롤·요소 겹침 0건 |
| REQ-N-002 | 브라우저 | 최신 주요 브라우저 지원 | Chrome·Edge·Safari·Firefox | Must | |
| REQ-N-003 | 접근성 | 웹 접근성 준수 | WCAG 2.1 AA | Must | 대비 4.5:1, 모든 이미지 alt, 키보드 탐색 가능 |
| REQ-N-004 | 성능 | 빠른 로딩 | Lighthouse 성능 90+ | Should | |
| REQ-N-005 | SEO | 검색 노출 기본 설정 | Lighthouse SEO 90+ | Must | 페이지별 title/description, OG, sitemap.xml, robots.txt |
| REQ-N-006 | 보안 | 안전한 전송·입력 처리 | HTTPS | Must | |

## 5. 콘텐츠 요구사항 (REQ-C)
| ID | 콘텐츠 | 위치 (SCR) | 제공 주체 (고객/제작) | 확보 상태 | 비고 |
|---|---|---|---|---|---|
| REQ-C-001 | | | | 확보 / 미확보 | |

## 6. 우선순위 요약 (MoSCoW)
| 우선순위 | 건수 | ID |
|---|---|---|
| Must | | |
| Should | | |
| Could | | |
| Won't (이번 범위 제외) | | |

## 7. 고객 확인 필요 사항 [TBD]
| # | 항목 | 관련 ID | 필요 시점 | 비고 |
|---|---|---|---|---|

## 변경 이력
| 버전 | 일자 | 작성자 | 내용 | 관련 리뷰/CR |
|---|---|---|---|---|
| 0.1 | | planner | 최초 작성 | |
