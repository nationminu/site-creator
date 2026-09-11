---
name: run-phase
description: 지정한 프로젝트의 단계(P1~P8)를 표준 루프(착수 점검 → Owner 작성 → 병렬 교차 검토 → 반영 → 게이트 문서 → PM 승인 요청 → 커밋·태그)로 실행한다. "다음 단계 진행", "디자인 단계 시작", "검증 돌려줘" 같은 단계 진행 지시에 사용.
argument-hint: "[project-slug] <P1~P8 | next>"
---

# 단계 실행

인자: $ARGUMENTS

당신은 **오케스트레이터**다. `CLAUDE.md` §0(틀·프로젝트 분리), §2(단계표), §3(표준 루프·호출 규칙), §8(안전 규칙), §9(Git 운영)를 기준으로 진행한다.
**팀 산출물을 직접 작성하지 말고 반드시 에이전트에게 위임한다. 틀 보호 영역은 수정하지 않는다.**

---

## 0. 프로젝트·단계 결정
- 첫 토큰이 `projects/<토큰>/` 디렉토리로 존재하면 그것이 `SLUG`, 아니면 `CLAUDE.md` §10 규칙(진행 중 프로젝트가 하나면 자동 선택, 여러 개면 질문)으로 정한다.
- 단계가 비어 있거나 `next`이면 `projects/<SLUG>/pm/STATUS.md` 기준으로 승인 완료된 마지막 단계의 다음 단계.
- 이하 `{PROJECT}` = `projects/<SLUG>`. **모든 에이전트 호출 프롬프트 첫 줄에 `PROJECT: projects/<SLUG>`를 넣는다.**

## 1. 착수 점검
- `{PROJECT}/pm/STATUS.md`에서 이전 게이트가 PM 승인되었는지 확인한다.
  미승인이면 진행하지 않고 PM에게 알린다. PM이 명시적으로 강행을 지시하면 STATUS.md "예외 기록"에 남기고 진행한다.
- 이 단계의 입력 산출물이 존재하고 `status: approved`인지 확인한다.
- `{PROJECT}/shared/tickets/`, `{PROJECT}/pm/requests/`에서 이 단계와 관련된 미해결 건(`status: open|in-progress|reopened`)을 Grep으로 확인한다.
- `git -C {PROJECT} status --short`로 커밋되지 않은 변경을 확인한다 (이전 작업의 미커밋분이 있으면 PM에게 알림).
- `{PROJECT}/pm/STATUS.md`의 해당 단계를 `🔄 진행 중`으로 갱신한다.

## 2. 작성 — 단계별 진행 방식

표의 산출물 경로는 모두 `{PROJECT}` 기준 상대경로다.

| 단계 | 진행 방식 |
|---|---|
| **P1** | `pmo` 작성 → 검토 `planner`, `developer` |
| **P2** | `planner`가 요구사항 → IA → 화면정의서를 작성 → 검토 `designer`, `developer`, `qa` |
| **P3** | ① `designer` 컨셉 시안 2~3안 → **PM에게 시안 선택 요청** (AskUserQuestion, 시안별 요약을 옵션 설명에) → ② `pmo`가 선택 결과를 `shared/decisions/ADR-*`로 기록 → ③ `designer` 디자인 시스템·페이지 디자인·목업 → 검토 `planner`, `developer` |
| **P4** | ① `developer` 기술 설계 → 검토 `devops`, `qa` (루프) → ② `developer` 구현 + 로컬 빌드 확인 + 개발 보고서 → 검토 `designer`(디자인 QA), `planner`(기능 부합) (루프). 이 기간에 `qa`의 테스트 계획·케이스 선작성을 병렬로 지시할 수 있다 |
| **P5** | ① `qa` 테스트 계획·케이스(미작성 시) → 테스트 실행 → DEF 발행 → ② `developer` 결함 수정 → ③ `qa` 재검증·회귀 테스트 → Critical·Major 0건까지 ②③ 반복(최대 3사이클, 초과 시 PM 보고) → ④ `qa` 결과 보고서 → 검토 `developer`, `planner` |
| **P6** | `pmo` 중간보고서 → 검토(사실 확인) `planner`, `designer`, `developer`, `qa`, `devops` → PM 보고. PM이 고객 피드백을 전달하면 `pmo`가 보고서 "고객 피드백 기록"에 정리하고, 변경이 필요한 항목은 `/change-request` 절차로 처리 |
| **P7** | ① `devops` 배포 계획 → 검토 `developer`, `qa` → ② **PM 배포 승인 요청** (배포 대상·호스팅·도메인·롤백 계획·PM 조치 필요 사항 제시) → ③ 승인 시 **릴리스 커밋·태그** (§6-2) → ④ `devops` 호출 프롬프트에 `PM 배포 승인 완료: {일시}`와 `배포 태그: release-v{x.y.z}` 명시하여 배포 실행 → ⑤ `qa` 운영 스모크 테스트(`qa/07_smoke-test-report.md`) → ⑥ `devops` 배포 보고서 (실패 시 롤백 기준에 따라 판단하고 PM 보고) |
| **P8** | ① `devops` 운영 가이드 (필요 정보는 `developer` 티켓으로) + `pmo` 최종 보고서 → 검토 전 팀 → ② `pmo` 회고 회의록(각 팀 WORKLOG·리뷰 이력 기반 Keep/Problem/Try + 틀 개선 제안) → ③ G8 승인 후 **고객 인도 패키지** (§6-3) |

**호출 프롬프트 필수 항목** (`CLAUDE.md` §3): `PROJECT`, 단계·작업 종류, 입력 경로, 템플릿 경로(`templates/docs/…`), 출력 경로(`{PROJECT}/…`), 관련 리뷰·티켓·CR, 라운드 번호.

에이전트 완료 보고의 생성/수정 파일에 `{PROJECT}` 밖 경로가 있으면 즉시 중단하고 PM에게 알린다.

## 3. 교차 검토
- 검토자들을 **한 메시지에서 병렬로** 호출한다.
- 각 검토자 프롬프트에 포함: `PROJECT`, 검토 대상 경로와 버전, 라운드 번호, 리뷰 파일 경로 `{PROJECT}/shared/reviews/{단계}_{대상}_{검토자}_r{n}.md`, 템플릿 `templates/docs/shared/review.md`, "에이전트 정의의 '검토자로서' 관점 적용", R2 이상이면 이전 라운드 리뷰 경로.

## 4. 반영과 수렴
- 판정이 `수정 요청`인 리뷰가 있으면 Owner를 재호출한다: 모든 리뷰 경로를 전달하고, 각 지적에 처리 결과를 리뷰 문서에 기입한 뒤 버전을 올리게 한다.
- 다음 라운드는 **Must를 지적한 검토자만** 재검토한다.
- 3라운드 후 Must 잔존 또는 팀 간 의견 충돌 → `pmo`가 `{PROJECT}/shared/decisions/ADR-*`에 쟁점·선택지·권고 정리 → PM 결정 요청 → 결정을 ADR에 기록 후 Owner 반영.
- 에이전트 완료 보고의 "PM 결정 필요 사항"은 모아 두었다가 게이트 보고에 포함한다(진행을 막는 사항이면 즉시 질문).

## 5. 게이트 준비와 PM 보고
`pmo` 호출: `templates/docs/pm/gate.md`로 `{PROJECT}/pm/gates/G{n}_{slug}.md` 작성, `{PROJECT}/pm/STATUS.md` 갱신(`⏳ 승인 대기`, 열린 티켓·리스크·PM 결정 필요 사항 반영).

PM에게 보고:
```
## [<SLUG>] G{n} {단계명} 게이트 보고
**pmo 권고**: 승인 권고 / 조건부 승인 권고 / 보류 권고
**산출물**: (클릭 가능한 경로 목록)
**핵심 결과**: (3~5줄)
**리뷰 요약**: 라운드 수 · Must/Should 처리 현황
**미해결·이월 사항**:
**PM 결정 필요 사항**:
→ 승인 / 수정 지시 / 보류 중 선택해 주세요.
```

PM 응답 처리:
- **승인** — 게이트 문서 "PM 결정" 절 기입, 산출물 헤더 `status: approved`·`version: 1.0`(이미 1.x면 유지)·`updated` 갱신, STATUS.md `✅ 완료`와 승인일·다음 할 일 갱신 → **§6-1 커밋·태그**.
- **수정 지시** — 지시를 게이트 문서에 기록하고 4단계(반영)로 복귀.
- **보류** — 사유를 STATUS.md에 기록하고 대기.
- **변경 요청 포함** — `.claude/skills/change-request/SKILL.md` 절차를 따른다.

## 6. Git (오케스트레이터만 수행)

커밋 전 공통: `git -C {PROJECT} status --short`로 `.env`, 비밀 정보, `node_modules`, 대용량 파일이 포함되지 않았는지 확인한다. 의심 파일이 있으면 커밋하지 말고 PM에게 알린다.

### 6-1. 게이트 승인
```bash
git -C {PROJECT} add -A
git -C {PROJECT} commit -m "gate(G{n}): {단계명} 승인" -m "gate: pm/gates/G{n}_{slug}.md"
git -C {PROJECT} tag -a G{n} -m "G{n} {단계명} 승인 — {승인 일시}"
```
- 태그 `G{n}`이 이미 있으면(CR 회귀 후 재승인) `G{n}-CR-{nnn}`을 사용한다.

### 6-2. 릴리스 (P7, PM 배포 승인 직후 · 배포 실행 전)
- 버전: 최초 오픈 `v1.0.0`, 이후 CR 기능 추가 `minor`, 결함 수정만 `patch` 증가.
```bash
git -C {PROJECT} add -A
git -C {PROJECT} commit -m "release: v{x.y.z} 배포 승인"      # 변경이 없으면 커밋 생략
git -C {PROJECT} tag -a release-v{x.y.z} -m "PM 배포 승인 {일시} — devops/07_deploy-plan.md"
```

### 6-3. 고객 인도 패키지 (P8, G8 승인 후)
- PM에게 인도 범위를 확인한다 (예: `developer/site devops/08_operation-guide.md pm/08_final-report.md` / 전체 / 내부 리뷰·티켓·WORKLOG 제외 여부).
```bash
mkdir -p {PROJECT}/.delivery
git -C {PROJECT} archive --format=zip -o .delivery/<SLUG>-G8.zip G8 <선택 경로...>
```
- `STATUS.md` 프로젝트 상태를 `종료`로 갱신하고 최종 커밋(`chore(close): 프로젝트 종료`)한다.

### 원격 저장소
원격 저장소 생성·연결·push는 **PM이 요청·승인한 경우에만** 수행한다.
