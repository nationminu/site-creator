---
doc_id: DEV-TECH
title: 기술 설계서
phase: P4
owner: developer
version: 0.1
status: draft
reviewers: [devops, qa]
inputs: [planning/02_requirements.md, planning/02_information-architecture.md, design/03_design-system.md, design/03_page-design.md]
updated: YYYY-MM-DD
---

# 기술 설계서

## 1. 기술 관점 요구사항 요약
| REQ ID | 요구사항 | 기술적 고려 사항 |
|---|---|---|

## 2. 기술 스택
> 기준: `CLAUDE.md` §6 "기술 스택 표준", `.claude/agents/developer.md` "스택 표준 카탈로그". 카탈로그 밖 선택은 ADR 필요.

### 2.1 선택 과정
| 순서 | 검토 항목 | 결과 |
|---|---|---|
| 1 | 외부 서비스로 충분한가 (폼·CMS·예약·결제) | |
| 2 | 고객 제약 (운영 언어·기존 시스템·호스팅) — 관련 REQ-N | |
| 3 | 요구 기능 (동적 기능 범위) | |
| 4 | 운영 비용·난이도 | |
| 5 | 틀 기본값과 다른 경우 그 사유 | |

### 2.2 선택 결과
| 영역 | 선택 | 검토한 대안 | 선택 근거 |
|---|---|---|---|
| 프론트엔드 | Astro / Next.js / Nuxt / 순수 HTML(예외) | | |
| 화면 렌더링 방식 | 프론트 + API 분리 / 백엔드 템플릿 서버 렌더링 / 정적만 | | |
| 백엔드 | 없음 / Spring Boot / Laravel / CodeIgniter 4 / Django | | |
| DB | 없음 / PostgreSQL / MySQL·MariaDB | | |
| 스타일링 | CSS 변수 / Tailwind (디자인 프로필) | | |
| 폼 처리 | | | |
| 이미지 최적화 | | | |
| 분석·외부 연동 | | | |
| 호스팅 전제 | | | |

고객 운영 관점 (콘텐츠 수정 난이도, 비용):

### 2.3 버전 · 지원 종료 (EOL)
| 구성 요소 | 버전 (정확히) | 지원 유형 (LTS 등) | EOL 일자 | 확인 출처 |
|---|---|---|---|---|
| Node.js (프론트 빌드) | | | | |
| 프론트엔드 프레임워크 | | | | |
| 백엔드 런타임 (JDK / PHP / Python) | | | | |
| 백엔드 프레임워크 | | | | |
| DB | | | | |

### 2.4 의존성
| 패키지 | 용도 | 라이선스 | 필요 이유 (대안 대비) |
|---|---|---|---|

## 3. 디렉토리 구조
```
developer/site/
├── README.md        # 설치·실행·빌드 방법 (표준 명령 매핑 포함)
├── .env.example
├── compose.yaml     # (백엔드 시) 로컬 DB
└── …                # 분리 구성이면 web/ (프론트) · api/ (백엔드)
```

## 4. 화면 · 컴포넌트 매핑
디자인 토큰 파일: (예: `src/styles/tokens.css` — 단일 출처)

| SCR / CMP | 구현 파일 | 비고 |
|---|---|---|

## 5. 외부 연동 · 환경 변수
| 이름 | 용도 | 필수 | 비고 (값 기록 금지) |
|---|---|---|---|

## 6. 로컬 개발 환경
| 항목 | 내용 |
|---|---|
| 요구 런타임 | (2.3 버전 표 기준 — Node / JDK / PHP·Composer / Python·uv) |
| 로컬 DB | (Docker Compose / 대안과 운영 DB 차이) |
| 개발 서버 주소 | 프론트: · 백엔드: |
| 빌드 출력 디렉토리 | |

### 6.1 표준 명령 매핑
> qa·devops는 이 표만 보고 실행한다. Windows·macOS 양쪽에서 동작해야 한다 (Gradle은 Windows에서 `gradlew.bat`).

| 표준 명령 | 프론트엔드 | 백엔드 | 비고 |
|---|---|---|---|
| install | | | |
| dev | | | |
| build | | | |
| preview | | | |
| test | | | |
| lint | | | |
| check | | | lint + 타입 + 테스트 |
| migrate | | | |
| seed (테스트 데이터) | | | |
| audit | | | |

## 7. 품질 구현 방안
| 항목 | 구현 방법 |
|---|---|
| 반응형 | |
| 접근성 | |
| SEO (메타, OG, sitemap, robots) | |
| 성능 (이미지, 폰트, 번들) | |
| 보안 (입력 검증, 비밀 정보) | |
| 백엔드 보안 (CSRF, 인증·권한, 비밀번호 해시, 업로드 검증, rate limit, 오류 비노출, 로그 개인정보) | |
| API 문서 (OpenAPI) | |
| 테스트 전략 (단위·통합·주요 엔드포인트) | |

## 8. 구현 순서
| # | 작업 | 관련 SCR/REQ |
|---|---|---|
| 1 | 프로젝트 초기화, 디자인 토큰 적용, 공통 레이아웃 | |

## 9. 기술 리스크
| 리스크 | 영향 | 대응 |
|---|---|---|

## 변경 이력
| 버전 | 일자 | 작성자 | 내용 | 관련 리뷰/CR |
|---|---|---|---|---|
| 0.1 | | developer | 최초 작성 | |
