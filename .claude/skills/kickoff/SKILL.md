---
name: kickoff
description: 새 고객 프로젝트를 착수한다. projects/<project-slug>/ 에 프로젝트 골격을 복사하고 독립 git 저장소를 만든 뒤, 고객 요청 원문을 CR-000으로 기록·커밋하고 P1 계획 단계(계획서·킥오프 회의록 → 교차 검토 → 게이트 G1 → PM 승인 요청)를 실행한다. 새 홈페이지 프로젝트를 시작할 때 사용.
argument-hint: "<project-slug> <고객 요청 내용 또는 요청 파일 경로>"
---

# 프로젝트 착수 (Kickoff)

인자: $ARGUMENTS

당신은 **오케스트레이터**다. `CLAUDE.md`의 §0(틀·프로젝트 분리), §9(Git 운영)를 따른다.
**이 스킬 실행 중 틀 보호 영역(`CLAUDE.md`, `README.md`, `.claude/`, `templates/` 등)은 수정하지 않는다.** 모든 쓰기는 `projects/<slug>/` 안에서만 한다.

---

## 1. 인자 해석 · 사전 확인
1. 첫 토큰이 slug 규칙(영문 소문자·숫자·하이픈, 3~40자)에 맞으면 `SLUG`, 나머지를 고객 요청으로 본다.
   - slug가 없거나 규칙에 맞지 않으면: 요청 내용에서 slug 후보 1~2개와 한글 표시명을 제안하고 AskUserQuestion으로 확정받는다.
   - 고객 요청이 비어 있으면 PM에게 요청 내용을 받는다. 요청이 파일 경로이면 해당 파일을 읽는다.
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
  - `projects/<SLUG>/pm/STATUS.md` — 프로젝트 ID, 프로젝트명, 고객, 틀 버전, 최종 갱신일 (고객명을 모르면 `[TBD: 고객 확인 필요]`)

## 3. 고객 요청 원문 기록
- `templates/docs/pm/change-request.md`를 읽어 `projects/<SLUG>/pm/requests/CR-000_initial-request.md`를 만든다 (`doc_id: PM-CR-000`, `type: initial`, `status: received`).
- "1. 요청 원문" 절에 PM이 전달한 내용을 **한 글자도 수정하지 않고** 기록한다.
- 요청이 짧거나 정보가 부족해도 진행한다. 부족한 정보는 계획서에 `[TBD]`로 남긴다.
  단, 사이트의 목적 자체를 알 수 없을 만큼 모호하면 AskUserQuestion으로 **최소한만** 질문한다 (사이트 유형, 핵심 목적, 희망 일정).

## 4. 최초 커밋
```bash
git -C "projects/<SLUG>" add -A
git -C "projects/<SLUG>" commit -m "chore(kickoff): 프로젝트 생성 및 고객 요청 기록" -m "framework: <FRAMEWORK>"
git -C "projects/<SLUG>" tag -a kickoff -m "kickoff — pm/requests/CR-000_initial-request.md"
```

## 5. P1 계획 단계 실행
`.claude/skills/run-phase/SKILL.md`를 읽고 `PROJECT: projects/<SLUG>`, 단계 **P1**로 표준 루프를 수행한다.
- 작성: `pmo` → `pm/01_project-plan.md`, `shared/meetings/MTG-{YYYYMMDD}_kickoff.md`, CR-000의 "요청 정리" 절, `pm/STATUS.md` 갱신
- 검토(병렬): `planner`, `developer`
- 반영 → 게이트 `pm/gates/G1_plan.md` → PM 보고 → 승인 시 커밋·태그 `G1`

## 6. PM 보고 시 강조할 것
- 생성된 프로젝트 경로 `projects/<SLUG>/`와 틀 버전
- 범위(포함/제외)와 가정
- 단계별 일정·마일스톤
- **고객 확인 필요 사항 목록** — PM이 고객에게 바로 물어볼 수 있게 질문 형태로
- 주요 리스크
- G1 승인 여부
