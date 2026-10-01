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

### 이미지 · 에셋
| 파일 | 크기 · 비율 | 형식 | 출처 · 라이선스 | 상태 |
|---|---|---|---|---|
| | | | `design/assets/SOURCES.md` | 확보 / [TBD: 고객 제공 필요] |

### 개발 전달 노트
-

---

(SCR마다 위 블록 반복)

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
