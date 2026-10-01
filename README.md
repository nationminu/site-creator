# Site Creator

여러 팀(에이전트)이 협업하여 고객 홈페이지를 제작하는 **프레임워크 저장소**입니다.
프로젝트는 `projects/<프로젝트>/`에 독립 저장소로 생성되고, 모든 산출물은 그 안에 출력됩니다.
프로세스·규칙의 기준 문서는 [CLAUDE.md](CLAUDE.md)입니다.

## 빠른 시작 (총괄 PM용)

| 순서 | 명령 | 하는 일 |
|---|---|---|
| 1 | `/kickoff <project-slug> [--standard] <고객 요청 내용>` | `projects/<slug>/` 생성 + git init → 계획서 작성·검토 → G1 승인 요청 (기본 lite 모드, `--standard`: 전체 검토 모드) |
| 2 | (보고 확인 후) "승인" / "수정: …" | 게이트 승인(→ 자동 커밋·태그) 또는 수정 지시 |
| 3 | `/run-phase next` | 다음 단계 실행 (작성 → 교차 검토 → 반영 → 게이트) |
| 4 | `/status` | 프로젝트 목록 / 현황 확인 |
| 5 | `/change-request <변경 내용>` | 고객 요구 변경 시 영향도 분석 후 회귀 |
| 6 | `/answer [Q-xxx] <답변>` | 고객 질문 답변 기록 → 관련 문서 반영 |
| 7 | `/suggest <제안> [파일·URL]` | PM·고객의 시안 캡처·기능·자료 제안 접수 → 팀 검토 → PM 결정 → 반영 (파일은 `projects/<slug>/inbox/`에) |

프로젝트가 여러 개 진행 중이면 명령에 `project-slug`를 붙입니다. 예: `/run-phase acme-homepage next`

> ⚠️ Claude Code 기본 명령 `/init`은 CLAUDE.md를 덮어쓰므로 사용하지 마세요.

## 진행 흐름

```
P1 계획 → P2 기획 → P3 디자인 → P4 개발 → P5 검증(로컬) → P6 중간보고 → P7 배포(운영) → P8 최종 산출물
   G1        G2         G3         G4           G5              G6              G7              G8
```

## PM이 결정하는 순간

| 시점 | 결정 내용 |
|---|---|
| G1 | 범위·일정·산출물 확정, 잠정 스택 프리셋, 콘텐츠 수급·PM 사전 준비 기한 확인 |
| P3 중간 | 디자인 컨셉 시안 선택 |
| G2 | 요구사항 승인, **스택 프리셋·로컬 개발 환경·운영 환경 확정** |
| G3 ~ G5 | 단계 산출물 승인 |
| G5 | (lite) P6 중간보고 생략 여부, 프리뷰 배포 여부 |
| G6 | 중간보고(고객) 결과 및 피드백 반영 여부 (lite에서 생략하면 없음) |
| P7 중간 | **운영 배포 실행 승인**, 남은 `[TBD` 예외 승인, 검색엔진 등록(PM 계정) |
| G8 | 하자보수·유지보수 범위, 고객 인도 범위·방식, 계정 명의 이전, 프로젝트 종료 승인 |
| kickoff·G1 | 진행 모드 (기본 lite, 대규모면 standard 전환 권고) |
| 수시 | 리뷰 라운드 상한 초과·팀 간 충돌 시 결정, 원격 저장소 push |

## 디렉토리 구조

```
site-creator/                      ← Git ① 틀 저장소 (에이전트·규칙·템플릿)
├── CLAUDE.md                      # 프레임워크 헌장 (조직·프로세스·규칙)
├── README.md                      # 이 문서
├── USAGE.md                       # PM용 상세 사용 가이드
├── ACTIVITY.md                    # 틀 변경 진행 요약 (날짜별, 최신이 위)
├── .claude/
│   ├── agents/                    # 팀 에이전트: pmo, planner, designer, developer, qa, devops
│   ├── skills/                    # PM 명령어: kickoff, run-phase, status, change-request, resume-project, suggest, answer
│   ├── reference/                 # 기준 문서: 진행 모드, 스택 프리셋, 실행 환경, 디자인 프로필, 국내 실무·마감 품질 체크리스트, Git 운영
│   └── settings.json              # 틀 보호 규칙 (보호 영역 Edit·Write 시 확인 요청)
├── templates/
│   ├── project/                   # /kickoff 때 복사되는 프로젝트 골격
│   └── docs/                      # 산출물 문서 템플릿
│       └── pm/ planning/ design/ developer/ qa/ devops/ shared/
└── projects/                      # (틀 저장소에서 git 제외)
    └── <project-slug>/            ← Git ② 프로젝트 저장소 (프로젝트별 독립)
        ├── README.md
        ├── ACTIVITY.md            #   진행 요약 (날짜별, 최신이 위)
        ├── inbox/                 #   제안 자료(캡처·파일) 넣는 곳 → /suggest
        ├── pm/                    #   STATUS.md(현황판), requests/(CR·QNA·SUG), gates/, 계획·보고서
        ├── planning/              #   요구사항, IA, 화면정의서
        ├── design/                #   컨셉, 디자인 시스템, mockups/, assets/
        ├── developer/site/        #   ★ 홈페이지 소스코드
        ├── qa/                    #   테스트 계획·케이스·결과
        ├── devops/                #   운영 환경 명세, 프리뷰, 배포 계획·결과, 운영 가이드
        └── shared/                #   tickets/, reviews/, decisions/, meetings/
```

## 틀과 프로젝트의 분리

| | 틀 (`./`) | 프로젝트 (`projects/<slug>/`) |
|---|---|---|
| 수정 시점 | 사용자가 에이전트·스킬·규칙·템플릿 변경을 **명시적으로 요청**할 때만 | 프로젝트 작업 중 상시 |
| Git | 틀 변경 시 커밋 | **작업 단위(에이전트 호출)마다** 자동 커밋, 게이트 승인·릴리스 시 태그 |
| 템플릿 | 한 곳에서 관리 | 복사하지 않음 — 작성된 산출물만 저장. kickoff 시 틀 커밋 번호 기록 |
