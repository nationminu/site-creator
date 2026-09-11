# Site Creator

여러 팀(에이전트)이 협업하여 고객 홈페이지를 제작하는 **프레임워크 저장소**입니다.
프로젝트는 `projects/<프로젝트>/`에 독립 저장소로 생성되고, 모든 산출물은 그 안에 출력됩니다.
프로세스·규칙의 기준 문서는 [CLAUDE.md](CLAUDE.md)입니다.

## 빠른 시작 (총괄 PM용)

| 순서 | 명령 | 하는 일 |
|---|---|---|
| 1 | `/kickoff <project-slug> <고객 요청 내용>` | `projects/<slug>/` 생성 + git init → 계획서 작성·검토 → G1 승인 요청 |
| 2 | (보고 확인 후) "승인" / "수정: …" | 게이트 승인(→ 자동 커밋·태그) 또는 수정 지시 |
| 3 | `/run-phase next` | 다음 단계 실행 (작성 → 교차 검토 → 반영 → 게이트) |
| 4 | `/status` | 프로젝트 목록 / 현황 확인 |
| 5 | `/change-request <변경 내용>` | 고객 요구 변경 시 영향도 분석 후 회귀 |

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
| G1 | 범위·일정·산출물 확정 |
| P3 중간 | 디자인 컨셉 시안 선택 |
| G2 ~ G5 | 단계 산출물 승인 |
| G6 | 중간보고(고객) 결과 및 피드백 반영 여부 |
| P7 중간 | **운영 배포 실행 승인** |
| G8 | 고객 인도 범위, 프로젝트 종료 승인 |
| 수시 | 리뷰 3라운드 초과·팀 간 충돌 시 결정, 원격 저장소 push |

## 디렉토리 구조

```
site-creator/                      ← Git ① 틀 저장소 (에이전트·규칙·템플릿)
├── CLAUDE.md                      # 프레임워크 헌장 (조직·프로세스·규칙)
├── README.md                      # 이 문서
├── .claude/
│   ├── agents/                    # 팀 에이전트: pmo, planner, designer, developer, qa, devops
│   ├── skills/                    # PM 명령어: kickoff, run-phase, status, change-request
│   └── settings.json              # 틀 보호 규칙 (보호 영역 편집 시 확인 요청)
├── templates/
│   ├── project/                   # /kickoff 때 복사되는 프로젝트 골격
│   └── docs/                      # 산출물 문서 템플릿
│       └── pm/ planning/ design/ developer/ qa/ devops/ shared/
└── projects/                      # (틀 저장소에서 git 제외)
    └── <project-slug>/            ← Git ② 프로젝트 저장소 (프로젝트별 독립)
        ├── README.md
        ├── pm/                    #   STATUS.md(현황판), requests/, gates/, 계획·보고서
        ├── planning/              #   요구사항, IA, 화면정의서
        ├── design/                #   컨셉, 디자인 시스템, mockups/, assets/
        ├── developer/site/        #   ★ 홈페이지 소스코드
        ├── qa/                    #   테스트 계획·케이스·결과
        ├── devops/                #   배포 계획·결과, 운영 가이드
        └── shared/                #   tickets/, reviews/, decisions/, meetings/
```

## 틀과 프로젝트의 분리

| | 틀 (`./`) | 프로젝트 (`projects/<slug>/`) |
|---|---|---|
| 수정 시점 | 사용자가 에이전트·스킬·규칙·템플릿 변경을 **명시적으로 요청**할 때만 | 프로젝트 작업 중 상시 |
| Git | 틀 변경 시 커밋 | kickoff·게이트 승인·CR 결정·릴리스 시 자동 커밋·태그 |
| 템플릿 | 한 곳에서 관리 | 복사하지 않음 — 작성된 산출물만 저장. kickoff 시 틀 커밋 번호 기록 |
