---
doc_id: PM-FINAL
title: 최종 보고서
phase: P8
owner: pmo
version: 0.1
status: draft
reviewers: [planner, designer, developer, qa, devops]
inputs: [pm/01_project-plan.md, pm/06_interim-report.md, devops/07_deploy-report.md, qa/05_test-report.md]
updated: YYYY-MM-DD
---

# 최종 보고서

## 1. 프로젝트 요약
| 항목 | 내용 |
|---|---|
| 프로젝트명 | |
| 고객 | |
| 운영 URL | |
| 기간 | 착수 ~ 오픈 ~ 종료 |
| 결과 한 줄 | |

## 2. 목표 달성도
| 성공 기준 (계획서 §1) | 목표 | 결과 | 달성 |
|---|---|---|---|

## 3. 범위 이행 결과
| 우선순위 | 계획 | 이행 | 이행률 | 미이행 사유 |
|---|---|---|---|---|
| Must | | | | |
| Should | | | | |
| Could | | | | |

## 4. 품질 결과
| 항목 | 목표 | 최종 결과 | 근거 문서 |
|---|---|---|---|
| TC Pass율 | 100% | | `qa/05_test-report.md` |
| 결함 (발견/해결/잔존) | 잔존 Critical·Major 0 | | |
| Lighthouse | 90+ | | |
| 운영 스모크 테스트 | 통과 | | `qa/07_smoke-test-report.md` |

## 5. 일정 실적
| 마일스톤 | 계획 | 실적 | 차이·사유 |
|---|---|---|---|

## 6. 변경 관리 이력
| CR ID | 내용 | 결정 | 영향 |
|---|---|---|---|

## 7. 인도 산출물 목록
| 구분 | 산출물 | 경로 | 버전 |
|---|---|---|---|
| 계획 | 프로젝트 계획서 | `pm/01_project-plan.md` | |
| 기획 | 요구사항 정의서 | `planning/02_requirements.md` | |
| 기획 | 정보구조 | `planning/02_information-architecture.md` | |
| 기획 | 화면정의서 | `planning/02_storyboard.md` | |
| 디자인 | 디자인 컨셉 / 시스템 / 페이지 디자인 | `design/03_*.md` | |
| 개발 | 소스코드 | `developer/site/` | |
| 개발 | 기술 설계서 / 개발 보고서 | `developer/04_*.md` | |
| 검증 | 테스트 계획·케이스·결과 | `qa/05_*.md` | |
| 배포 | 배포 계획·보고서 | `devops/07_*.md` | |
| 운영 | 운영·유지보수 가이드 | `devops/08_operation-guide.md` | |

## 8. 인수인계
- 운영 가이드: `devops/08_operation-guide.md`
- 계정·접근 정보 전달 방식: (값 기재 금지)
- 잔여 이슈·이월 결함:
- 고객 제공 대기 콘텐츠 ([TBD] 잔존):

## 9. 회고
> 상세: `shared/meetings/MTG-{YYYYMMDD}_retrospective.md`

| 구분 | 내용 |
|---|---|
| Keep (잘한 점) | |
| Problem (문제점) | |
| Try (다음 프로젝트 개선) | |

## 10. 향후 제언
- 고도화:
- 유지보수:

## 변경 이력
| 버전 | 일자 | 작성자 | 내용 | 관련 리뷰/CR |
|---|---|---|---|---|
| 0.1 | | pmo | 최초 작성 | |
