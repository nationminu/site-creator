---
doc_id: PLN-IA
title: 정보구조 (IA)
phase: P2
owner: planner
version: 0.1
status: draft
reviewers: [designer, developer, qa]
inputs: [planning/02_requirements.md]
updated: YYYY-MM-DD
---

# 정보구조 (IA)

## 1. 사이트맵
```
홈 (SCR-001)
├── 회사 소개 (SCR-002)
│   ├── …
├── …
└── 문의 (SCR-00x)
```

## 2. 메뉴 구조
### 2.1 GNB (주 메뉴)
| 1depth | 2depth | SCR ID | URL | 비고 |
|---|---|---|---|---|

### 2.2 Footer 메뉴
| 항목 | 링크 대상 | 비고 |
|---|---|---|

### 2.3 기타 (유틸 메뉴, 플로팅 버튼 등)

## 3. 페이지 목록
| SCR ID | 페이지명 | URL | 페이지 유형 (메인/목록/상세/폼/정적) | 관련 REQ | 우선순위 |
|---|---|---|---|---|---|

## 4. 주요 사용자 흐름
### Flow-1: (예: 첫 방문자 → 서비스 확인 → 문의)
| 단계 | 화면 | 사용자 행동 | 다음 화면 |
|---|---|---|---|
| 1 | SCR-001 | | |

## 5. 공통 요소
| 요소 | 노출 위치 | 구성 | 관련 REQ |
|---|---|---|---|
| 헤더 | 전 페이지 | 로고, GNB, (모바일) 햄버거 메뉴 | |
| 푸터 | 전 페이지 | 회사 정보, 약관·개인정보처리방침 링크 | |

## 6. URL · SEO 규칙
- URL 규칙: 소문자 영문 kebab-case, 예) `/about`, `/services/web-design`
- title 패턴: `{페이지명} | {사이트명}`
- description 작성 기준:
- 404 페이지:

## 변경 이력
| 버전 | 일자 | 작성자 | 내용 | 관련 리뷰/CR |
|---|---|---|---|---|
| 0.1 | | planner | 최초 작성 | |
