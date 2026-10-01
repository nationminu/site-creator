---
doc_id: DSN-SYSTEM
title: 디자인 시스템
phase: P3
owner: designer
version: 0.1
status: draft
reviewers: [planner, developer]
inputs: [design/03_design-concept.md]
updated: YYYY-MM-DD
---

# 디자인 시스템

## 1. 디자인 토큰

### 1.1 컬러
| 토큰 | 값 | 용도 | 대비 (기준 배경) — designer 계산 / developer 검증 |
|---|---|---|---|
| `--color-primary` | `#` | 주요 CTA, 링크 | `#FFFFFF` 대비 x.x:1 |
| `--color-text` | `#` | 본문 | |
| `--color-bg` | `#` | 기본 배경 | |
| `--color-error` | `#` | 오류 상태 | |

### 1.2 타이포그래피
| 토큰 | font-family | size (desktop / mobile) | weight | line-height | 용도 |
|---|---|---|---|---|---|
| `--font-h1` | | / | | | 페이지 제목 |
| `--font-body` | | / | | | 본문 |

**한글 조판 규칙**
| 항목 | 규칙 |
|---|---|
| 본문 행간 | 1.6~1.8 (제목 1.2~1.4) |
| 자간 | 본문 0 ~ -0.01em, 큰 제목 -0.02em 내외 |
| 줄바꿈 | `word-break: keep-all` (한글 단어 단위), 긴 URL·영문은 `overflow-wrap: anywhere` |
| 최소 본문 크기 | 모바일 16px 이상 |

**웹폰트 전략**
| 항목 | 내용 |
|---|---|
| 폰트·라이선스 | 예: Pretendard (OFL) — 출처를 `assets/SOURCES.md`에 기록 |
| 사용 굵기 | 최대 3종 (예: 400·600·700) |
| 형식·경량화 | woff2, 한글 subset(또는 dynamic subset), 자체 호스팅 / CDN |
| 로딩 | `font-display: swap`, 주요 굵기 preload, 대체 폰트 스택 |

### 1.3 간격 · 레이아웃
| 토큰 | 값 | 비고 |
|---|---|---|
| `--space-1` … `--space-n` | 4 / 8 / 12 / 16 / 24 / 32 / 48 / 64 px | 간격은 4px 배수만 사용 (테두리·폰트·행간 제외) |
| `--container-max` | | |
| 그리드 | desktop 12col · tablet 8col · mobile 4col / gutter | |

### 1.4 Radius · Shadow · Motion
| 토큰 | 값 | 용도 |
|---|---|---|

### 1.5 Breakpoint
| 이름 | 범위 |
|---|---|
| mobile | ~ 767px |
| tablet | 768px ~ 1279px |
| desktop | 1280px ~ |

### 1.6 CSS 변수 (개발 전달용)
```css
:root {
  /* color */
  --color-primary: #000000;
  /* typography */
  /* spacing */
  /* radius, shadow, motion */
}
```

### 1.7 Tailwind 테마 (스타일 체계 `tailwind`인 경우만 — 아니면 "해당 없음")
Tailwind 버전: v4 (기술 설계에서 변경 시 개정) · §1.1~1.6 값과 1:1 일치
```css
@import "tailwindcss";

@theme {
  /* color — 브랜드 색 우선 */
  --color-primary: #000000;
  /* typography */
  --font-sans: "", sans-serif;
  /* spacing — 4px 기준 (p-1=4px, p-2=8px …) */
  --spacing: 0.25rem;
  /* radius, shadow */
  /* breakpoint — §1.5와 일치 */
  --breakpoint-md: 48rem;   /* 768px */
  --breakpoint-xl: 80rem;   /* 1280px */
}
```
- 임의 값 유틸리티(`p-[13px]` 등) 사용 금지. 필요한 값은 토큰으로 추가한다.

## 2. 컴포넌트

### CMP-001 Button
| 항목 | 내용 |
|---|---|
| 변형 | primary / secondary / text |
| 크기 | sm / md / lg (높이·패딩·폰트) |
| 상태 | default / hover / focus-visible / active / disabled |
| 접근성 | 최소 44×44px, 포커스 링 명시 |
| Tailwind 클래스 (해당 시) | 예: `inline-flex min-h-11 px-4 py-2 rounded-md bg-primary text-white hover:… focus-visible:ring-2` |
| 사용 화면 | SCR- |

### CMP-00x Form Input (폼이 있으면 필수 — 요구사항 §3.3 폼 정의와 연결)
| 항목 | 내용 |
|---|---|
| 구성 | 레이블(항상 표시), 필수 표시(*와 텍스트), 입력 칸, 도움말, 오류 문구 |
| 상태 | default / focus-visible / filled / error / disabled / success(제출 완료 화면) |
| 오류 표시 | 위치(입력 칸 아래), 색 + 아이콘 + 문구(색만으로 전달 금지), 스크린리더 연결(`aria-describedby`) |
| 동의 체크 | 체크박스 + 동의 문구 + 처리방침 링크, 미체크 오류 |
| 제출 버튼 | 전송 중(중복 제출 방지) 상태 |

(컴포넌트마다 반복: Header, Footer, Card, 목록·페이지네이션 …)

### 관리자 UI (관리자 화면이 있을 때)
관리자 화면은 공개 화면처럼 개별 디자인하지 않는다. **공통 최소 세트**로 만든다.
| 구성 | 내용 |
|---|---|
| 레이아웃 | 좌측 메뉴 + 상단 바 + 본문 1단, 데스크톱 우선(모바일은 깨지지 않는 수준) |
| 컴포넌트 | 표(정렬·페이지네이션), 폼(CMP Form Input 재사용), 버튼, 알림·확인 대화상자, 상태 배지 |
| 토큰 | 공개 사이트 토큰 재사용(브랜드 색은 포인트에만) |
| 목업 | 대표 화면 2개(목록·등록/수정)만, 나머지는 명세 |

## 3. 아이콘 · 이미지 가이드
- 아이콘 세트 (라이선스, SVG):
- 이미지 비율·톤·처리 규칙 (색감, 인물·배경, 보정):
- 파일 형식·최적화 (WebP/AVIF, 최대 용량, @1x·@2x):
- **이미지 조달 원칙**: 고객 제공 우선 → 라이선스가 확인된 스톡(출처·라이선스 기록) → 제작. **AI 생성 이미지는 PM이 사용을 승인한 경우에만** 쓰고 출처에 생성 도구를 적는다. 자리별 규격·조달은 페이지 디자인 "이미지 자리 명세".
- 파비콘(16·32·180·512, SVG)·기본 OG 이미지(1200×630) 디자인을 포함한다.

## 4. 접근성 체크
- [ ] 본문 텍스트 대비 4.5:1, 큰 텍스트 3:1 이상
- [ ] 포커스 표시가 모든 인터랙티브 요소에 명확함
- [ ] 터치 영역 44×44px 이상
- [ ] 색상만으로 정보를 전달하지 않음
- [ ] 모션은 `prefers-reduced-motion` 대응

## 4-1. 대비 검증 (P3 검토 시 developer가 스크립트로 계산)
| 전경 토큰 | 배경 토큰 | designer 값 | 검증 값 | 기준 (4.5 / 3) | 결과 |
|---|---|---|---|---|---|

## 4-2. 디자인 QA 판정 기준 (P4 — 구현 vs 명세·목업)
| 차이 | 등급 |
|---|---|
| 토큰 밖 색·폰트·굵기 사용, 임의 값 | Major (Must) |
| 360 폭 가로 넘침·요소 겹침, 화면 붕괴 | Major (Must) |
| 컴포넌트 상태(포커스·오류) 누락 | Major (Must) |
| 명세에 없는 화면 요소·동작 | Must (`CLAUDE.md` §8) |
| 간격이 토큰과 4px 넘게 다름, 정렬 어긋남 | Minor (Should) |
| 이미지 비율·자르기 차이 | Minor (Should) |
| 실제 콘텐츠 차이로 인한 높이·줄 수 변화 | 지적 안 함 (콘텐츠 반영 결과) |

## 5. 외부 디자인 도구 반입 내역 (Claude Design `on`인 경우)
| 반입 경로 (`design/imports/…`) | 일시 | 반영 위치 | 정규화 내용 (토큰 매핑·제외 항목) |
|---|---|---|---|

## 변경 이력
| 버전 | 일자 | 작성자 | 내용 | 관련 리뷰/CR |
|---|---|---|---|---|
| 0.1 | | designer | 최초 작성 | |
