# 실행 환경 (로컬 개발 환경 · 운영 환경)

> 기준 문서. 개발자가 로컬에서 어떻게 개발·검증하고, 사이트를 어떤 운영 환경에 올리는지에 관한 규칙이다. 스택(무엇으로 만드나)은 `stack-presets.md`, 실행 환경(어디서 어떻게 돌리나)은 이 문서가 정한다.

## 1. 설정 두 가지와 결정 시점

| 설정 | 값 | 정하는 시점 | 기록 |
|---|---|---|---|
| **로컬 개발 환경** (`local-env`) | `native` / `docker` / `hybrid` | P1에서 도구 설치 현황 확인 → **G2에서 PM 확정** (스택 프리셋과 함께) | STATUS "로컬 개발 환경", ADR |
| **운영 환경** (`prod-env`) | `static-hosting` / `shared-hosting` / `paas` / `docker-vm` / `k8s` / `linux-native` | P1 잠정(devops 호스팅 사전 의견) → **G2 확정** → P4 devops "운영 환경 명세" 작성 | STATUS "운영 환경", ADR, `devops/04_environment.md` |

- G2 이후 변경은 `CLAUDE.md` §7 CR이다.
- 운영 서버 접속 정보(계정·비밀번호·키·kubeconfig)는 **문서·저장소·채팅 어디에도 기록하지 않는다.** 보관 위치와 전달 방식만 적고, 접속이 필요한 작업은 PM 조치 또는 PM 승인 후 PM이 제공한 방식으로만 한다(`CLAUDE.md` §8).

## 2. 로컬 개발 환경 (`local-env`)

| 값 | 구성 | 장점 | 단점·주의 | 권장 |
|---|---|---|---|---|
| `native` | 런타임(Node·JDK·PHP·Python)과 DB를 개발 PC에 직접 설치 | 빠르고 IDE 연동이 쉬움, Docker 불필요 | 운영과 버전 차이 위험, PC마다 설정 다름 | `static` (Node만 필요) |
| `docker` | 런타임·DB·메일 도구를 모두 `compose.yaml` 컨테이너로 실행, 소스는 볼륨 마운트 | **운영과 같은 버전** 재현, PC 설정 최소(Docker만) | Docker Desktop 필요, Windows는 WSL2 권장(파일 감시·속도), 초기 이미지 다운로드 시간 | `kr-shared` (공유 호스팅 PHP·DB 버전 재현) |
| `hybrid` | 런타임은 직접 설치, **DB·메일 확인 도구 등 서비스만 Docker** | 개발 편의 + 서비스 버전 일치 | 런타임 버전은 수동 관리(`.nvmrc`·Gradle toolchain·`.python-version`) | `react-spring`, 백엔드 있는 `custom` |

**공통 규칙**
- 어떤 값이든 `developer/site/README.md`에 **처음 받은 사람이 그대로 따라 하는 설치·실행 절차**와 필요한 도구·버전을 적고, 표준 명령 매핑(`stack-presets.md` §5)이 그 환경에서 동작해야 한다.
- `docker`·`hybrid`의 `compose.yaml`은 `developer/site/`에 두고, 서비스 버전은 운영 환경 명세와 일치시킨다. 포트 충돌을 피하도록 포트를 `.env`로 바꿀 수 있게 한다.
- qa도 **같은 로컬 개발 환경**으로 검증한다(qa 도구 자체는 `qa/tools/`의 Node).
- 필요한 도구가 PC에 없으면 에이전트가 설치하지 않는다 — "PM 조치 필요 사항"으로 설치 안내(도구·버전·공식 설치 페이지)를 보고한다.
- Docker를 쓸 수 없으면 `native`로 바꾸고, 운영 환경과의 차이(버전·DB 종류)를 기술 설계 "리스크"에 적는다.

**P1 도구 확인** (오케스트레이터 — kickoff 사전 점검 때): `node -v`, `docker --version`, `docker compose version`, `java -version`, `php -v`, `python3 --version`(Windows는 `python --version`)을 실행해 결과를 QNA에 PM 답변(출처: 환경 확인)으로 기록한다. 없는 도구는 "없음"으로 적는다.

## 3. 운영 환경 (`prod-env`)

| 값 | 구성 | 맞는 프리셋 | devops가 만드는 배포 설정 (`{PROJECT}` 안) | 롤백 |
|---|---|---|---|---|
| `static-hosting` | 정적 호스팅 (Cloudflare Pages·Netlify·Vercel·GitHub Pages 등) | `static`, `react-spring`(전 페이지 SSG 프론트) | 호스팅 설정 파일(`_headers`·`_redirects`·`netlify.toml`·`vercel.json` 등) | 이전 배포로 되돌리기 |
| `shared-hosting` | 국내 공유 호스팅 (카페24·가비아 등, Apache·PHP·MariaDB) | `static`, `kr-shared` | 업로드 묶음 구성, `.htaccess` | 이전 앱 디렉토리·묶음 + DB 백업 |
| `paas` | 관리형 컨테이너·앱 플랫폼 (클라우드 PaaS) | `react-spring`, `custom` | 플랫폼 설정(앱 정의·환경 변수 이름·헬스체크), `Dockerfile`(developer) | 이전 릴리스·이미지 태그 |
| `docker-vm` | 리눅스 VM + Docker Compose + 리버스 프록시(Nginx 등) + TLS | `react-spring`, `kr-shared`(VPS), `custom` | `compose.prod.yaml`, 리버스 프록시 설정, TLS 갱신 방법, 배포 스크립트 | 이전 이미지 태그로 재기동 + DB 백업 |
| `k8s` | 쿠버네티스 클러스터 (고객 보유·관리형) | `react-spring`, `custom` | 이미지 빌드·레지스트리 푸시 절차, 매니페스트(Deployment·Service·Ingress·ConfigMap, Secret은 **이름만**) 또는 Helm 차트, 헬스체크·리소스 요청 | `rollout undo` / 이전 이미지 태그 |
| `linux-native` | 리눅스 VM에 런타임 직접 설치 + systemd + Nginx + TLS | `react-spring`, `kr-shared`(VPS), `custom` | systemd 유닛, Nginx 설정, 설치·배포 스크립트, TLS 갱신 | 이전 릴리스 디렉토리로 심볼릭 링크 전환 + DB 백업 |

- 컨테이너 기반(`paas`·`docker-vm`·`k8s`)이면 **`Dockerfile`은 developer가** `developer/site/`(분리 구성이면 `web/`·`api/`)에 작성한다 — 멀티 스테이지 빌드, 비루트 사용자, 헬스체크 엔드포인트. 배포 설정은 devops가 작성한다.
- `k8s`는 대개 고객 IT 조직이 운영한다. devops는 매니페스트·배포 가이드를 만들고, **실제 클러스터 적용은 고객 측 담당자 또는 PM 승인 후 PM이 제공한 방식으로만** 한다.
- 프리셋과 운영 환경이 맞지 않으면(예: `kr-shared` + `k8s`) G2 전에 PM에게 알리고 `custom`으로 정리한다.

## 4. 운영 서버 확인 질문 (QNA 등록용)
운영 환경이 `docker-vm`·`k8s`·`linux-native`·`paas`이거나 고객이 서버를 직접 운영하면 P1~P2에 아래를 질문한다. (`shared-hosting`은 `stack-presets.md` `kr-shared` "호스팅 확인 항목")

| 구분 | 질문 |
|---|---|
| 서버 | 서버 위치(클라우드 업체·사내·IDC), OS·배포판·버전, CPU·메모리·디스크, 서버 대수 |
| 접근 | 우리가 접근 가능한가(SSH·sudo·콘솔), 접근 불가면 배포 수행자(고객 IT 담당)와 전달 방식 |
| 컨테이너 | Docker·Compose 설치 여부, 쿠버네티스 버전·Ingress 컨트롤러·네임스페이스·이미지 레지스트리 |
| 네트워크 | 도메인·DNS 관리 주체, 방화벽·사내망 제한, 외부 API·메일 발송 허용 여부 |
| TLS | 인증서 발급 방식(Let's Encrypt·구매 인증서·로드밸런서) |
| DB | 관리형 DB 사용 여부, 버전, 백업 주체·주기 |
| 운영 | 모니터링·로그 수집 도구, 배포 승인 절차(변경 관리), 운영 시간대·점검 가능 시간 |
| CI/CD | 사용 중인 CI(GitHub Actions·GitLab CI·Jenkins 등), 저장소 위치 |
