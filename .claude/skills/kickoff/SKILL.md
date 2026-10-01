---
name: kickoff
description: 새 고객 프로젝트를 착수한다. projects/<project-slug>/ 에 프로젝트 골격을 복사하고 독립 git 저장소를 만든 뒤, 고객 요청 원문을 CR-000으로 기록·커밋하고, 빠진 핵심 정보만 사전 질문한 뒤 P1 계획 단계(계획서·킥오프 미팅 자료·고객 질문 → 교차 검토 → 게이트 G1 → PM 승인 요청)를 실행한다. 새 홈페이지 프로젝트를 시작할 때 사용.
argument-hint: "<project-slug> [--standard] <고객 요청 내용 또는 요청 파일 경로>"
---

# 프로젝트 착수 (Kickoff)

인자: $ARGUMENTS

당신은 **오케스트레이터**다. `CLAUDE.md`의 §0(틀·프로젝트 분리), §9(Git 운영)와 `.claude/reference/git-ops.md`, 진행 모드는 `.claude/reference/modes.md`를 따른다.
**이 스킬 실행 중 틀 보호 영역(`CLAUDE.md` §0)은 수정하지 않는다.** 모든 쓰기는 `projects/<slug>/` 안에서만 한다.

---

## 1. 인자 해석 · 사전 확인
1. 첫 토큰이 slug 규칙(영문 소문자·숫자·하이픈, 3~40자)에 맞으면 `SLUG`, 나머지를 고객 요청으로 본다.
   - slug가 없거나 규칙에 맞지 않으면: 요청 내용에서 slug 후보 1~2개와 한글 표시명을 제안하고 AskUserQuestion으로 확정받는다.
   - 고객 요청이 비어 있으면 PM에게 요청 내용을 받는다. 요청이 파일 경로이면 해당 파일을 읽는다.
   - 인자에 `--standard` 또는 `--lite`가 있으면 그것이 `MODE`다(인자에서 제거한 뒤 나머지를 요청으로 본다). **없으면 `MODE=lite`(기본)** 로 정하고 묻지 않는다.
     요청에 대규모 신호(회원·결제·외부 연동 다수, 10페이지 초과, 다수 이해관계자 등)가 있으면 G1 보고에 `standard` 전환 권고를 넣도록 pmo에 전달한다.
2. `projects/<SLUG>`가 이미 존재하면 **중단**하고 PM에게 알린다 (덮어쓰지 않는다).
3. 틀 버전 확인 (틀 루트에서):
   ```bash
   git rev-parse --short HEAD          # 틀 커밋 번호 → FRAMEWORK
   git status --porcelain              # 출력이 있으면 FRAMEWORK에 "-dirty" 붙이고 PM에게 "틀에 커밋되지 않은 변경이 있음"을 알림
   ```
   틀 저장소가 git이 아니면 `FRAMEWORK=untracked`로 기록한다.

## 2. 프로젝트 골격 생성
```bash
cp -r templates/project "projects/<SLUG>"
git -C "projects/<SLUG>" init -b main
```
- 복사 결과에 `.gitignore`, `.gitattributes`, 팀 폴더(`pm planning design developer qa devops shared`)가 모두 있는지 확인한다.
- 다음 파일의 자리표시자를 채운다:
  - `projects/<SLUG>/README.md` — `{프로젝트 표시명}`, `{project-slug}`, `{고객명}`, 착수일, `{framework-commit}`
  - `projects/<SLUG>/pm/STATUS.md` — 프로젝트 ID, 프로젝트명, 고객, 진행 모드(`MODE`), 틀 버전, 최종 갱신일 (고객명을 모르면 `[TBD: 고객 확인 필요]`)

## 3. 고객 요청 원문 기록
- `templates/docs/pm/change-request.md`를 읽어 `projects/<SLUG>/pm/requests/CR-000_initial-request.md`를 만든다 (`doc_id: PM-CR-000`, `type: initial`, `status: received`).
- "1. 요청 원문" 절에 PM이 전달한 내용을 **한 글자도 수정하지 않고** 기록한다.
- **사전 점검 (계획서 작성 전)**: 요청 원문을 아래 필수 항목으로 대조한다.

  | 필수 항목 | 왜 필요한가 |
  |---|---|
  | 사이트 목적·핵심 전환 목표 | 범위·성공 기준 |
  | 주요 대상 사용자 | 기획·디자인 방향 |
  | 페이지 범위 (대략적인 페이지·메뉴) | 규모·일정 |
  | 희망 오픈일 | 역산 일정 |
  | 호스팅·도메인 현황 | 스택 프리셋 |
  | 관리자(콘텐츠 수정) 필요 여부 | 스택 프리셋·규모 |

  빠진 항목 중 **계획에 가장 큰 영향을 주는 것만 AskUserQuestion으로 한 번(최대 4문항)** 묻는다. 각 질문에 흔한 선택지를 옵션으로 주고, PM이 "모름/나중에"를 고르면 그대로 진행한다. 다 갖춰졌으면 묻지 않는다.
- PM의 답과 남은 미확인 항목은 `projects/<SLUG>/pm/requests/QNA.md`에 기록한다: 답을 받은 항목은 `Q-001`부터 질문·답변(원문)·출처 PM·상태 `answered`로, 모르는 항목은 상태 `open`으로 둔다(이후 pmo가 고객 질문을 이어서 등록한다). 요청 원문(CR-000)은 수정하지 않는다.

## 4. 최초 커밋
`projects/<SLUG>/ACTIVITY.md`의 날짜 자리표시자를 오늘로 바꾸고 첫 항목을 적는다: `* 추가: 프로젝트 생성, 고객 요청 CR-000 기록 — 진행 모드 {MODE} (오케스트레이터)`
```bash
git -C "projects/<SLUG>" add -A
git -C "projects/<SLUG>" commit -m "chore(kickoff): 프로젝트 생성 및 고객 요청 기록" -m "framework: <FRAMEWORK>"
git -C "projects/<SLUG>" tag -a kickoff -m "kickoff — pm/requests/CR-000_initial-request.md"
```

## 5. P1 계획 단계 실행
`.claude/skills/run-phase/SKILL.md`를 읽고 `PROJECT: projects/<SLUG>`, 단계 **P1**로 표준 루프를 수행한다.
- 작성: (해당 시 `devops` 호스팅 사전 의견 →) `pmo` → `pm/01_project-plan.md`, `shared/meetings/MTG-{YYYYMMDD}_kickoff-agenda.md`(고객 킥오프 미팅 자료), CR-000의 "요청 정리" 절, `pm/requests/QNA.md` 질문 등록, `pm/STATUS.md` 갱신
- 검토: `run-phase` §2·§3 기준 (standard: `planner`, `developer` / lite: `.claude/reference/modes.md`의 P1 주 검토자)
- 반영 → 게이트 `pm/gates/G1_plan.md` → PM 보고 → 승인 시 커밋·태그 `G1`

## 6. PM 보고 시 강조할 것
- 생성된 프로젝트 경로 `projects/<SLUG>/`, 진행 모드(기본 lite — `standard` 전환 권고가 있으면 함께), 틀 버전
- 범위(포함/제외)와 가정
- 잠정 스택 프리셋과 근거, 예상 월 운영 비용
- 규모 초안(페이지 n개, 고객 요청 기능 n개, 팀 제안 n개)과 오픈일 역산 일정·임계 경로
- 비용 요약(일회성·월)
- **고객 확인 질문 목록**(QNA의 `open` 질문 — 고객에게 그대로 보낼 수 있는 형태, 호스팅·참고 사이트·결정권자 포함)과 **킥오프 미팅 자료** 경로
- 주요 리스크 (상위 3개)
- **G1 결정 묶음** — AskUserQuestion 한 번(최대 4문항)으로 확인한다: ① 진행 모드(lite 유지 / standard 전환 — 권고 표시) ② 잠정 스택 프리셋 ③ 오픈 목표일(계획서 역산 결과 수용 / 조정) ④ 피드백 정책(기본안 / 조정). 답은 게이트 문서 "PM 결정"에 기록하고, 이어서 G1 승인 여부를 받는다.
