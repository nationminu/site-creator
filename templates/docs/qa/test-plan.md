---
doc_id: QA-PLAN
title: 테스트 계획서
phase: P5
owner: qa
version: 0.1
status: draft
reviewers: [developer, planner]
inputs: [planning/02_requirements.md, developer/04_tech-design.md]
updated: YYYY-MM-DD
---

# 테스트 계획서

## 1. 목적 · 범위
| 항목 | 내용 |
|---|---|
| 대상 | `developer/site/` (개발 보고서 버전: ) |
| 포함 | |
| 제외 (사유) | |

## 2. 테스트 유형
| 유형 | 내용 | 도구 · 방법 | 합격 기준 |
|---|---|---|---|
| 기능 | 수용 기준 기반 | 수동 / Playwright | 전 TC Pass |
| 콘텐츠 | 오탈자, `[TBD` 잔존, 화면정의서 문구 일치 | Grep, 육안 | 결함 0 (TBD는 목록화) |
| 링크 | 내부·외부 링크 | 링크 체커 | 깨진 링크 0 |
| 반응형 | 360 / 768 / 1280px | DevTools / Playwright | 레이아웃 붕괴 0 |
| 크로스브라우저 | Chrome · Edge · Safari · Firefox | 가능 범위 명시 | |
| 접근성 | 대비, alt, 키보드, 랜드마크, 포커스 | Lighthouse / axe | Lighthouse 접근성 90+ |
| 성능 · SEO | Lighthouse, 메타, sitemap, robots | Lighthouse CLI | 각 90+ |
| 폼 · 보안 | 입력 검증, 오류 처리, 비밀 정보 노출 | | |
| 디자인 일치 | 디자인 명세·목업 대비 | 비교 | Major 불일치 0 |

## 3. 테스트 환경
| 항목 | 내용 |
|---|---|
| OS | Windows |
| 실행 방법 | `developer/site/README.md` |
| 로컬 주소 | |
| 브라우저 · 뷰포트 | |
| 증거 저장 위치 | `qa/evidence/` |

## 4. 진입 · 종료 기준
- **진입**: G4 승인, 로컬 빌드·실행 성공
- **종료**: 전 TC 수행, Critical·Major 결함 0건, 요구사항 추적 100%

## 5. 결함 관리
- 심각도: `CLAUDE.md` §5
- 발행: `shared/tickets/DEF-{nnn}_{slug}.md`
- 흐름: open → resolved(developer) → closed / reopened(qa)

## 6. 일정 · 사이클
| 사이클 | 내용 | 비고 |
|---|---|---|
| 1차 | 전체 TC 수행 | |
| 2차 | 결함 재검증 + 회귀 | |
| 3차 | 잔여 재검증 (초과 시 PM 보고) | |

## 7. 리스크 · 제약
(도구 설치 불가, 브라우저 미보유 등 검증 한계)

## 변경 이력
| 버전 | 일자 | 작성자 | 내용 | 관련 리뷰/CR |
|---|---|---|---|---|
| 0.1 | | qa | 최초 작성 | |
