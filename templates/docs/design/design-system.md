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
| 토큰 | 값 | 용도 | 대비 (기준 배경) |
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

폰트 로딩 방식·라이선스:

### 1.3 간격 · 레이아웃
| 토큰 | 값 | 비고 |
|---|---|---|
| `--space-1` … `--space-n` | 4 / 8 / 12 / 16 / 24 / 32 / 48 / 64 px | |
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

## 2. 컴포넌트

### CMP-001 Button
| 항목 | 내용 |
|---|---|
| 변형 | primary / secondary / text |
| 크기 | sm / md / lg (높이·패딩·폰트) |
| 상태 | default / hover / focus-visible / active / disabled |
| 접근성 | 최소 44×44px, 포커스 링 명시 |
| 사용 화면 | SCR- |

(컴포넌트마다 반복: Header, Footer, Card, Form Input, Modal …)

## 3. 아이콘 · 이미지 가이드
- 아이콘 세트 (라이선스):
- 이미지 비율·톤·처리 규칙:
- 파일 형식·최적화 (WebP/AVIF, 최대 용량):

## 4. 접근성 체크
- [ ] 본문 텍스트 대비 4.5:1, 큰 텍스트 3:1 이상
- [ ] 포커스 표시가 모든 인터랙티브 요소에 명확함
- [ ] 터치 영역 44×44px 이상
- [ ] 색상만으로 정보를 전달하지 않음
- [ ] 모션은 `prefers-reduced-motion` 대응

## 변경 이력
| 버전 | 일자 | 작성자 | 내용 | 관련 리뷰/CR |
|---|---|---|---|---|
| 0.1 | | designer | 최초 작성 | |
