# 진행 요약 (틀 저장소)

> site-creator 틀(규칙·에이전트·스킬·템플릿)의 변경 내역을 한눈에 보는 요약입니다. **최신이 위**에 오며, 틀 수정 커밋마다 추가합니다 (`CLAUDE.md` §0 "틀 수정 요청", 형식은 `.claude/reference/git-ops.md` §3).
> 구분: `추가` · `수정` · `삭제` — 상세는 `git log`를 봅니다.

### 2026-10-01

* 수정: 전체 구조 리팩터링 — 규칙을 기준 문서 `.claude/reference/`(modes · stack-presets · design-profile · kr-web-checklist · git-ops)로 분리하고 CLAUDE.md를 원칙 중심으로 축소, 에이전트는 필요한 절·기준 문서만 읽도록 변경.
* 수정: lite 주 검토자를 산출물 단위로 재정의(P8 최종 보고서 devops · 운영 가이드 developer)해 자기 검토 제거, lite P6 생략 시 G6 의존 제거, 시안 수 모드 연동, lite 게이트 문서 간소판.
* 추가: P1 devops 호스팅·운영 비용 사전 의견, P4 구현 작업 단위별 호출·커밋, designer 디자인 QA용 스크린샷(구현·목업 쌍) 절차.
* 추가: 국내 실무 — 콘텐츠 수급 계획·PM 사전 준비 항목, 법적·필수 고지 REQ-C 점검, 배포 전 `[TBD` 0건 조건, 네이버 서치어드바이저 등 검색엔진 등록.
* 추가: 진행 요약 `ACTIVITY.md` 도입 — 프로젝트 루트(오케스트레이터가 커밋마다 기록)와 틀 저장소에 날짜별·최신순 요약. 팀별 `WORKLOG.md`는 상세 과정 기록으로 계속 유지.
* 수정: lite 모드 교차 검토 호출에 `sonnet` 모델 지정. 작성·개발·결함 수정·배포 호출은 세션 기본 모델 유지, 판단이 갈리면 기본 모델로 1회 재검토.
* 수정: 진행 모드 기본값을 `lite`로 변경. `--standard`로 시작하거나 대규모 신호가 있으면 G1에서 standard 전환 권고.
* 추가: 스택 프리셋 `static` · `kr-shared` · `react-spring` · `custom` 도입. P1 잠정 → G2 PM 확정(ADR), 이후 변경은 CR. 프리셋별 서버 배치·배포·롤백·QA 환경 정의.
* 추가: 개발자 규칙·기술 스택 카탈로그 — 프론트 Astro/Next.js/Nuxt, 백엔드 Spring Boot/Laravel·CI4/Django, 표준 명령 매핑(install~audit), 백엔드 보안 기본, 자체 점검 후 qa 전달.
* 추가: 디자인 프로필(스타일 체계 Tailwind, Claude Design 반입, Design Sync 게시) 선택 규칙. 기준은 승인된 산출물, 외부 도구 업로드는 PM 승인.
* 추가: lite 진행 모드, QA 표준 검증 도구(Lighthouse·Playwright·axe·linkinator), 보호 영역 `Write` 규칙·Bash 쓰기 금지 명시, 실행 환경 Windows/macOS 공통화.

### 2026-09-12

* 수정: 프로젝트 저장소를 게이트 단위가 아니라 작업 단위(에이전트 호출 1회·병렬 배치)마다 커밋하도록 변경.

### 2026-09-11

* 추가: 세션 재개 스킬 `/resume-project` — 파일·git 상태로 재개 지점 판정 후 PM 확인을 거쳐 이어서 진행.
* 추가: PM용 상세 사용 가이드 `USAGE.md`.
* 추가: 멀티 에이전트 홈페이지 제작 틀 초기 구성 — 6개 팀 에이전트, 8단계·게이트 프로세스, 스킬(kickoff·run-phase·status·change-request), 산출물 템플릿.
