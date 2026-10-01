# 스택 프리셋 · 기술 스택 카탈로그

> 기준 문서. 스택 선택·프리셋·툴체인·표준 명령에 관한 규칙은 이 파일에만 정의한다. 배포 방식은 `.claude/agents/devops.md`, 프리셋별 검증 환경은 `.claude/agents/qa.md`.

## 1. 프리셋 개요
호스팅 환경이 가능한 스택을 결정하므로, 프론트·백엔드·DB·호스팅·배포 방식을 **한 묶음**으로 고른다.

| 프리셋 | 이런 경우 | 프론트 | 백엔드 | DB | 호스팅·배포 |
|---|---|---|---|---|---|
| **`static`** | 동적 기능 없음 (폼·CMS는 외부 서비스로 충분) | Astro | 없음 | 없음 | 정적 호스팅 또는 국내 공유 호스팅 업로드 |
| **`kr-shared`** | 국내 공유 호스팅(카페24·가비아 등) 사용·희망 + 게시판·관리자 등 동적 기능 | Astro (정적 빌드) | CodeIgniter 4 (SSH·Composer 가능 시 Laravel) | 호스팅의 MariaDB / MySQL | SFTP 업로드 묶음, 앱은 웹 루트 밖, 같은 도메인 `/api` |
| **`react-spring`** | 회원·결제·외부 연동이 많거나 장기 운영·확장 필요 + 서버 운영 가능 | Next.js (페이지별 SSG/SSR) | Spring Boot | PostgreSQL | 프론트(정적/Node) + 컨테이너·VM + 관리형 DB |
| **`custom`** | 위 셋에 맞지 않음 (고객 지정 기술·기존 시스템 등) | Astro / Next.js / Nuxt / 순수 HTML | 없음 / Spring Boot / Laravel / CodeIgniter 4 / Django | 없음 / PostgreSQL / MySQL·MariaDB | 기술 설계·배포 계획에서 정의 |

프리셋의 "호스팅·배포"는 대표 예다. 실제 **운영 환경**(정적 호스팅·공유 호스팅·PaaS·Docker VM·K8s·리눅스 직접 설치)과 **로컬 개발 환경**(native·docker·hybrid)은 `.claude/reference/environments.md`에서 프리셋과 함께 G2에 확정한다.

**판단 순서**: ① 동적 기능이 외부 서비스로 충분하면 `static` → ② 국내 공유 호스팅이 정해져 있거나 원하면 `kr-shared` → ③ 고객 지정 기술·기존 시스템이 있으면 `custom` → ④ 그 외에는 규모·운영 비용으로 `kr-shared`와 `react-spring`을 비교해 PM이 고른다.

**결정 시점**
- **P1**: devops의 호스팅 사전 의견을 바탕으로 pmo가 계획서 "기술·환경 초기 방향"에 **잠정 프리셋**과 근거, 호스팅 확인 질문(아래 `kr-shared` "호스팅 확인 항목" 등)을 제시한다. STATUS에는 `{프리셋} (잠정)`으로 기록한다.
- **G2**: 요구사항으로 동적 기능 범위가 확정되면 PM이 프리셋을 확정하고, pmo가 ADR로 기록하고 STATUS의 "(잠정)"을 지운다. `custom`이면 프론트·백엔드·DB 선택도 함께 기록한다.
- **P4**: developer는 프리셋 안의 세부(버전, CI4/Laravel, 페이지별 SSG/SSR 등)만 정한다. **G2 이후 프리셋 변경은 `CLAUDE.md` §7 CR 절차**를 따른다. 프리셋을 벗어나야 하는 사정이 생기면 developer는 구현하지 말고 `to-pmo` 티켓을 발행한다.
- 프리셋은 진행 모드(`.claude/reference/modes.md`)·디자인 프로필(`.claude/reference/design-profile.md`)과 독립적이다.
- 프리셋 안에서도 **가장 단순한 선택**을 우선한다: 외부 서비스(폼·헤드리스 CMS·예약·결제)로 충분한 기능은 직접 구현하지 않는다. `custom`의 카탈로그 밖 스택은 ADR로 PM 결정을 받는다.

## 2. 프리셋 상세

### `static` — 정적 사이트
| 항목 | 내용 |
|---|---|
| 구성 | Astro (SSG) + TypeScript strict. 백엔드·DB 없음 |
| 동적 요소 | 문의 폼은 외부 폼 서비스, 콘텐츠 수정 요구가 있으면 헤드리스 CMS 검토 (개인정보 수집 시 처리방침 고지) |
| 결과물 | `dist/` 정적 파일 — 정적 호스팅 또는 국내 공유 호스팅 웹 루트에 업로드 가능 |
| 표준 명령 | 프론트엔드 열만 사용 |

### `kr-shared` — 국내 공유 호스팅 (카페24·가비아 등)
| 항목 | 내용 |
|---|---|
| 구성 | Astro 정적 빌드 + PHP API(**CodeIgniter 4 기본**, SSH·Composer 가능하고 PHP 버전이 맞으면 Laravel 허용) + 호스팅 제공 MariaDB/MySQL |
| 서버 배치 | 앱 본체·`vendor/`·`.env`는 **웹 루트 밖**(예: `~/app/`), 웹 루트(`~/www/` 등)에는 Astro `dist/` + `api/index.php`(앱 `public/index.php` 사본, 경로 수정) + `.htaccess`(`/api/*` → `api/index.php`). 웹 루트 안 `.htaccess` 차단만으로 비밀 파일을 보호하는 배치는 쓰지 않는다 |
| API 연결 | **같은 도메인 `/api/*`** — CORS 불필요, 세션 쿠키·CSRF 그대로 사용 |
| 서버 제약 | 서버에 Node 없음 → 프론트는 로컬/CI에서 빌드 후 업로드. 서버 상주 프로세스 없음 → 큐는 동기 처리 또는 cron(지원 시)으로 대체 |
| 의존성 | `composer.json`의 `config.platform.php`를 **운영 PHP 버전으로 고정**하고 `composer install --no-dev -o` 결과 `vendor/`를 업로드 묶음에 포함 (SSH·Composer가 있어도 같은 방식 권장) |
| DB 마이그레이션 | 프레임워크 마이그레이션 + **버전 번호가 붙은 SQL 파일**(`database/sql/V{nnn}__{설명}.sql`)을 함께 유지. SSH가 없으면 SQL 파일을 호스팅 DB 관리도구로 적용 |
| 로컬·검증 환경 | `compose.yaml`로 **운영과 같은 버전**의 `php:{버전}-apache` + `mariadb:{버전}`(또는 mysql) 구성, 필요한 PHP 확장 동일하게 설치. 폼 메일은 로컬 메일 확인 도구(예: Mailpit)로 받는다 |
| 메일 | 호스팅 메일 발송 제한 확인, 부족하면 외부 메일 발송 서비스 |
| 호스팅 확인 항목 | PHP 버전·확장(mbstring·intl·pdo_mysql·openssl·curl·fileinfo·gd), SSH/SFTP, Composer, cron, DB 종류·버전·용량·외부 접속, 웹 루트 위치·상위 디렉토리 쓰기 권한, `.htaccess`·mod_rewrite, 무료 SSL·자동 갱신, 업로드 용량 제한, 메일 발송 제한, 백업 주기 |

### `react-spring` — Next.js + Spring Boot + PostgreSQL
| 항목 | 내용 |
|---|---|
| 구성 | `web/` Next.js(App Router) + TypeScript strict, `api/` Spring Boot REST API, PostgreSQL |
| 렌더링 | **페이지별로 SSG/SSR을 정한다.** 콘텐츠 페이지는 SSG(정적 생성) 우선, 개인화·실시간 데이터·요청 시점 SEO가 필요한 페이지만 SSR. 페이지별 렌더링 방식과 근거를 기술 설계에 표로 기록. 전 페이지가 SSG면 static export로 Node 서버 없이 배포 |
| API 연결 | 운영은 **같은 사이트**로 구성(리버스 프록시 `/api` 또는 `api.<도메인>` 서브도메인). 인증은 세션 쿠키(HttpOnly·Secure·SameSite) 우선, CORS는 프론트 도메인만 허용. Next 서버 기능은 렌더링·BFF 수준으로만 쓰고 업무 로직은 Spring에 둔다 |
| 백엔드 | Spring Security, Bean Validation, springdoc-openapi, Spring Data JPA + Flyway, Actuator 헬스체크 |
| 로컬 | `compose.yaml`(PostgreSQL, 메일 확인 도구 예: Mailpit) + `web` `npm run dev` + `api` `./gradlew bootRun`, 개발 시 Next rewrites로 `/api` 프록시 |
| 운영 비용 | 프론트(정적 또는 Node) + Spring 런타임 + 관리형 PostgreSQL — 월 비용 추정을 기술 설계·배포 계획에 기록 |

### `custom` — 개별 선택
| 항목 | 내용 |
|---|---|
| 구성 | 프론트(Astro / Next.js / Nuxt / 순수 HTML) × 백엔드(없음 / Spring Boot / Laravel / CodeIgniter 4 / Django) × DB(없음 / PostgreSQL / MySQL·MariaDB)를 §3 카탈로그에서 각각 선택 |
| 호환성 확인 | 호스팅과 맞지 않는 조합 금지 — 국내 공유 호스팅에서는 Node SSR·Spring Boot·Django·PostgreSQL 불가 |
| 근거 | 왜 다른 프리셋이 아닌지(고객 지정 기술, 기존 시스템 등)를 ADR에 기록 |
| 세부 규칙 | 가장 가까운 프리셋의 규칙(서버 배치·API 연결·로컬 환경)을 준용하고, 다른 부분만 기술 설계에 명시 |

## 3. 스택 표준 카탈로그

### 프론트엔드
| 구분 | 표준 | 언어 | 쓰는 경우 |
|---|---|---|---|
| **정적** | Astro (SSG) | TypeScript `strict` | 회사 소개·랜딩·포트폴리오 등 정보 제공형. 동적 기능이 있어도 콘텐츠 페이지는 Astro로 두고 동적 기능만 백엔드 API로 붙이는 구성을 우선 검토 |
| React | Next.js | TypeScript `strict` | 상호작용이 많은 화면, SSR이 필요한 경우 |
| Vue | Nuxt | TypeScript `strict` | 위와 같으나 고객·운영 인력이 Vue를 선호할 때 |
| 순수 HTML/CSS/JS | — | JavaScript | **예외** — 1~2페이지이고 고객이 HTML을 직접 수정해야 하는 경우만. 사유를 기술 설계에 기록 |

- Next·Nuxt의 서버 기능(API routes, server actions 등)은 렌더링 용도로만 쓰고, **업무 로직·DB 접근은 아래 백엔드 카탈로그 스택으로 구현한다** (Node 백엔드는 틀 표준이 아니다).
- 프론트엔드 공통: npm, Node LTS 고정(`package.json` `engines` + `.nvmrc`), lock 파일 커밋, ESLint + Prettier, `astro check`/`tsc --noEmit`.

### 백엔드 (동적 요구사항이 있을 때만)
| 스택 | 프레임워크 | 빌드·패키지 | 린트·포맷·정적 분석 | 테스트 | DB 접근·마이그레이션 | 쓰는 경우 |
|---|---|---|---|---|---|---|
| **Java** | Spring Boot (+ Spring Security, Bean Validation, springdoc-openapi) | Gradle Kotlin DSL + Gradle Wrapper | Spotless + Checkstyle | JUnit 5 + Spring Boot Test (+ Testcontainers) | Spring Data JPA + Flyway | `react-spring` 프리셋. 장기 운영·확장 |
| **PHP** | CodeIgniter 4 (공유 호스팅 기본) / Laravel (SSH·Composer 가능 시) | Composer | Laravel Pint + PHPStan(Larastan) / CI4: PHP-CS-Fixer + PHPStan | Pest(Laravel) / PHPUnit(CI4) | Eloquent 마이그레이션 / CI4 Migrations | 공유 호스팅, 기존 PHP 운영 환경, 낮은 운영 비용 |
| **Python** | Django (+ API 필요 시 Django REST framework) | uv (`pyproject.toml` + `uv.lock`) | Ruff (lint + format) | pytest-django | Django ORM 마이그레이션 | 관리자 화면(Django Admin) 중심, 데이터 처리 |

- **DB**는 프리셋이 정한다(`kr-shared`: 호스팅의 MariaDB/MySQL, `react-spring`: PostgreSQL). **로컬 DB는 운영과 같은 종류·버전**으로 맞춘다. 로컬 DB는 `developer/site/compose.yaml`(Docker Compose)로 띄우는 것을 권장하고, Docker를 쓸 수 없으면 대안(SQLite 등)과 운영 DB와의 차이를 기술 설계에 기록한다.
- **화면 렌더링 방식**은 기술 설계에서 정한다: ① Astro 등 프론트엔드 + 백엔드 REST API 분리, ② 백엔드 템플릿(Thymeleaf·Blade·Django Template)으로 서버 렌더링. 선택 근거(호스팅 수, SEO, 운영 난이도)를 기록한다.
- **버전 정책**: 착수 시점에 공식 지원 중인 **LTS 또는 최신 안정 버전**을 고르고(Java LTS, Django LTS 우선, PHP·Laravel은 보안 지원 기간이 남은 버전), 기술 설계 "버전·지원 종료" 표에 정확한 버전과 EOL 일자를 고정한다. 틀은 버전 숫자를 고정하지 않는다.

## 4. 디렉토리 구성
- 단일 애플리케이션(Astro 단독, 백엔드 서버 렌더링 단독): `developer/site/`가 앱 루트.
- 프론트엔드 + 백엔드 분리: `developer/site/web/`(프론트), `developer/site/api/`(백엔드), 공용 `developer/site/README.md`·`compose.yaml`.

## 5. 표준 명령 매핑
qa·devops는 기술 설계의 이 매핑만 보고 실행한다. 스택별 실제 명령을 기술 설계 "표준 명령 매핑" 표와 `developer/site/README.md`에 적는다. Windows에서는 `./gradlew` 대신 `gradlew.bat`를 쓴다.

| 표준 명령 | 프론트엔드 (npm) | Java (Gradle) | PHP Laravel | PHP CI4 | Python Django (uv) |
|---|---|---|---|---|---|
| install | `npm ci` | (Wrapper가 자동 처리) | `composer install` | `composer install` | `uv sync` |
| dev | `npm run dev` | `./gradlew bootRun` | `php artisan serve` | `php spark serve` | `uv run python manage.py runserver` |
| build | `npm run build` | `./gradlew bootJar` | `composer install --no-dev -o` (+ 프론트 자산 `npm run build`) | `composer install --no-dev -o` | `uv run python manage.py collectstatic --noinput` |
| preview | `npm run preview` | `java -jar build/libs/*.jar` | — | — | — |
| test | `npm test` | `./gradlew test` | `php artisan test` | `vendor/bin/phpunit` | `uv run pytest` |
| lint | `npm run lint` | `./gradlew spotlessCheck checkstyleMain` | `vendor/bin/pint --test` · `vendor/bin/phpstan analyse` | `vendor/bin/php-cs-fixer fix --dry-run` · `vendor/bin/phpstan analyse` | `uv run ruff check .` · `uv run ruff format --check .` |
| check (lint+타입+테스트) | `npm run check` | `./gradlew check` | `composer check` (scripts에 정의) | `composer check` (scripts에 정의) | lint + `uv run pytest` |
| migrate | — | Flyway (기동 시 자동 / `./gradlew flywayMigrate`) | `php artisan migrate` | `php spark migrate` | `uv run python manage.py migrate` |
| audit | `npm audit` | OWASP dependency-check 플러그인 | `composer audit` | `composer audit` | `uv run pip-audit` (dev 의존성) |
