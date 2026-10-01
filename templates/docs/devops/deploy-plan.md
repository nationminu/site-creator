---
doc_id: OPS-PLAN
title: 배포 계획서
phase: P7
owner: devops
version: 0.1
status: draft
reviewers: [developer, qa]
inputs: [developer/04_tech-design.md, qa/05_test-report.md, pm/gates/G6_interim-report.md]   # lite에서 P6 생략 시 G6 대신 G5 + STATUS 예외 기록
updated: YYYY-MM-DD
---

# 배포 계획서

## 1. 배포 개요
| 항목 | 내용 |
|---|---|
| 배포 대상 | `developer/site/` (개발 보고서 버전: ) |
| 스택 프리셋 | static / kr-shared / react-spring / custom |
| 배포 유형 | 최초 오픈 / 재배포 (CR-) |
| 예정 일시 | |
| 운영 URL (예정) | |

## 2. 호스팅 · 인프라 선택
| 항목 | 선택 | 대안 | 근거 (요구사항 · 비용 · 고객 운영 역량) |
|---|---|---|---|
| 호스팅 | | | |
| 빌드 방식 | 호스팅 빌드 / CI / 수동 업로드 | | |
| 폼·외부 서비스 | | | |

### 2.1 서버 배치 (kr-shared · 공유 호스팅)
| 위치 | 내용 |
|---|---|
| 웹 루트 (`~/www/` 등) | Astro `dist/`, `api/index.php`, `.htaccess` |
| 웹 루트 밖 (`~/app-v{x.y.z}/`) | 앱 본체, `vendor/`, `.env`(서버에서 1회 배치) |
| 업로드 묶음 | `devops/release/<slug>-release-v{x.y.z}.zip` (SHA-256: ) |
| DB 적용 | 배포 전 백업 → `database/sql/V*.sql` 적용 순서: |

## 3. 환경 구성
| 환경 | URL | 용도 |
|---|---|---|
| 로컬 | http://localhost: | 개발 · 검증 |
| 스테이징 (선택) | | 배포 전 확인 |
| 운영 | | 고객 공개 |

## 4. 도메인 · DNS · HTTPS
| 항목 | 내용 | 담당 (PM 조치 여부) |
|---|---|---|
| 도메인 보유 · 등록처 | | |
| DNS 레코드 변경 | | |
| HTTPS 인증서 | | |

## 5. 환경 변수 · 비밀 정보
> 이름만 기록한다. 값은 절대 기록하지 않는다.

| 이름 | 용도 | 설정 위치 | 준비 상태 |
|---|---|---|---|

## 6. 배포 설정 파일 변경 내역
| 파일 | 변경 내용 | developer 리뷰 |
|---|---|---|

## 7. 배포 전 체크리스트
- [ ] G5 승인 (Critical·Major 결함 0건)
- [ ] G6 승인, 고객 피드백 CR 처리 완료 (lite에서 P6 생략 시: G5 승인 + STATUS 예외 기록의 생략 승인)
- [ ] 사이트 안 `[TBD` 0건 — 확인 명령·결과: ____ (남은 항목은 PM 예외 승인 목록 첨부)
- [ ] 법적·필수 고지(개인정보처리방침 등) 반영 확인
- [ ] 운영 빌드 로컬 성공 (명령 · 결과)
- [ ] 환경 변수 준비
- [ ] 도메인 · DNS 준비
- [ ] 롤백 절차 확인
- [ ] PM 조치 필요 사항 완료
- [ ] **PM 배포 승인** — 일시: ____

## 8. 배포 절차
| # | 작업 | 명령 · 조작 | 확인 방법 |
|---|---|---|---|
| 1 | | | |

## 9. 배포 후 확인
- devops 기본 확인: 주요 페이지 HTTP 200, HTTPS, sitemap.xml · robots.txt
- 검색엔진 등록 (`.claude/reference/kr-web-checklist.md` §4): 네이버 서치어드바이저·Google Search Console 소유 확인·sitemap 제출 — 계정 조작은 PM 조치, 확인용 메타 태그·파일은 developer 티켓
- qa 스모크 테스트 범위:

## 10. 롤백
- **롤백 판단 기준**: (예: 메인·핵심 흐름 접속 불가, Critical 결함 발견)
- **롤백 절차**:
- **판단 · 보고**: devops 판단 즉시 실행 가능 여부, PM 보고 시점

## 11. PM 조치 필요 사항
| # | 내용 (계정 생성, 결제, DNS 접근 권한, 토큰 발급 등) | 기한 | 상태 |
|---|---|---|---|

## 변경 이력
| 버전 | 일자 | 작성자 | 내용 | 관련 리뷰/CR |
|---|---|---|---|---|
| 0.1 | | devops | 최초 작성 | |
