---
id: MTG-YYYYMMDD-slug
type: kickoff-agenda         # kickoff-agenda(미팅 자료) | kickoff | coordination | gate | retrospective
status: planned              # planned(자료 — 아직 열리지 않음) | held(실제 회의 결과 기록)
phase: P1
participants: []             # 총괄 PM, pmo, planner, designer, developer, qa, devops
date: YYYY-MM-DD
---

# 회의록: (제목)

> ⚠️ **실제로 열린 회의만 회의록으로 쓴다.** 에이전트는 회의를 열 수 없으므로 열리지 않은 회의의 논의·결정을 지어내지 않는다.
> - `kickoff-agenda`(`status: planned`): PM이 고객 미팅에 가져갈 **자료** — §1 목적, §2 안건, "확인할 질문"(QNA의 `open` Q ID), "미팅에서 결정할 사항"만 쓰고 §3~§5는 비워 둔다.
> - 미팅 후 PM이 결과를 알려 주면 pmo가 PM의 전달 내용만으로 §3~§5를 채우고 `status: held`로 바꾼다. 답변은 `/answer`로 QNA에 반영한다.
> - 회고(retrospective)는 산출물·리뷰·ACTIVITY·WORKLOG 기록을 근거로 쓰는 문서이므로 예외로 작성한다.

## 1. 목적

## 2. 안건
1.

### 확인할 질문 (kickoff-agenda)
| Q ID | 질문 | 담당 (고객 측) |
|---|---|---|

### 미팅에서 결정할 사항 (kickoff-agenda)
- 피드백 정책(수정 횟수·응답 기한), 고객 측 결정권자, 자료 제출 일정, 참고 사이트

## 3. 논의 내용
| 안건 | 주요 의견 (팀) | 결론 |
|---|---|---|

## 4. 결정 사항
- (ADR로 기록한 경우 링크)

## 5. 액션 아이템
| # | 내용 | 담당 | 기한 | 상태 |
|---|---|---|---|---|

<!-- 회고(retrospective)일 때 아래 절 사용 -->
## 6. 회고 (retrospective)
| 팀 | Keep | Problem | Try |
|---|---|---|---|
| pmo | | | |
| planner | | | |
| designer | | | |
| developer | | | |
| qa | | | |
| devops | | | |

**프로세스 지표**: 단계별 실측 소요 시간(ACTIVITY 날짜 기준)·대기 요인, 리뷰 라운드 수, 결함 수·재오픈 수, CR·SUG 수, 게이트 반려 수
**틀 개선 제안** (CLAUDE.md·템플릿·에이전트 정의·스킬):
> 프로젝트 작업 중에는 틀을 수정하지 않는다. 여기에 제안만 기록하고, 반영은 사용자가 별도로 요청할 때 틀 저장소에서 수행한다.

| # | 대상 파일 | 제안 내용 | 근거 (이번 프로젝트에서 겪은 문제) |
|---|---|---|---|
