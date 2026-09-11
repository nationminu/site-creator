---
name: change-request
description: 프로젝트에 대한 고객(PM)의 변경 요청을 접수해 CR 문서로 기록하고, 기획·개발팀 의견을 받아 PMO가 영향도(요구사항·산출물·회귀 단계·일정·리스크)를 분석한 뒤 PM 결정을 커밋하고, 승인 시 영향 받는 가장 앞 단계로 회귀시킨다. 진행 중 요구사항 추가·변경·삭제, 중간보고 고객 피드백 반영 시 사용.
argument-hint: "[project-slug] <변경 요청 내용>"
---

# 변경 요청 (CR)

인자: $ARGUMENTS

당신은 **오케스트레이터**다. `CLAUDE.md` §0(틀·프로젝트 분리), §7(변경 관리), §9(Git 운영)를 따른다.
**변경 요청은 프로젝트 산출물에 대한 것이다.** 요청이 에이전트·스킬·규칙·템플릿(틀) 변경이라면 이 스킬을 쓰지 말고, 틀 수정 요청으로 PM에게 확인한 뒤 틀 저장소에서 처리한다.

---

## 0. 프로젝트 결정
- 첫 토큰이 `projects/<토큰>/`으로 존재하면 그것이 `SLUG`, 나머지가 변경 내용. 아니면 `CLAUDE.md` §10 규칙으로 정한다.
- 이하 `{PROJECT}` = `projects/<SLUG>`. 모든 에이전트 호출에 `PROJECT: projects/<SLUG>`를 넣는다.

## 1. 접수
- `{PROJECT}/pm/requests/`에서 기존 CR 번호를 확인하고 다음 번호를 부여한다 (`CR-001`부터).
- `templates/docs/pm/change-request.md`를 읽어 `{PROJECT}/pm/requests/CR-{nnn}_{slug}.md`를 만들고 (`type: change`, `status: analyzing`) "요청 원문" 절에 **수정 없이** 기록한다.
- `{PROJECT}/pm/STATUS.md`의 "변경 요청" 표에 추가한다.

## 2. 영향도 분석
1. `planner`, `developer`를 **병렬 호출**하여 영향 의견을 받는다.
   - planner → `{PROJECT}/shared/reviews/CR-{nnn}_impact_planner.md` (영향 REQ/SCR, 신규·변경·삭제 항목, 회귀 필요 단계)
   - developer → `{PROJECT}/shared/reviews/CR-{nnn}_impact_developer.md` (영향 파일, 작업량, 기술 리스크)
   - 디자인 변경이 명백하면 `designer`도 함께 호출 → `CR-{nnn}_impact_designer.md`
2. `pmo`를 호출해 의견을 종합하고 CR 문서의 "요청 정리", "영향도 분석" 절과 권고(수용/부분 수용/보류/거절)를 작성한다 (`status: pending-approval`).

## 3. PM 결정
다음을 보고하고 결정을 받는다:
- 변경 요약 · 영향 받는 산출물 · 회귀 시작 단계 · 일정/리스크 영향 · pmo 권고

결정은 CR 문서 "PM 결정" 절에 기록하고, `pmo`가 `{PROJECT}/shared/decisions/ADR-*`로 남긴다. 이어서 커밋한다:
```bash
git -C {PROJECT} add -A
git -C {PROJECT} commit -m "cr(CR-{nnn}): {요약} — {승인|거절|보류}"
```

## 4. 승인 시 회귀
- CR `status: approved`, `{PROJECT}/pm/STATUS.md`에서 회귀 대상 단계들을 `↩️ 회귀`로 표시한다.
- 회귀 시작 단계부터 `.claude/skills/run-phase/SKILL.md` 절차를 수행한다. 이때:
  - Owner에게 CR 경로를 전달하고 **영향 받은 부분만** 개정하게 한다 (버전 증가, 변경 이력에 CR ID).
  - 검토·검증도 영향 범위 중심으로 한다 (qa는 영향 TC + 회귀 테스트).
  - 재승인 태그는 `G{n}-CR-{nnn}`을 사용한다.
  - 이미 배포된 뒤라면 P7 재배포(새 `release-v*` 태그)까지 포함한다.
- 모든 회귀 단계가 재승인되면 CR `status: done`.

## 5. 거절·보류 시
사유를 CR 문서와 `{PROJECT}/pm/STATUS.md`에 기록한다 (`status: rejected | on-hold`).
