---
doc_id: OPS-ENV
title: 운영 환경 명세
phase: P4
owner: devops
version: 0.1
status: draft
reviewers: [developer, qa]
inputs: [pm/01_project-plan.md, pm/requests/QNA.md, shared/decisions/ADR-*]   # 기술 설계와 병렬 작성 — Dockerfile 등 기술 설계 연동 항목은 검토 시 대조
updated: YYYY-MM-DD
---

# 운영 환경 명세

> 기준: `.claude/reference/environments.md`. 접속 정보(계정·비밀번호·키·kubeconfig)는 **절대 기록하지 않는다** — 보관 위치·전달 방식만 적는다.

## 1. 결정 사항
| 항목 | 값 | 근거 (ADR · Q ID) |
|---|---|---|
| 스택 프리셋 | | |
| 운영 환경 (`prod-env`) | static-hosting / shared-hosting / paas / docker-vm / k8s / linux-native | |
| 로컬 개발 환경 (`local-env`) | native / docker / hybrid | |
| 스테이징·프리뷰 | 있음 (방식) / 없음 | |

## 2. 운영 서버·플랫폼
| 항목 | 내용 | 확인 출처·일자 |
|---|---|---|
| 위치·업체 | | |
| OS·배포판·버전 | | |
| 사양 (CPU·메모리·디스크) | | |
| 런타임·컨테이너 (Docker·Compose·K8s 버전) | | |
| 리버스 프록시·TLS | | |
| DB (종류·버전·관리형 여부·백업) | | |
| 네트워크 제한 (방화벽·외부 API·메일) | | |
| 모니터링·로그 | | |
| 접근 방식 · 배포 수행자 | 우리 / 고객 IT (담당·전달 방식) | |
| 접속 정보 보관 위치 | (값 기록 금지) | |

## 3. 로컬 ↔ 운영 버전 일치
| 구성 요소 | 로컬 (compose·런타임) | 운영 | 차이·대응 |
|---|---|---|---|
| 런타임 | | | |
| DB | | | |
| 웹 서버·프록시 | | | |

## 4. 배포 설정 산출물
| 파일 | 작성 | 내용 | 리뷰 |
|---|---|---|---|
| `Dockerfile` (컨테이너 기반) | developer | 멀티 스테이지, 비루트, 헬스체크 | devops |
| `compose.prod.yaml` / 매니페스트·Helm / systemd·Nginx / 호스팅 설정 | devops | | developer |

## 5. 환경 변수·비밀 정보 (이름만)
| 이름 | 용도 | 설정 위치 (플랫폼 설정·Secret·서버 `.env`) | 준비 주체 |
|---|---|---|---|

## 6. 배포·롤백 방식 요약
- 배포:
- 롤백:
- 배포 승인 절차 (고객 측 변경 관리 포함):

## 7. 미확인 사항
| Q ID | 내용 | 영향 |
|---|---|---|

## 변경 이력
| 버전 | 일자 | 작성자 | 내용 | 관련 리뷰/CR |
|---|---|---|---|---|
| 0.1 | | devops | 최초 작성 | |
