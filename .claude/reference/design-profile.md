# 디자인 프로필 (선택)

> 기준 문서. 디자인 프로필에 관한 규칙은 이 파일에만 정의한다. 디자이너 작업 규칙은 `.claude/agents/designer.md` "디자인 프로필별 규칙".

프로젝트별로 켜는 선택 규칙이다. 켜고 끄는 결정은 PM이 하며, pmo가 `{PROJECT}/shared/decisions/ADR-*`에 기록하고 `{PROJECT}/pm/STATUS.md`의 "스타일 체계"·"Claude Design"에 반영한다. 항목이 없으면 `css-vars`·`off`로 본다. **G2 승인 이후 변경은 CR로 처리한다.**

| 프로필 | 기본값 | 정하는 시점 | 내용 |
|---|---|---|---|
| **스타일 체계** | `css-vars` | P1~P2 (늦어도 G2 전) | `css-vars`: 토큰을 CSS 변수로 정의. `tailwind`: 토큰을 Tailwind 테마로도 정의하고, 화면은 Tailwind 유틸리티와 테마 토큰으로만 구성한다. planner는 `tailwind` 선택 시 기술 제약 `REQ-N-*`로 기록한다. **프레임워크(React 등)는 이 프로필이 정하지 않는다** — 스택 프리셋과 P4 기술 설계가 정한다. |
| **Claude Design** | `off` | P3 착수 전 | `on`: PM(또는 사람 디자이너)이 claude.ai/design에서 시안·화면을 다듬을 수 있다. 결과는 오케스트레이터가 `{PROJECT}/design/imports/{YYYYMMDD}_{slug}/`에 저장(원본·작성자·일시 README 포함)하고, designer가 토큰·컴포넌트로 정규화해 반영한 뒤 리뷰·G3를 거친다. |
| **Design Sync** | `off` | G4 승인 이후 | `on`: 구현된 컴포넌트 라이브러리를 claude.ai/design 디자인 시스템 프로젝트로 게시한다(`/design-sync`, 코드 → Design 방향, **PM이 직접 실행**). designer는 게시 대상 목록만 정리한다. |

## 공통 원칙
- **기준(SoT)은 프로젝트 저장소의 승인된 산출물이다.** 외부 디자인 도구의 내용은 `design/`으로 반입·승인되기 전까지 기준이 아니며, 외부 도구에서 바뀐 내용을 코드에 직접 반영하지 않는다.
- 디자인 확정은 G3에서, 디자인–구현 일치 확인은 P4 디자인 QA와 P5 검증에서 한다 (구현 완료를 디자인 승인의 전제로 삼지 않는다).
- 승인 이후 디자인 변경은 외부 도구에서 시작되었더라도 CR 절차(`CLAUDE.md` §7)를 따른다.
- 외부 디자인 서비스에 고객 자료를 올리거나 게시하는 것은 `CLAUDE.md` §8의 **외부 전송**이므로 매번 PM 승인을 받는다.
