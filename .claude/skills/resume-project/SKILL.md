---
name: resume-project
description: 다른 세션이나 중단 이후에 프로젝트 작업을 이어간다. STATUS·게이트·산출물 헤더·리뷰·티켓·CR·ADR·WORKLOG·git 상태를 읽어 마지막 완료 지점과 재개 지점을 판정하고, PM에게 재개 계획을 확인받은 뒤 해당 스킬(run-phase / change-request) 절차의 그 지점부터 이어서 진행한다. "이어서 진행", "어제 하던 거 계속", "세션이 끊겼어" 등에 사용.
argument-hint: "[project-slug]"
---

# 프로젝트 이어서 진행 (Resume)

인자: $ARGUMENTS

당신은 **오케스트레이터**다. 새 세션에는 이전 대화의 기억이 없으므로 **파일과 git만을 사실로** 삼는다.
`CLAUDE.md` §0(틀·프로젝트 분리), §3(표준 루프), §9(Git 운영)를 따른다.

> Claude Code 기본 명령 `/resume`(이전 대화 다시 열기)과 다른 기능이다. 이 스킬은 대화가 아니라 **프로젝트 파일 상태**로 재개한다.

---

## 0. 프로젝트 결정
- 첫 토큰이 `projects/<토큰>/`으로 존재하면 그것이 `SLUG`, 아니면 `CLAUDE.md` §10 규칙(상태가 `진행 중`·`유지보수`인 프로젝트가 하나면 자동 선택, 여러 개면 질문).
- 이하 `{PROJECT}` = `projects/<SLUG>`. 이후 호출하는 모든 에이전트 프롬프트에 `PROJECT: projects/<SLUG>`를 넣는다.

## 1. 상태 수집 — 읽기 전용
**PM이 재개 계획을 확인하기 전까지 어떤 파일도 수정하지 않고, 에이전트도 호출하지 않는다.**

| 수집 대상 | 방법 | 볼 것 |
|---|---|---|
| 현황판 | `{PROJECT}/pm/STATUS.md` | 프로젝트 상태, **진행 모드**, 현재 단계, 단계 표, PM 결정 필요 사항, 예외 기록 |
| Git | `git -C {PROJECT} log --oneline -10`, `git -C {PROJECT} tag --sort=creatordate`, `git -C {PROJECT} status --short` | 마지막 커밋·태그 = **마지막 확정 지점**, 미커밋 변경 = 그 이후 작업분 |
| 동시 작업 흔적 | `{PROJECT}/.git/index.lock` 존재 여부 | 있으면 다른 세션이 git 작업 중일 수 있음 → 즉시 PM에게 확인 |
| 게이트 | `{PROJECT}/pm/gates/G*.md` | `status`, "PM 결정" 절이 비었는지, 대응 태그가 있는지 |
| 현재 단계 산출물 | `CLAUDE.md` §2 단계표의 산출물 경로 | 존재 여부, 헤더 `version`·`status`·`updated`, 문서 끝 "변경 이력" 표 존재(부분 작성 여부) |
| 리뷰 | `{PROJECT}/shared/reviews/{단계}_*` | 검토자별 최신 라운드, `verdict`, 지적 표 "처리 결과" 열 기입 여부 |
| 티켓·결함 | `{PROJECT}/shared/tickets/` | `status: open\|in-progress\|reopened\|resolved` |
| 변경 요청 | `{PROJECT}/pm/requests/CR-*` | `status`가 `done\|rejected`가 아닌 것 |
| 고객 질문 | `{PROJECT}/pm/requests/QNA.md` | `answered`인데 반영 위치가 비어 있으면 → `/answer` §3 반영 재개 |
| 제안 | `{PROJECT}/pm/requests/SUG-*`, `{PROJECT}/inbox/` | `status`가 `applied\|rejected`가 아닌 것(analyzing → 검토·종합 재개, pending-approval → PM 결정 요청, accepted → 반영 재개), inbox에 남은 파일 → 접수 여부 PM 확인 (`suggest` 스킬 해당 절) |
| 결정 기록 | `{PROJECT}/shared/decisions/ADR-*` | `status: proposed` |
| 진행 요약 | `{PROJECT}/ACTIVITY.md` 상단 항목 | 마지막으로 기록된 작업 단위 (git 로그와 대조 — 커밋에 없는 항목·항목 없는 커밋은 불일치로 보고) |
| 작업 로그 | `{PROJECT}/{팀}/WORKLOG.md` 각 최신 항목 | 마지막 작업, "이슈·다음 할 일" |
| 틀 저장소 | `git status --short` (틀 루트) | 틀에 의도치 않은 변경이 있으면 보고 |

## 2. 재개 지점 판정

위에서부터 **처음 일치하는 행**을 재개 지점으로 삼는다. 여러 행이 동시에 해당하면 모두 보고하고 위쪽 행부터 처리한다.

| # | 관찰된 상태 | 판정 | 재개 행동 |
|---|---|---|---|
| 1 | `STATUS.md` 프로젝트 상태 `종료` | 종료된 프로젝트 | 현황만 보고하고 멈춤 (변경 요청이면 `/change-request`로 `유지보수` 재개) |
| 1-1 | 프로젝트 상태 `유지보수` | 종료 후 변경 진행 중 | 열린 CR을 찾아 아래 CR 행부터 판정 |
| 2 | `index.lock` 존재 또는 PM이 다른 세션 작업 중이라고 함 | 동시 작업 의심 | 진행하지 않고 PM 확인 |
| 3 | CR `status: analyzing` | CR 영향도 분석 중 | `change-request` §2 — 없는 영향 의견만 요청 → pmo 종합 |
| 3-0 | CR `status: received` | CR 접수 후 끊김 | `change-request` §2 영향도 분석 시작 |
| 4 | CR `status: pending-approval` | CR 결정 대기 | `change-request` §3 PM 결정 요청 |
| 4-1 | CR `status: approved` + 회귀 단계 재승인 태그(`G{n}-CR-{nnn}`) 일부 없음 | CR 회귀 진행 중 | 회귀 중인 가장 앞 단계부터 `run-phase` 재개 (`change-request` §4) |
| 4-2 | CR `approved` + 모든 회귀 단계 재승인 태그 있음 + `status`≠`done` | CR 마감 누락 | CR `status: done`·STATUS 갱신 → 커밋 |
| 4-3 | SUG `status: received`/`analyzing` | 제안 검토 중 | `suggest` §2 — 없는 검토만 호출 → §3 pmo 종합 |
| 4-4 | SUG `status: pending-approval` | 제안 결정 대기 | `suggest` §3 PM 결정 요청 |
| 4-5 | SUG `status: accepted`/`partially-accepted` + "반영 결과" 비어 있음 | 제안 반영 중 | `suggest` §4 분류별 반영 재개 (`on-hold`는 보고만) |
| 4-6 | QNA `answered` + "반영 위치" 비어 있음 | 답변 반영 중 | `answer` §3 반영 재개 |
| 4-7 | `inbox/`에 README 외 파일 | 접수 안 된 제안 자료 | PM에게 `/suggest` 접수 여부 확인 |
| 5 | 게이트 문서 존재 + "PM 결정" 절 비어 있음 | **게이트 승인 대기** | `run-phase` §5 — 게이트 문서를 읽어 **게이트 보고를 다시 제시** |
| 6 | 게이트 "PM 결정" 기입됨(승인) + 대응 태그(`G{n}`) 없음 | 승인 후 커밋 누락 | 산출물 헤더 approved/1.0·STATUS 반영 확인 → `run-phase` §6-1 커밋·태그 |
| 6-1 | lite: G5 문서 PM 결정에 P6 생략 승인 + STATUS P6 ≠ `➖ 생략` | 생략 기록 누락 | STATUS P6 `➖ 생략`·예외 기록 → 커밋 → P7 |
| 6-2 | P2: 요구사항 초안 있음 + "3.1 제안 기능" PM 결정 열 비어 있음 + 교차 검토 리뷰 없음 | 제안 기능 결정 대기 | `run-phase` §2 P2 ② PM 개별 결정 요청 (검토 전) |
| 7 | P3: `03_design-concept.md` 시안 작성됨 + 컨셉 선택 ADR 없음 | 시안 선택 대기 | (시안 스크린샷이 없으면 먼저 캡처) → `run-phase` §2 P3 PM 시안 선택 요청 |
| 7-1 | P3: 컨셉 ADR 있음 + 전 SCR 목업 있음 + `design/evidence/mockups/scr-*` 없음 | 스크린샷 캡처 대기 | `run-phase` "스크린샷 캡처" |
| 7-2 | P3: 목업 스크린샷 있음 + 페이지 디자인 "시각 점검" 표 비어 있음 | 시각 자기 점검 대기 | `designer` 시각 자기 점검 (`run-phase` §2 P3 ⑤) |
| 7-3 | P4: 기술 설계 "외부 서비스 후보 비교"에 항목 있음 + 대응 ADR 없음 | 외부 서비스 선택 대기 | PM 선택 요청 → pmo ADR |
| 7-4 | P4: 작업 단위 1 커밋 있음 + `shared/reviews/P4_early-alignment_designer.md` 없음 | 초기 정합 확인 대기 | `designer` 초기 정합 확인 |
| 7-5 | P4: 백엔드 프리셋 + 개발 보고서 완성 + `P4_code_security-review_r*` 없음 또는 Must 미해결 | 보안 코드 리뷰 대기 | `run-phase` §2 P4 ④ |
| 7-6 | P5: `qa/tools`에 화면 회귀 기준 스크린샷 없음 + G4 승인 | 기준 생성 대기 | `qa` 기준 스크린샷 생성 |
| 7-7 | P5: Critical·Major 0 + 프리뷰 결정 기록 없음 | 프리뷰 결정 대기 | PM에게 프리뷰 배포 여부 질문 (`run-phase` §2 P5 ④) |
| 7-8 | P5: `devops/05_preview.md` 있음 + PM 실기기 확인·피드백 처리 기록 없음 | 프리뷰 확인 대기 | PM에게 실기기 점검표 확인 요청, 피드백은 `/suggest` |
| 8 | `status: proposed` ADR 존재 | PM 결정 대기 | ADR 요약 제시 후 결정 요청 |
| 9 | P7: 배포 계획 검토 완료 + `release-v*` 태그 없음 | 배포 승인 대기 | PM 배포 승인 요청 (**이전 세션의 승인 발언은 이월되지 않음**) |
| 10 | P7: `release-v*` 태그 있음 + 배포 보고서 없음 | 배포 실행 중 끊김 | **재배포 금지.** devops에게 운영 URL·호스팅 상태 **확인만** 지시 → 결과를 PM에게 보고하고 다음 행동(재배포/스모크 테스트/롤백) 결정 요청 |
| 10-1 | P7: 배포 보고서 있음 + `qa/07_smoke-test-report.md` 없음 | 스모크 테스트 대기 | `qa` 운영 스모크 테스트 |
| 10-2 | G7 태그 있음 + 배포 보고서 "오픈 후 관찰" 비어 있음 | 관찰 기록 대기 | 관찰 예정일이 지났으면 `devops` 관찰 기록 호출, 아니면 예정일만 보고 |
| 10-3 | G8 태그 있음 + (종료 체크리스트 "G8 승인 후" 미완료 또는 인도 패키지 없음 또는 STATUS≠`종료`) | 종료 처리 중 | `run-phase` §6-3 재개 |
| 11 | P5: `DEF-*` `open`/`reopened` 존재 | 결함 수정 대기 | developer 결함 수정 |
| 12 | P5: `DEF-*` `resolved` 존재 | 재검증 대기 | qa 재검증·회귀 테스트 |
| 13 | 최신 라운드 리뷰 중 `verdict: 수정 요청` + "처리 결과" 열 비어 있음 | 반영 대기 | Owner에게 해당 리뷰 반영 지시 (`run-phase` §4) |
| 14 | "처리 결과" 기입 완료 + Must 지적 검토자의 다음 라운드 리뷰 없음 | 재검토 대기 | 해당 검토자만 다음 라운드 재검토 |
| 15 | 산출물 `status: in-review` + 단계표 검토자(lite 모드면 산출물별 주 검토자 — `.claude/reference/modes.md`) 중 일부의 리뷰만 존재 | 교차 검토 중 끊김 | **누락된 검토자만** 호출 (lite면 `model: sonnet` — `run-phase` §3) |
| 16 | 모든 검토자 최신 판정이 승인/조건부 승인 + 게이트 문서 없음 | 게이트 준비 대기 | Should 미반영 사유 확인 → pmo 게이트 준비 (`run-phase` §5) |
| 17 | 산출물 없음·일부만 있음·`status: draft`·변경 이력 누락(부분 작성) | 작성 중 끊김 | Owner에게 **기존 파일을 읽고 이어서 완성** 지시 (처음부터 덮어쓰기 금지) |
| 17-1 | P4: 개발 보고서 "진행 현황"에 남은 작업 단위가 있음 (또는 마지막 `feat(P4)` 커밋 이후 미커밋 변경) | 구현 작업 단위 중 끊김 | 미커밋 변경을 확인(필요 시 보존 커밋 제안) → `developer`에게 진행 현황의 **다음 작업 단위**부터 이어서 구현 지시 (`run-phase` §2 P4) |
| 18 | 이전 게이트 태그 있음 + 다음 단계 산출물 없음 | 단계 사이 | `run-phase next` 제안 |

### 불일치 처리
- `STATUS.md`와 파일·git 상태가 다르면 **파일·git을 사실로** 보고, 차이를 보고에 명시하고, STATUS 갱신을 재개 계획에 포함한다.
- WORKLOG에 기록이 없는데 산출물이 바뀌었거나, WORKLOG에는 완료로 적혔는데 산출물이 부분 작성이면 "에이전트 작업 중 끊김"으로 보고 17행 방식으로 처리한다.
- 미커밋 변경이 많으면 재개 전에 보존 커밋(`chore: 세션 재개 전 작업분 보존`)을 PM에게 제안한다 (PM 동의 시에만 커밋).

## 3. PM에게 재개 계획 보고

```
## [<SLUG>] 재개 지점 보고 (기준: YYYY-MM-DD)
**마지막 확정 지점**: 커밋 <hash> · 태그 <최근 태그> (<일자>)
**현재 상태**: P{n} {단계명} — {판정}
**근거**: (판정에 쓴 파일·필드를 2~4줄로)
**미커밋 변경**: N건 — (주요 파일)
**불일치·주의**: (STATUS 불일치, 부분 작성 파일, 틀 변경, 동시 작업 흔적 등 / 없음)
**재개 계획**:
  1. …
  2. …
**PM 결정 대기**: (STATUS의 결정 필요 사항 중 재개에 필요한 것)
→ 이 계획대로 진행할까요? (wip 커밋이 필요하면 함께 확인)
```

- 판정이 **게이트 승인 대기(5행)** 이면 재개 계획 보고 대신 `run-phase` §5 형식의 **게이트 보고**를 바로 제시한다. 게이트 결정이 곧 재개의 첫 행동이기 때문이다.

## 4. 이어서 진행
- PM이 확인하면 판정된 지점에 해당하는 스킬 절(`.claude/skills/run-phase/SKILL.md` 또는 `.claude/skills/change-request/SKILL.md`)을 읽고 **그 지점부터** 수행한다.
- 이미 완료된 작성·검토를 반복하지 않는다. 에이전트 호출 프롬프트에는 기존 산출물·리뷰 경로와 "이전 세션에서 중단된 작업을 이어서 수행"임을 명시한다.
- 배포·push·외부 계정 등 **안전 규칙(§8) 대상 행동은 이번 세션에서 PM 승인을 새로 받는다.**
