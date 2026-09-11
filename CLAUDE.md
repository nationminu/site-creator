# Site Creator — 멀티 에이전트 홈페이지 제작 프레임워크

고객이 원하는 홈페이지를 **여러 팀(에이전트)의 협업**으로 계획·기획·디자인·개발·검증·배포한다.
이 문서는 프레임워크 헌장이며, 오케스트레이터(메인 세션)와 모든 에이전트가 따르는 최상위 규칙이다.

---

## 0. 틀과 프로젝트의 분리 (가장 먼저 지킬 원칙)

```
site-creator/                    ← Git ① 틀 저장소: 에이전트·규칙·템플릿 (보호 영역)
├── CLAUDE.md  README.md  .gitignore  .gitattributes
├── .claude/     agents/ · skills/ · settings.json
├── templates/
│   ├── project/                 ← /kickoff 때 복사되는 프로젝트 골격
│   └── docs/{팀}/               ← 산출물 문서 템플릿
└── projects/                    ← 틀 저장소에서 git 제외
    └── <project-slug>/          ← Git ② 프로젝트 저장소: 모든 산출물 (프로젝트별 독립)
```

### 경로 표기
- `{PROJECT}` = `projects/<project-slug>` (작업 대상 프로젝트 루트). 오케스트레이터는 모든 에이전트 호출에 `PROJECT: projects/<slug>`를 명시한다.
- `{PROJECT}/…` 로 표기된 경로는 프로젝트 산출물, 접두어가 없는 `CLAUDE.md`, `templates/…`, `.claude/…`는 틀 저장소 경로다.
- 프로젝트 **문서 내부**에 쓰는 경로(예: `inputs:`)는 `{PROJECT}` 기준 상대경로로 적는다 (예: `planning/02_requirements.md`).

### 프로젝트 ID (slug) 규칙
- 영문 소문자·숫자·하이픈(kebab-case), 3~40자, 예: `acme-corp-homepage`
- 한글 프로젝트명·고객명은 `{PROJECT}/README.md`와 `{PROJECT}/pm/STATUS.md`에 표시명으로 기록한다.
- 같은 slug의 폴더가 이미 있으면 새로 만들지 않는다.

### 보호 영역 (틀)
`CLAUDE.md`, `README.md`, `.gitignore`, `.gitattributes`, `.claude/**`, `templates/**`

| 상황 | 규칙 |
|---|---|
| **프로젝트 작업 중** (kickoff·단계 실행·CR·현황 조회 등) | 보호 영역을 **수정·삭제·생성하지 않는다.** 쓰기는 `{PROJECT}/` 안에서만 한다. |
| **다른 프로젝트** | `projects/<다른 slug>/`는 읽기만 가능 (참고용). 수정·삭제 금지. |
| **틀 수정 요청** (사용자가 에이전트·스킬·규칙·템플릿 변경을 명시적으로 요청) | 요청 범위만 수정 → 변경 요약 보고 → 틀 저장소에 커밋 (`framework: …`). |
| **프로젝트 중 틀 개선 아이디어** | 틀을 고치지 않고 회고록(`{PROJECT}/shared/meetings/…_retrospective.md`) "틀 개선 제안"에 기록만 한다. |
| **프로젝트에만 필요한 문서 양식** | 템플릿을 고치지 않고 `{PROJECT}` 안에 문서를 추가한다. |

- 명령 실행(npm, 빌드, 테스트 등)은 반드시 `{PROJECT}` 하위 디렉토리에서 한다. 틀 루트에 `package.json`, `node_modules` 등을 만들지 않는다.
- `.claude/settings.json`의 권한 규칙이 보호 영역 편집 시 사용자 확인을 요구한다. 확인 요청이 뜨면 프로젝트 작업 중에는 **거절이 기본**이다.

---

## 1. 조직과 역할 (R&R)

| 역할 | 주체 | 산출물 위치 | 정의 파일 | 핵심 책임 |
|---|---|---|---|---|
| 총괄 PM | **사용자** | `{PROJECT}/pm/requests/` | — | 고객 요청 전달, 게이트 승인, 최종 의사결정 |
| 오케스트레이터 | 메인 Claude 세션 | — | 이 문서 | 프로젝트 생성, 단계 진행, 에이전트 호출, 리뷰 루프 조율, PM 보고, **git 커밋** |
| PMO | `pmo` | `{PROJECT}/pm/` | `.claude/agents/pmo.md` | 계획서, 현황판, 게이트 문서, 중간·최종 보고서 |
| 기획팀 | `planner` | `{PROJECT}/planning/` | `.claude/agents/planner.md` | 요구사항, 정보구조(IA), 화면정의서 |
| 디자인팀 | `designer` | `{PROJECT}/design/` | `.claude/agents/designer.md` | 디자인 컨셉, 디자인 시스템, 페이지 디자인, 목업 |
| 개발팀 | `developer` | `{PROJECT}/developer/` | `.claude/agents/developer.md` | 기술 설계, 사이트 구현(`developer/site/`), 개발 보고 |
| 품질검증팀 | `qa` | `{PROJECT}/qa/` | `.claude/agents/qa.md` | 테스트 계획·케이스·결과, 결함 관리 |
| 배포운영팀 | `devops` | `{PROJECT}/devops/` | `.claude/agents/devops.md` | 빌드·배포 환경, 운영 배포, 운영 가이드 |
| 공용 소통 | 전 팀 | `{PROJECT}/shared/` | `{PROJECT}/shared/README.md` | 티켓, 리뷰, 결정 기록, 회의록 |

> **구조적 제약**: 서브에이전트는 다른 서브에이전트를 직접 호출할 수 없다.
> 따라서 팀 간 소통은 **파일(산출물·리뷰·티켓)** 로만 이루어지고, 에이전트 호출과 순서 조율은 **오케스트레이터**가 담당한다.
>
> **오케스트레이터는 팀 산출물을 직접 작성하지 않는다.** 반드시 해당 팀 에이전트에게 위임한다.
> (예외: 프로젝트 골격 생성, `pm/STATUS.md` 갱신, CR 요청 원문 기록, 게이트 문서의 PM 결정 기입, 승인 후 산출물 헤더의 status/version 갱신)

---

## 2. 프로세스 (8단계 + 게이트)

```
P1 계획 ─G1→ P2 기획 ─G2→ P3 디자인 ─G3→ P4 개발 ─G4→ P5 검증(로컬) ─G5→ P6 중간보고 ─G6→ P7 배포(운영) ─G7→ P8 최종 산출물 ─G8→ 종료
              ▲                                                                   │
              └────────────── 변경 요청(CR) 발생 시 영향 받는 가장 앞 단계로 회귀 ──────────┘
```

아래 산출물 경로는 모두 `{PROJECT}` 기준이다.

| 단계 | Owner | 산출물 | 교차 검토자 | 게이트 통과 기준(요약) |
|---|---|---|---|---|
| **P1 계획** | pmo | `pm/01_project-plan.md` | planner, developer | 범위·일정·산출물·리스크 확정 |
| **P2 기획** | planner | `planning/02_requirements.md`<br>`planning/02_information-architecture.md`<br>`planning/02_storyboard.md` | designer, developer, qa | 모든 요구사항에 ID·우선순위·수용 기준 존재, 모든 Must REQ가 화면/비기능 항목과 연결 |
| **P3 디자인** | designer | `design/03_design-concept.md`<br>`design/03_design-system.md`<br>`design/03_page-design.md`<br>`design/mockups/` | planner, developer | PM 컨셉 선택 완료, 전 화면(SCR) 디자인 명세 완료, 구현 가능성 확인 |
| **P4 개발** | developer (+devops) | `developer/04_tech-design.md`<br>`developer/site/`<br>`developer/04_dev-report.md` | 설계: devops, qa<br>구현: designer, planner | 로컬 빌드·실행 성공, Must 요구사항 구현 완료 |
| **P5 검증(로컬)** | qa | `qa/05_test-plan.md`<br>`qa/05_test-cases.md`<br>`qa/05_test-report.md`<br>`shared/tickets/DEF-*` | developer, planner | Critical·Major 결함 0건, 요구사항 추적 100% |
| **P6 중간보고** | pmo | `pm/06_interim-report.md` | 전 팀(사실 확인) | PM(고객) 승인, 피드백은 CR로 등록 |
| **P7 배포(운영)** | devops | `devops/07_deploy-plan.md`<br>`qa/07_smoke-test-report.md`<br>`devops/07_deploy-report.md` | developer, qa | **배포 전 PM 명시 승인**, 운영 스모크 테스트 통과 |
| **P8 최종 산출물** | pmo (+devops) | `pm/08_final-report.md`<br>`devops/08_operation-guide.md`<br>`shared/meetings/MTG-*_retrospective.md` | 전 팀 | 인도 산출물 목록 완비, 인수인계 완료, 회고 기록 |

**단계 내 체크포인트**
- **P3**: 컨셉 시안(2~3안)을 먼저 PM에게 제시해 방향을 선택받은 뒤 상세 디자인에 착수한다.
- **P4**: ① 기술 설계 작성·검토 → ② 구현 → ③ 로컬 빌드 확인·개발 보고서 → ④ 디자인/기능 검토.
- **P5**: qa는 G3 승인 이후(P4 진행 중) 테스트 계획·케이스를 미리 작성할 수 있다. 결함 수정 ↔ 재검증은 최대 3사이클, 초과 시 PM 보고.
- **P7**: 배포 계획 검토 → **PM 배포 승인** → 릴리스 태그 → 배포 실행 → qa 운영 스모크 테스트 → 배포 보고서.
- **P8**: PM이 고객 인도 범위를 결정하면 인도 패키지를 만든다 (§9).

---

## 3. 단계 실행 표준 루프

```
① 착수 점검 → ② 작성 → ③ 교차 검토(병렬) → ④ 반영 ─┬→ ⑤ 게이트 준비 → ⑥ PM 승인 → ⑦ 커밋·태그
                              ▲                 │
                              └── Must 잔존 시 ──┘  (최대 3라운드, 초과 시 PM 결정)
```

1. **착수 점검** — `{PROJECT}/pm/STATUS.md`에서 이전 게이트 승인 확인, 입력 산출물이 `approved`인지 확인, 관련 open 티켓·CR 확인.
2. **작성** — Owner 에이전트 호출. 산출물은 `status: in-review`로 제출.
3. **교차 검토** — 검토자 에이전트를 **한 번에 병렬 호출**. 각자 `{PROJECT}/shared/reviews/`에 리뷰를 작성.
4. **반영** — `수정 요청` 판정이 있으면 Owner 재호출. Owner는 리뷰 문서의 모든 지적에 처리 결과(반영/부분 반영/미반영+사유)를 기입하고 버전을 올린다. Must를 지적한 검토자만 다음 라운드 재검토.
5. **수렴** — 3라운드 후에도 Must가 남거나 팀 간 의견이 충돌하면, pmo가 `{PROJECT}/shared/decisions/ADR-*`에 쟁점·선택지·권고를 정리하고 PM 결정을 요청.
6. **게이트 준비** — pmo 호출: `{PROJECT}/pm/gates/G{n}_{slug}.md` 작성, `STATUS.md` 갱신(`승인 대기`).
7. **PM 승인** — 오케스트레이터가 PM에게 게이트 보고. 승인 시 산출물 `status: approved`, `version: 1.0`. 반려 시 지시를 게이트 문서에 기록하고 ④로 복귀.
8. **커밋·태그** — 승인 반영 후 오케스트레이터가 프로젝트 저장소에 커밋하고 게이트 태그를 단다 (§9).

### 에이전트 호출 규칙 (오케스트레이터)
호출 프롬프트에는 반드시 포함한다:
- `PROJECT: projects/<slug>`
- 단계와 작업 종류 (작성 / 검토 / 반영 / 결함 수정 / 영향도 의견 등)
- 입력 문서 경로, 템플릿 경로(`templates/docs/…`), 출력 경로(`{PROJECT}/…`)
- 관련 리뷰·티켓·CR 경로, 리뷰 라운드 번호
- (P7 배포 실행 시에만) `PM 배포 승인 완료: YYYY-MM-DD HH:MM`, 배포 대상 태그

### 완료 보고 형식 (모든 에이전트)
```
## 완료 보고
- 프로젝트: projects/<slug>
- 작업: (단계 · 작업 종류)
- 생성/수정 파일: (경로 목록 — 모두 {PROJECT} 안이어야 함)
- 핵심 내용 요약: (3~5줄)
- 발행/처리한 티켓: TKT-… / DEF-… / 없음
- 리스크·가정:
- PM 결정 필요 사항: … / 없음
```
오케스트레이터는 보고된 파일 중 `{PROJECT}` 밖의 경로가 있으면 즉시 PM에게 알린다.

---

## 4. 산출물 규칙

### 문서 헤더 (모든 단계 산출물 공통)
```yaml
---
doc_id: PLN-REQ            # 팀코드-문서코드 (PM, PLN, DSN, DEV, QA, OPS)
title: 요구사항 정의서
phase: P2
owner: planner
version: 0.1               # 초안 0.x → 게이트 승인 시 1.0 → 이후 변경 1.1, 1.2 …
status: draft              # draft | in-review | revision | approved | superseded
reviewers: [designer, developer, qa]
inputs: [pm/01_project-plan.md]   # {PROJECT} 기준 상대경로
updated: YYYY-MM-DD
---
```
문서 마지막에는 항상 **변경 이력** 표(버전·일자·작성자·내용·관련 리뷰/CR)를 둔다.

### 파일 명명
- 단계 산출물: `{단계번호 2자리}_{kebab-case}.md` (예: `02_requirements.md`)
- 파일·폴더명은 영문 kebab-case, 문서 본문은 한국어.
- 새 산출물은 반드시 템플릿(`templates/docs/{팀}/`, 공용 `templates/docs/shared/`)을 **읽어서 `{PROJECT}` 안에 새 파일로** 작성한다. 템플릿 파일 자체는 수정하지 않는다.

### 작업 로그 (진행 과정 기록)
각 팀은 작업이 끝날 때마다 `{PROJECT}/{팀}/WORKLOG.md`에 항목을 추가한다(요청·수행·산출물·티켓·다음 할 일).
**산출물이 "결과"라면 WORKLOG는 "과정"이다.**

### 추적 ID 체계 (프로젝트마다 독립 번호)
| 대상 | 형식 | 부여 주체 |
|---|---|---|
| 고객 요청·변경 요청 | `CR-000`(최초 요청), `CR-001` … | 오케스트레이터 |
| 요구사항 | `REQ-F-001`(기능) / `REQ-N-001`(비기능) / `REQ-C-001`(콘텐츠) | planner |
| 화면 | `SCR-001` | planner |
| 컴포넌트 | `CMP-001` | designer |
| 테스트 케이스 | `TC-001` | qa |
| 팀 간 티켓 | `TKT-{발행팀}-001` | 발행 팀 |
| 결함 | `DEF-001` | qa |
| 결정 기록 | `ADR-001` | pmo |
| 리스크 | `RSK-001` | pmo |
| 게이트 | `G1` ~ `G8` | pmo |

- ID는 재사용하지 않는다. 삭제 시 행을 지우지 말고 `폐기`로 표시한다.
- **추적성**: REQ → SCR → CMP → 구현 파일 → TC 로 연결을 유지한다 (`qa/05_test-cases.md`의 추적 매트릭스가 최종 확인처).

---

## 5. 소통 규칙

경로는 모두 `{PROJECT}` 기준이다.

| 수단 | 위치 · 파일명 | 템플릿 | 언제 |
|---|---|---|---|
| 리뷰 | `shared/reviews/{단계}_{대상}_{검토자}_r{라운드}.md` | `templates/docs/shared/review.md` | 교차 검토 (대상 문서가 여러 개면 한 파일로 묶어도 됨) |
| 티켓 | `shared/tickets/TKT-{발행팀}-{nnn}_to-{수신팀}_{slug}.md` | `templates/docs/shared/ticket.md` | 리뷰 주기 밖의 요청·질의·자료 요청 |
| 결함 | `shared/tickets/DEF-{nnn}_{slug}.md` | `templates/docs/shared/defect.md` | 검증 중 발견된 결함 |
| 결정 기록 | `shared/decisions/ADR-{nnn}_{slug}.md` | `templates/docs/shared/decision.md` | PM 결정, 팀 간 합의, 되돌리기 어려운 선택 |
| 회의록 | `shared/meetings/MTG-{YYYYMMDD}_{slug}.md` | `templates/docs/shared/meeting.md` | 킥오프, 이슈 조율, 회고 |

### 쓰기 권한
- 모든 에이전트는 틀과 프로젝트 전체를 **읽을 수 있다.**
- **쓰기는 `{PROJECT}/{자기 팀}/` + `{PROJECT}/shared/tickets/`, `{PROJECT}/shared/reviews/`** 로 제한한다. (`shared/decisions/`, `shared/meetings/`는 pmo)
- **타 팀 산출물을 직접 수정하지 않는다.** 수정이 필요하면 리뷰 지적 또는 티켓으로 요청한다.
- 티켓의 "처리 결과"는 수신 팀이, 리뷰의 "처리 결과" 열은 산출물 Owner가 기입한다.
- 예외: devops는 배포 설정 파일(`vercel.json`, `netlify.toml`, CI 워크플로 등)을 `{PROJECT}` 안에 작성할 수 있다. 변경 내역을 배포 계획서에 기록하고 developer 리뷰를 받는다.
- `{PROJECT}/pm/requests/`의 **요청 원문 절은 수정 금지**(보존). 해석·정리는 별도 절이나 계획서·요구사항 문서에서 한다.
- **보호 영역(§0)과 다른 프로젝트는 어떤 에이전트도 수정하지 않는다.**

### 리뷰 지적 등급
| 등급 | 의미 | 게이트 영향 |
|---|---|---|
| **Must** | 반드시 수정 — 요구사항 누락·오류, 구현 불가, 품질 기준 미달 | 미해결 시 게이트 차단 |
| **Should** | 수정 권장 | 미반영 시 사유 기재 필수 |
| **Could** | 제안·아이디어 | 선택 |

리뷰 판정: `승인` / `조건부 승인`(Should 이하만 존재) / `수정 요청`(Must 존재)

### 결함 심각도
| 등급 | 기준 |
|---|---|
| Critical | 사이트 접속·핵심 흐름 불가, 보안·개인정보 문제 |
| Major | 주요 기능 오동작, 주요 화면 레이아웃 붕괴 |
| Minor | 부분 기능 오류, 디자인 명세와 경미한 불일치 |
| Trivial | 오탈자, 미세한 스타일 차이 |

---

## 6. 기본 품질 기준 (고객이 달리 요구하지 않는 한)
- **반응형**: 모바일 360px / 태블릿 768px / 데스크톱 1280px 이상
- **브라우저**: Chrome, Edge, Safari, Firefox 최신 버전
- **웹 접근성**: WCAG 2.1 AA (KWCAG 2.2) 수준 — 대체 텍스트, 명도 대비 4.5:1, 키보드 탐색, 포커스 표시
- **성능·SEO**: Lighthouse 성능·접근성·권장사항·SEO 각 90점 이상 목표
- **SEO 기본**: title/description, OG 태그, sitemap.xml, robots.txt, 시맨틱 마크업
- **보안**: HTTPS, 비밀 정보 저장소 커밋 금지, 폼 입력 검증, 개인정보 수집 시 처리방침 고지

planner는 이 기준을 `REQ-N-*` 비기능 요구사항으로 구체화한다.

---

## 7. 변경 관리 (CR)
1. PM이 변경 요청 전달 → 오케스트레이터가 `{PROJECT}/pm/requests/CR-{nnn}_{slug}.md`에 원문 기록
2. 영향도 분석 — planner·developer가 영향 의견 제출 → pmo가 종합(영향 REQ/SCR/산출물, 회귀 단계, 일정, 리스크, 권고)
3. PM 결정 → 커밋 (`cr(CR-nnn): …`)
4. 승인 시 영향 받는 **가장 앞 단계부터 회귀**하여 영향 부분만 개정 (버전 증가, 변경 이력에 CR ID 기록)
5. 이후 단계는 영향 범위 중심으로 재검토·재검증, 재승인 시 태그 `G{n}-CR-{nnn}`

---

## 8. 안전 규칙 (반드시 준수)
- **운영 배포, 도메인·DNS 변경, 외부 서비스 계정 생성·유료 결제, 원격 저장소 생성·push, 고객 데이터 외부 전송**은 PM의 명시적 승인 후에만 수행한다. 이전 승인은 다음 작업으로 이월되지 않는다.
- API 키·비밀번호 등 비밀 정보는 `.env` 등으로 분리하고 문서·소스·로그·커밋에 기록하지 않는다.
- 고객 요청에 없는 기능을 임의로 추가하지 않는다. 제안은 `Could` 우선순위나 티켓으로 올린다.
- 확인되지 않은 사실(고객 정보, 연락처, 수치, 연혁 등)은 지어내지 않고 `[TBD: 고객 확인 필요]`로 표시한다.
- 외부 이미지·폰트·코드는 상업적 사용 가능 라이선스만 사용하고 출처를 기록한다.
- 확인하지 않은 것을 "완료"·"통과"로 보고하지 않는다. 실행 증거(명령·결과)를 남긴다.
- 보호 영역(§0)은 프로젝트 작업 중 수정하지 않는다.

---

## 9. Git 운영

두 저장소는 완전히 독립이다. 틀 저장소의 `.gitignore`가 `projects/*`를 제외한다 (submodule 사용 안 함).

| 구분 | ① 틀 저장소 (`./`) | ② 프로젝트 저장소 (`projects/<slug>/`) |
|---|---|---|
| 내용 | 에이전트, 스킬, 규칙, 템플릿 | 해당 프로젝트의 모든 산출물·소스·소통 기록 |
| 생성 | 최초 1회 | `/kickoff` 때 `git init` |
| 커밋 주체 | 오케스트레이터 | **오케스트레이터만** (에이전트는 커밋하지 않음 — 병렬 작업 시 잠금 충돌 방지) |
| 커밋 시점 | 사용자 요청으로 틀을 수정한 뒤 | **변경이 생길 때마다** (아래 표) |
| 원격 push | PM이 지시할 때만 | **PM이 지시할 때만** |

### 프로젝트 저장소 커밋 시점 — 작업 단위마다
산출물이 바뀌면 그때그때 커밋한다. 한 작업 단위 = **에이전트 1회 호출**(병렬 호출이면 그 배치 전체)이며, 배치가 끝난 뒤 한 번 커밋한다. 커밋하지 않은 채 다음 에이전트를 호출하지 않는다.

| 작업 단위 | 커밋 메시지 | 태그 |
|---|---|---|
| kickoff 골격 생성 + CR-000 기록 | `chore(kickoff): 프로젝트 생성 및 고객 요청 기록` | `kickoff` |
| 산출물 작성 | `docs({단계}): {문서} v{버전} 작성 — {팀}` | — |
| 리뷰 반영 | `docs({단계}): {문서} v{버전} R{n} 리뷰 반영 — {팀}` | — |
| 교차 검토 (병렬 배치 1커밋) | `review({단계}): {대상} R{n} — {검토자들}` | — |
| 구현 | `feat(P4): {요약} — developer` | — |
| 결함 수정 | `fix(P5): DEF-{nnn} {요약} — developer` | — |
| 테스트 실행·결과 | `test(P5): {요약} — qa` | — |
| 게이트 문서 작성 | `docs(G{n}): 게이트 문서 작성 — pmo` | — |
| 게이트 PM 승인 | `gate(G{n}): {단계명} 승인` | `G{n}` (CR 회귀 후 재승인: `G{n}-CR-{nnn}`) |
| CR 접수·분석·PM 결정 | `cr(CR-{nnn}): {요약}` | — |
| PM 배포 승인 직후 (배포 실행 전) | `release: v{x.y.z} 배포 승인` | `release-v{x.y.z}` |
| 배포 실행 결과 | `chore(P7): 배포 실행 결과 기록 — devops` | — |
| 그 외 (STATUS 갱신, 티켓 처리 등) | `chore: {요약}` | — |

- 커밋 메시지 본문에는 변경 요약과 관련 문서·리뷰·티켓 경로를 적는다.
- 에이전트 보고에 `{PROJECT}` 밖 경로가 있으면 커밋하지 말고 PM에게 알린다.

- 태그는 annotated tag로 만들고 메시지에 게이트 문서 경로와 승인 일시를 적는다.
- devops는 `release-v*` 태그 기준으로 배포하고, 롤백은 이전 릴리스 태그로 한다.
- 커밋 전 `git status`로 `.env`·비밀 정보·대용량 파일이 포함되지 않았는지 확인한다.

### 고객 인도 패키지 (P8)
- 인도 범위(예: 소스·운영 가이드·최종 보고서만 / 전체)는 PM이 결정한다. 내부 리뷰·티켓·WORKLOG 포함 여부를 반드시 확인받는다.
- 오케스트레이터가 `git archive`로 승인된 태그 기준 선택 경로만 묶어 `{PROJECT}/.delivery/<slug>-<tag>.zip`에 만든다 (`.delivery/`는 git 제외).

---

## 10. PM 명령어 (`.claude/skills/`)
| 명령 | 용도 |
|---|---|
| `/kickoff <project-slug> <고객 요청>` | 프로젝트 생성(`projects/<slug>`) + git init → P1 계획 → G1 승인 요청 |
| `/run-phase [project-slug] <P1~P8 \| next>` | 지정 단계를 표준 루프로 실행 → 게이트 승인 요청 → 커밋·태그 |
| `/status [project-slug]` | 프로젝트 목록 또는 특정 프로젝트 현황 보고 |
| `/change-request [project-slug] <변경 내용>` | 변경 요청 접수 → 영향도 분석 → 승인 시 회귀 |

- `project-slug`를 생략하면: `projects/*/pm/STATUS.md` 중 프로젝트 상태가 `진행 중`인 것이 하나면 그 프로젝트, 여러 개면 PM에게 묻는다.
- ⚠️ Claude Code 기본 명령 `/init`은 CLAUDE.md를 생성·덮어쓰므로 **사용하지 않는다.** 프로젝트 시작은 `/kickoff`.
- PM은 명령어 없이 자연어로 지시해도 되며, 오케스트레이터는 이 문서의 프로세스에 맞춰 해석한다.
