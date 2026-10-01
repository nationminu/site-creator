---
doc_id: DSN-PAGE
title: 페이지 디자인 명세
phase: P3
owner: designer
version: 0.1
status: draft
reviewers: [planner, developer]
inputs: [planning/02_storyboard.md, design/03_design-system.md]
updated: YYYY-MM-DD
---

# 페이지 디자인 명세

## 공통
| 요소 | Desktop | Tablet | Mobile | 컴포넌트 |
|---|---|---|---|---|
| Header | | | | CMP- |
| Footer | | | | CMP- |
| 컨테이너·그리드 | | | | |

## 화면 대응표
| SCR ID | 화면명 | 목업 | 상태 |
|---|---|---|---|
| SCR-001 | | `design/mockups/scr-001.html` | 완료 / 진행 중 |

---

## SCR-001 {화면명}
- 화면정의: `planning/02_storyboard.md` SCR-001
- 목업: `design/mockups/scr-001.html`

### 섹션별 명세
| # | 섹션 | Desktop | Tablet | Mobile | 컴포넌트 | 토큰 · 수치 | 인터랙션 · 모션 |
|---|---|---|---|---|---|---|---|
| 1 | Hero | | | | CMP- | 높이, 여백, 폰트 토큰 | |

### 이미지 자리 명세
| 자리 (섹션) | 비율 · 크기 (@1x/@2x) | 핵심 영역 (잘려도 남아야 할 부분) | 내용 · 톤 | 조달 (고객 / 스톡 / 제작 / AI — PM 승인) | 파일 · 출처 | 상태 |
|---|---|---|---|---|---|---|
| Hero | 16:9 · 1920×1080 | 중앙 인물 | | 고객 | `assets/…` · `SOURCES.md` | 확보 / [TBD: Q-xxx] |

### 개발 전달 노트
-

---

(SCR마다 위 블록 반복)

## 개발 전달 패키지 (G3 전 완료)
- [ ] 토큰 (디자인 시스템 §1.6 CSS 변수, §1.7 Tailwind 테마 해당 시)
- [ ] 웹폰트 파일 또는 CDN 지정, 라이선스
- [ ] 아이콘 SVG (`design/assets/icons/`)
- [ ] 이미지 (자리 명세 규격, WebP/AVIF + 원본), 미확보는 플레이스홀더 + `[TBD: Q-xxx]`
- [ ] 파비콘 세트, 기본 OG 이미지(1200×630), SCR별 OG(화면정의서 지정 시)
- [ ] `assets/SOURCES.md` (모든 외부 자료 출처·라이선스)

## 관리자 화면 (해당 시)
| SCR ID | 화면 | 목업 / 명세만 | 사용 컴포넌트 |
|---|---|---|---|

## 상태 · 예외 화면 (`.claude/reference/polish-checklist.md` §1·§2)
| 대상 | 상태 | 명세·목업 | 해당 없음 사유 |
|---|---|---|---|
| 404 페이지 | | | |
| 폼 | 입력 오류 · 전송 중 · 성공 · 실패 | | |
| 목록 | 빈 상태 | | |
| 이미지 | 로딩 전 · 없음 | | |

## 시각 점검 (스크린샷 자기 점검)
> 스크린샷: `design/evidence/mockups/scr-{nnn}-{360|768|1280}.jpg` (오케스트레이터 캡처)

| SCR | 폭 | 발견한 문제 (위계·여백·정렬·넘침·대비·일관성) | 조치 | 재캡처 |
|---|---|---|---|---|

## 변경 이력
| 버전 | 일자 | 작성자 | 내용 | 관련 리뷰/CR |
|---|---|---|---|---|
| 0.1 | | designer | 최초 작성 | |
