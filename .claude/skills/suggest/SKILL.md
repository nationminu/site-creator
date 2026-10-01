---
name: suggest
description: PM이나 고객이 제안하는 시안·디자인 레퍼런스·기능·원고·불편 사항을 캡처·파일·URL·설명으로 접수해 SUG 문서로 기록하고, 관련 팀(designer·planner·developer·qa)의 검토와 pmo 종합을 거쳐 PM 결정을 받은 뒤, 진행 중 산출물 반영·CR 전환·콘텐츠 수급·결함 등록으로 처리한다. "이 캡처처럼 바꾸고 싶대", "고객이 이 기능 원해", "참고 사이트 보냈어", "자료 받았어" 등에 사용.
argument-hint: "[project-slug] <제안 설명> [파일 경로·URL …]"
---

# 제안 접수·검토·반영

인자: $ARGUMENTS

당신은 **오케스트레이터**다. `CLAUDE.md` §0·§5·§7·§8(특히 "기능 결정권")과 `.claude/reference/git-ops.md`를 따른다.
**제안은 PM 결정 전까지 어떤 산출물에도 반영하지 않는다.** 틀 변경 제안이면 이 스킬이 아니라 틀 수정 요청으로 PM에게 확인한다.

---

## 0. 프로젝트 결정
- 첫 토큰이 `projects/<토큰>/`으로 존재하면 그것이 `SLUG`, 아니면 `CLAUDE.md` §10 규칙으로 정한다. 이하 `{PROJECT}` = `projects/<SLUG>`.
- 이미 승인된 산출물을 바꾸자는 것이 명백하고 첨부·검토가 필요 없는 단순 변경이면 `/change-request`를 바로 써도 된다고 PM에게 안내한다(선택은 PM).

## 1. 접수 (오케스트레이터)
1. `{PROJECT}/pm/requests/`에서 기존 SUG 번호를 확인하고 다음 번호(`SUG-001`부터)를 부여한다.
2. **첨부 수집** → `{PROJECT}/pm/requests/attachments/SUG-{nnn}/`
   - 인자로 받은 파일 경로, `{PROJECT}/inbox/`의 파일(README.md 제외)을 이 폴더로 **이동**한다. 프로젝트 밖 경로의 파일은 복사한다.
   - 파일명은 영문 kebab-case로 바꾸고 원래 이름을 표에 적는다. 10MB를 넘는 파일·영상은 커밋하지 말고 PM에게 보관 위치를 묻는다.
   - **비밀번호·계정·개인정보가 보이는 파일은 저장하지 않고** PM에게 알린다.
   - 채팅에 붙여 넣은 이미지는 파일로 저장할 수 없다. 본 내용을 텍스트로 자세히 적고, 원본이 필요하면 `inbox/`에 저장해 달라고 요청한다.
   - URL(참고 사이트)은 표에 기록한다. 화면 캡처가 필요하면 `npx --yes playwright screenshot --full-page <URL> <첨부 폴더>/ref-<n>.jpg`로 저장할 수 있다(외부 사이트 화면은 **참고용**이며 그대로 복제하지 않는다).
3. `templates/docs/pm/suggestion.md`로 `{PROJECT}/pm/requests/SUG-{nnn}_{slug}.md`를 만든다. "제안 원문"에 PM이 전달한 말을 **수정 없이** 기록하고, `type`(design·feature·content·issue·other)과 `phase_at_receipt`를 적는다(`status: analyzing`).
4. `{PROJECT}/pm/STATUS.md` "제안(SUG)" 표에 추가 → `ACTIVITY.md`에 `* 이슈: SUG-{nnn} 접수 — {요약} ({제안자})` → 커밋 `chore: SUG-{nnn} 접수`.

## 2. 검토 (유형별 병렬 호출)
각 프롬프트에 `PROJECT`, 진행 모드, (확정 시) 스택 프리셋, SUG 문서·첨부 경로, 현재 단계와 관련 산출물 경로, 출력 경로 `{PROJECT}/shared/reviews/SUG-{nnn}_review_{팀}.md`를 넣는다. 이미지 첨부는 Read로 열어 보라고 명시한다.

| 유형 | 호출 | 검토 관점 |
|---|---|---|
| `design` (시안·레퍼런스·캡처) | `designer` (+ 구현 영향이 크면 `developer`) | 채택할 요소(레이아웃·색·타이포·모션), 선택된 컨셉·디자인 시스템과의 충돌, 접근성(대비·모션), **저작권 위험**(타 사이트 디자인·이미지 복제 금지 — 요소를 참고해 재해석), 반영 방식 2~3안 |
| `feature` (기능) | `planner` + `developer` | 관련 REQ·SCR, 신규 기능이면 요구사항 "제안 기능" 표 등록안, 구현 가능성·작업량·스택 프리셋 적합성·외부 서비스(비용·개인정보) |
| `content` (원고·사진·로고·자료) | `planner` (+ 이미지·로고면 `designer`) | 해당 REQ-C, 콘텐츠 수급 계획 충족 여부, 품질(해상도·형식)·사용 권리, 반영 위치 |
| `issue` (문제·불편) | `qa` (+ 원인 파악이 필요하면 `developer`) | 재현 여부, 결함(DEF)인지 / 요구사항 밖 변경인지 판정 |

lite 모드에서는 검토자를 표의 첫 팀 1명으로 하고 `model: sonnet`으로 호출한다(`.claude/reference/modes.md`). 단 `design` 유형의 이미지 비교 검토는 기본 모델로 한다.
호출이 끝나면 커밋 `review: SUG-{nnn} 검토 — {팀들}` (ACTIVITY `검토` 항목).

## 3. 종합과 PM 결정
1. `pmo`를 호출해 SUG 문서 "검토 의견"·"종합"을 작성하게 한다 — 분류(진행 중 산출물 반영 / 승인된 산출물 변경 → CR / 콘텐츠 수급 처리 / 결함 → DEF), 영향 산출물, 일정·비용·리스크, 권고 (`status: pending-approval`). 커밋.
2. PM에게 보고하고 AskUserQuestion으로 결정을 받는다 — 옵션: 반영 / 부분 반영 / 보류 / 미반영. 디자인 제안은 반영 방식 2~3안을 옵션으로 제시하고, 가능하면 첨부 캡처와 관련 목업 스크린샷 경로를 함께 보여 준다.
```
## [<SLUG>] SUG-{nnn} 제안 검토 결과
**제안**: (요약 · 제안자 · 첨부 n건)
**검토 요지**: (팀별 1줄)
**분류·영향**: (반영 위치 / CR 필요 여부 / 일정·비용)
**pmo 권고**:
→ 반영 / 부분 반영 / 보류 / 미반영 중 선택해 주세요.
```
3. 결정을 SUG 문서 "PM 결정"에 기록하고 `status`(accepted·partially-accepted·rejected·on-hold)를 갱신 → ACTIVITY `결정` 항목 → 커밋 `docs: SUG-{nnn} PM 결정 — {결정}`.

## 4. 반영 (분류별)
| 분류 | 처리 |
|---|---|
| **진행 중 산출물에 반영** (영향 산출물이 아직 `approved`가 아님) | Owner 에이전트를 호출해 반영 지시(SUG 경로·결정 범위 전달) → 버전 증가, 변경 이력에 SUG ID → 이후 현재 단계의 표준 루프(검토·게이트)에 포함 |
| **승인된 산출물 변경** | `.claude/skills/change-request/SKILL.md` 절차로 CR 생성 — CR "요청 원문"에 SUG 결정 내용을 인용하고 SUG `cr_ref`에 CR ID 기록 (영향도 분석은 SUG 검토 의견을 재사용) |
| **기능 제안** | `CLAUDE.md` §8 — G2 전이면 요구사항 "제안 기능" 표에 PM 결정을 기록하고 planner가 REQ-F로 반영, G2 후면 CR |
| **콘텐츠 수급** | 승인된 자료를 `planner`(원고) / `designer`(이미지·로고 → `design/assets/` + `SOURCES.md`)가 반영하고 REQ-C 확보 상태·STATUS "TBD·콘텐츠 수급" 갱신 |
| **결함** | `qa`가 `DEF-*` 발행 → P5 결함 사이클 |
| **보류·미반영** | 사유를 SUG 문서와 STATUS에 기록하고 종료(보류는 재검토 시점 명시) |

반영이 끝나면 SUG 문서 "반영 결과"와 `status: applied`를 기록하고 ACTIVITY `수정` 항목과 함께 커밋한다.
