# 프로젝트 현황판

> 프로젝트 상태의 기준 문서. pmo 또는 오케스트레이터가 갱신하며, 사실만 기록한다.

| 항목 | 내용 |
|---|---|
| 프로젝트 ID | `{project-slug}` |
| 프로젝트명 | {프로젝트 표시명} |
| 고객 | {고객명} |
| 프로젝트 상태 | **진행 중** (진행 중 / 보류 / 종료 / 유지보수) |
| 진행 모드 | `{mode}` (lite 기본 / standard — CLAUDE.md §2 "진행 모드") |
| 스택 프리셋 | `TBD` (static / kr-shared / react-spring / custom — CLAUDE.md §2 "스택 프리셋", P1 잠정 → G2 확정) |
| 로컬 개발 환경 | `docker` (기본 — hybrid / native는 PM 결정, `.claude/reference/environments.md`, G2 확정) |
| 운영 환경 | `TBD` (static-hosting / shared-hosting / paas / docker-vm / k8s / linux-native — P1 잠정 → G2 확정) |
| 스타일 체계 | `css-vars` (css-vars / tailwind — CLAUDE.md §2 "디자인 프로필", G2 전 결정) |
| Claude Design | `off` (반입 사용 on/off) |
| Design Sync | `off` (G4 이후 PM 결정) |
| 프리뷰 | 없음 (URL · 만료일 — `devops/05_preview.md`) |
| 운영 URL · 릴리스 | 미배포 (URL · `release-v…`) |
| 오픈 후 관찰 | — (대기: YYYY-MM-DD 확인 예정 / 기록 완료) |
| 보류 사유 | — |
| 현재 단계 | **P1 계획** — 착수 |
| 틀 버전 | `{framework-commit}` |
| 최종 갱신 | YYYY-MM-DD |

## 1. 단계 진행 현황
상태: ⬜ 대기 · 🔄 진행 중 · 👀 검토 중 · ⏳ 승인 대기 · ✅ 완료 · ↩️ 회귀(재작업) · ⏸️ 보류 · ➖ 생략(lite)

| 단계 | 상태 | Owner | 주요 산출물 | 게이트 | PM 승인일 |
|---|---|---|---|---|---|
| P1 계획 | ⬜ | pmo | `pm/01_project-plan.md` | G1 | - |
| P2 기획 | ⬜ | planner | `planning/02_*.md` | G2 | - |
| P3 디자인 | ⬜ | designer | `design/03_*.md`, `design/mockups/` | G3 | - |
| P4 개발 | ⬜ | developer | `developer/04_*.md`, `developer/site/` | G4 | - |
| P5 검증(로컬) | ⬜ | qa | `qa/05_*.md` | G5 | - |
| P6 중간보고 | ⬜ | pmo | `pm/06_interim-report.md` | G6 | - |
| P7 배포(운영) | ⬜ | devops | `devops/07_*.md`, `qa/07_smoke-test-report.md` | G7 | - |
| P8 최종 산출물 | ⬜ | pmo | `pm/08_final-report.md`, `devops/08_operation-guide.md` | G8 | - |

## 1-1. TBD·콘텐츠 수급
| 구분 | 건수 / 내용 | 기한 | 비고 |
|---|---|---|---|
| 산출물 `[TBD` 잔여 | 0 | | |
| 사이트 안 `[TBD` 잔여 (배포 전 0) | 0 | | |
| 미답변 고객 질문 (QNA `open`) | 0 (기한 지난 것: 없음) | | |
| 미수급 콘텐츠 | 없음 | | |
| PM 사전 준비 미완료 | 없음 | | |

## 2. PM 결정 필요 사항
| # | 내용 | 관련 문서 | 요청일 |
|---|---|---|---|
| - | 없음 | | |

## 3. 열린 티켓·결함
| ID | 제목 | From → To | 우선순위/심각도 | 상태 |
|---|---|---|---|---|
| - | 없음 | | | |

## 4. 리스크·이슈
| ID | 내용 | 가능성/영향 | 대응 | 담당 |
|---|---|---|---|---|
| - | 없음 | | | |

## 4-1. 제안 (SUG)
| SUG ID | 제목 | 제안자 | 유형 | 상태 | 처리 (반영 위치 / CR / DEF) | 사유·재검토 시점 (보류·미반영) |
|---|---|---|---|---|---|---|
| - | 없음 | | | | | |

## 5. 변경 요청 (CR)
| CR ID | 제목 | 상태 | 회귀 단계 | 사유·재검토 시점 (보류·거절) |
|---|---|---|---|---|
| - | 없음 | | | |

## 6. 다음 할 일
- [ ] P1 계획 단계 실행 (kickoff에서 자동 진행)

## 7. 예외 기록 (게이트 미승인 진행 등)
| 일자 | 내용 | PM 지시 |
|---|---|---|
