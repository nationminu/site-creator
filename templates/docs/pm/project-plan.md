---
doc_id: PM-PLAN
title: 프로젝트 계획서
phase: P1
owner: pmo
version: 0.1
status: draft
reviewers: [planner, developer]
inputs: [pm/requests/CR-000_initial-request.md]
updated: YYYY-MM-DD
---

# 프로젝트 계획서

## 1. 프로젝트 개요
| 항목 | 내용 |
|---|---|
| 프로젝트명 | |
| 고객 | |
| 사이트 유형 | 기업 소개 / 랜딩 / 포트폴리오 / 쇼핑몰 / 커뮤니티 / 기타 |
| 비즈니스 목표 | |
| 주요 대상 사용자 | |
| 성공 기준 (측정 가능) | 예: 문의 폼 전환, Lighthouse 90+, 오픈 일자 준수 |

## 2. 범위
### 2.1 포함 (In-scope)
-

### 2.2 제외 (Out-of-scope)
-

### 2.3 가정 및 제약
-

### 2.4 고객 확인 필요 사항
| # | 질문 (고객에게 그대로 물을 수 있게) | 필요 이유 | 영향 단계 | 확인 기한 |
|---|---|---|---|---|
| 1 | | | | |

## 3. 산출물 목록
| 단계 | 산출물 | Owner | 검토자 |
|---|---|---|---|
| P1 | `pm/01_project-plan.md` | pmo | planner, developer |
| P2 | `planning/02_requirements.md`, `02_information-architecture.md`, `02_storyboard.md` | planner | designer, developer, qa |
| P3 | `design/03_design-concept.md`, `03_design-system.md`, `03_page-design.md`, `mockups/` | designer | planner, developer |
| P4 | `developer/04_tech-design.md`, `site/`, `04_dev-report.md` | developer | devops, qa / designer, planner |
| P5 | `qa/05_test-plan.md`, `05_test-cases.md`, `05_test-report.md` | qa | developer, planner |
| P6 | `pm/06_interim-report.md` | pmo | 전 팀 |
| P7 | `devops/07_deploy-plan.md`, `07_deploy-report.md`, `qa/07_smoke-test-report.md` | devops | developer, qa |
| P8 | `pm/08_final-report.md`, `devops/08_operation-guide.md` | pmo, devops | 전 팀 |

프로젝트 특성에 따른 조정 사항(추가·생략 산출물과 사유):
-

## 4. 일정 및 WBS
| 단계 | 주요 작업 | Owner | 시작 | 종료 | 마일스톤 |
|---|---|---|---|---|---|
| P1 계획 | 요청 분석, 계획 수립 | pmo | | | G1 |
| P2 기획 | 요구사항, IA, 화면정의 | planner | | | G2 |
| P3 디자인 | 컨셉 시안 → PM 선택 → 디자인 시스템·페이지 디자인·목업 | designer | | | 컨셉 선택, G3 |
| P4 개발 | 기술 설계 → 구현 → 로컬 빌드 확인 | developer | | | G4 |
| P5 검증 | 테스트 → 결함 수정 → 재검증 | qa | | | G5 |
| P6 중간보고 | 고객 보고, 피드백 반영 | pmo | | | G6 |
| P7 배포 | 배포 계획 → PM 승인 → 배포 → 스모크 테스트 | devops | | | 오픈, G7 |
| P8 최종 | 운영 가이드, 최종 보고, 회고 | pmo | | | G8 |

## 5. 조직 및 커뮤니케이션
- 의사결정: 총괄 PM (게이트 승인, 컨셉 선택, 배포 승인, CR 승인)
- 보고 방식: 게이트 보고, `/status` 현황판
- 에스컬레이션 기준: 리뷰 3라운드 초과, 팀 간 의견 충돌, 일정 지연 예상, 범위 변경 필요

## 6. 품질 목표
`CLAUDE.md` §6 기본 품질 기준을 따른다. 프로젝트 추가·예외 사항:
-

## 7. 기술·환경 초기 방향 (확정은 P4 기술 설계, P7 배포 계획)
| 항목 | 초기 방향 | 근거 |
|---|---|---|
| 사이트 형태 | 정적 / 동적 | |
| 호스팅 후보 | | |
| 도메인 | 보유 / 신규 / TBD | |
| 외부 연동 | 폼·지도·분석·SNS 등 | |

## 8. 리스크 관리
| ID | 리스크 | 가능성(H/M/L) | 영향(H/M/L) | 대응 방안 | 담당 |
|---|---|---|---|---|---|
| RSK-001 | | | | | |

## 변경 이력
| 버전 | 일자 | 작성자 | 내용 | 관련 리뷰/CR |
|---|---|---|---|---|
| 0.1 | | pmo | 최초 작성 | CR-000 |
