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
| 기능 | 수용 기준 기반 | Playwright (+ 수동) | 전 TC Pass |
| 콘텐츠 | 오탈자, `[TBD` 잔존, 화면정의서 문구 일치 | Grep, 육안 | 결함 0 (TBD는 목록화) |
| 링크 | 내부·외부 링크 | linkinator | 깨진 링크 0 |
| 반응형 | 360 / 768 / 1280px | Playwright 스크린샷 | 레이아웃 붕괴 0 |
| 크로스브라우저 | Chrome · Edge · Safari · Firefox | Playwright chromium · firefox · webkit(Safari 대용) + 가능 범위 명시 | 주요 흐름 Pass |
| 접근성 | 대비, alt, 키보드, 랜드마크, 포커스 | axe-core(Playwright) + Lighthouse + 키보드 수동 | axe critical·serious 0, Lighthouse 접근성 90+ |
| 성능 · SEO | Lighthouse(모바일·데스크톱), 메타, sitemap, robots | Lighthouse CLI | 각 90+ |
| 폼 · 보안 | 입력 검증, 오류 처리, 비밀 정보 노출 | | |
| 디자인 일치 | 디자인 명세·목업 대비 | 비교 | Major 불일치 0 |
| 마감 품질 | 404·메타·OG·상태·예외·긴 텍스트 (`polish-checklist.md`) | Playwright + 수동 | 누락 0 (해당 없음은 사유) |
| 문구 | 톤앤매너·맞춤법·용어 일관성 | 텍스트 추출 + 검토 | Major 오류 0 |
| 사용자 시나리오 | 핵심 전환 목표별 시나리오 2~4개 | 수동 탐색 (360·1280) | 막힘 0 |
| 프리뷰 (PM 승인 시) | 프리뷰 URL 실기기 확인 지원, noindex | Lighthouse 1회 · 링크 | PM·고객 피드백 처리 완료 |

## 3. 테스트 환경
| 항목 | 내용 |
|---|---|
| OS | (실행 OS·버전 — 예: Windows 11 / macOS 15) |
| Node.js | (`node --version`) |
| 실행 방법 | `developer/site/README.md` |
| 로컬 주소 | |
| 브라우저 · 뷰포트 | |
| 증거 저장 위치 | `qa/evidence/` |
| 검증 도구 | `qa/tools/` (Playwright·axe 버전: ) · `npx` Lighthouse·linkinator 버전: |
| 도구 미사용 항목 | (사유 · 대체 방법) |

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
