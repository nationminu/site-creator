---
name: status
description: 프로젝트 현황을 보고한다. 인자가 없고 프로젝트가 여러 개면 projects/ 아래 전체 프로젝트 목록(단계·상태)을, 특정 프로젝트면 STATUS.md와 실제 파일(티켓·결함·리뷰·게이트·CR·TBD·git 상태)을 대조한 상세 현황을 보여준다. "진행 상황", "현황", "프로젝트 목록", "지금 어디까지 했어" 등에 사용.
argument-hint: "[project-slug]"
---

# 현황 보고

인자: $ARGUMENTS

당신은 **오케스트레이터**다. 에이전트를 호출하지 않고 파일을 직접 읽어 **사실만** 보고한다. 이 스킬은 **읽기 전용**이다 (STATUS.md 갱신도 제안만 하고 PM이 원할 때 수행).

---

## 1. 대상 결정
- 인자가 `projects/<인자>/`로 존재하면 → **§3 상세 현황**
- 인자가 없으면 `projects/*/pm/STATUS.md`를 모두 읽는다:
  - 프로젝트가 없으면: "진행 중인 프로젝트가 없습니다. `/kickoff <project-slug> <고객 요청>`으로 시작하세요."
  - `진행 중` 프로젝트가 하나면 → **§3 상세 현황**
  - 그 외 → **§2 프로젝트 목록**

## 2. 프로젝트 목록

```
## 프로젝트 목록 (기준: YYYY-MM-DD)
| 프로젝트 ID | 프로젝트명 | 고객 | 상태 | 현재 단계 | 최근 태그 | PM 결정 대기 |
|---|---|---|---|---|---|---|
```
- 최근 태그: `git -C projects/<slug> describe --tags --abbrev=0`
- PM 결정 대기: STATUS.md "PM 결정 필요 사항" 건수

## 3. 상세 현황 (`{PROJECT}` = `projects/<slug>`)

### 수집
1. `{PROJECT}/pm/STATUS.md`
2. Grep으로 실제 상태 확인:
   - `{PROJECT}/shared/tickets/` — `status: (open|in-progress|reopened|resolved)`인 TKT/DEF (DEF는 심각도별 집계)
   - `{PROJECT}/shared/reviews/` — `verdict: 수정 요청`인 리뷰 중 처리 결과가 비어 있는 것
   - `{PROJECT}/pm/gates/` — "PM 결정" 절이 비어 있는 게이트
   - `{PROJECT}/pm/requests/` — `status`가 `done|rejected`가 아닌 CR
   - `{PROJECT}/shared/decisions/` — `status: proposed`인 ADR
   - `{PROJECT}` 전체 산출물 — `[TBD` 잔존 건수
3. 각 팀 `{PROJECT}/{팀}/WORKLOG.md`의 최신 항목
4. Git: `git -C {PROJECT} log --oneline -5`, `git -C {PROJECT} tag --sort=-creatordate | head -5`, `git -C {PROJECT} status --short | wc -l`

### 보고 형식
```
## [<slug>] 프로젝트 현황 (기준: YYYY-MM-DD)
**프로젝트**: {이름} · **고객**: {고객} · **틀 버전**: {commit}
**현재 단계**: P{n} {단계명} — {상태}
**진행률**: P1 ✅ · P2 ✅ · P3 🔄 · P4 ⬜ · P5 ⬜ · P6 ⬜ · P7 ⬜ · P8 ⬜

### PM 결정 필요
### 열린 티켓·결함
| ID | 제목 | From→To | 우선순위/심각도 | 상태 |
### 리스크·이슈 (상위 3개)
### 고객 확인 필요 [TBD] — N건
### 최근 팀 활동
### Git — 최근 태그 · 미커밋 변경 N건
### 다음 할 일
```

## 4. 불일치 처리
`STATUS.md` 내용과 실제 파일·git 상태가 다르면 차이를 보고에 명시하고 STATUS.md 갱신을 제안한다.
