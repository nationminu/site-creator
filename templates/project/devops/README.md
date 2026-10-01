# devops/ — 배포운영팀

검증된 사이트를 운영 환경에 안전하게 배포하고 고객에게 운영을 인계합니다. 에이전트 정의: 틀 저장소 `.claude/agents/devops.md` · 문서 템플릿: 틀 저장소 `templates/docs/devops/`

## 산출물
| 단계 | 파일 | 내용 | 검토자 |
|---|---|---|---|
| P1 | `../shared/reviews/P1_hosting-input_devops.md` | 호스팅·운영 비용 사전 의견 (동적 기능·호스팅 미정 시) | — |
| P4 | `04_environment.md` | 운영 환경 명세 + 배포 설정 파일 초안 | developer, qa |
| P5 | `05_preview.md` | 프리뷰 배포 기록 (PM 승인 시) | — |
| P7 | `07_deploy-plan.md` | 배포 계획서 — DNS·검색 노출·롤백 리허설·체크리스트 | developer, qa |
| P7 | `07_deploy-report.md` | 배포 보고서 — 수행 내역·확인 결과·오픈 후 관찰 | — (PM이 G7에서 확인) |
| — | `release/` | 릴리스 클린 빌드·업로드 묶음 (git 제외) | — |
| P8 | `08_operation-guide.md` | 운영·유지보수 가이드 (고객 운영 담당자용) | 전 팀 |
| — | `WORKLOG.md` | 작업 과정 로그 | — |

## ⚠️ 배포 안전 규칙
운영 배포·DNS 변경·외부 계정 생성/결제는 **PM의 명시적 승인**(`PM 배포 승인 완료: {일시}`)이 있을 때만 실행합니다. (틀 저장소 `CLAUDE.md` §8)

## 검토 참여
P4 기술 설계(빌드·배포·환경 일치), P6·P8 보고서, lite P8 최종 보고서
