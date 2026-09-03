## 정리

계획을 요구하는 자리를 만들고, 시스템이 강제하게 하고, 계획하는 주체를 실행하는 주체와
분리했습니다.

### 무엇을 만들었나

| 파일 | 역할 |
| --- | --- |
| `.github/pull_request_template.md` | 계획을 적을 칸을 만든다 |
| `.github/workflows/plan-gate.yml` | 계획이 없으면 체크를 실패시킨다 |
| `.github/agents/planner.agent.md` | 파일을 못 고치는 읽기 전용 계획 에이전트 |
| `docs/plan.md` | 작업 계약 형태로 정리된 계획 산출물 |

### 꼭 기억할 것

- 플랫폼이 강제할 수 없으면 선택사항이다. 문서에만 있는 통제는 지켜지지 않는다
- 지시문은 안내이고, 도구 허용목록은 강제다
- 작업 계약 세 조각 — 입력, 출력, 성공 기준
- "CI 통과" 는 필요조건이지 충분조건이 아니다
- 커스텀 에이전트는 `.github/agents/` 아래 `.agent.md` 파일이다
- `description` 은 필수 항목이다
- cloud agent 의 MCP 설정은 파일이 아니라 리포 Settings 의 JSON 입력란이다

### 계획 우선인가, 계획과 실행 동시인가

계획을 언제 보여줄지에 두 가지 방식이 있습니다. 시험에 나옵니다.

| | Option A 계획 우선 | Option B 계획과 실행 동시 |
| --- | --- | --- |
| 동작 | 계획만 담은 PR 을 먼저 열고 승인 후 구현 | 계획과 코드를 한 PR 에 함께 |
| 검증 시점 | 코드 작성 **전** | 머지 **전** |
| 위험 노출 | 늦다 | 이르다 |
| 적합한 곳 | 되돌리기 어려운 고위험 작업 | 속도가 중요한 저위험 작업 |

핵심은 "검토를 하느냐"가 아닙니다. 검토는 어느 쪽이든 합니다.
**코드 생성을 사람의 검증 앞에 둘 것인가 뒤에 둘 것인가**가 선택지입니다.

### 시험에서는

**영역 1 — 에이전트 아키텍처 및 SDLC 프로세스 준비 (15~20%)**
- 계획과 실행의 경계를 정의한다
- 에이전트가 구조화된 계획을 출력하도록 구성한다
- 확인과 승인 전에는 실행을 막는다

**영역 2 — 도구 사용 및 환경 상호 작용 구현 (20~25%)**
- 필요한 도구를 식별하고 구성한다
- 에이전트 도구 권한을 구성한다
- MCP 서버를 도구로 추가한다

### 더 볼 것

- [Designing Agent Architecture and SDLC Integration](https://learn.microsoft.com/ko-kr/training/modules/design-agent-architecture-integration/)
- [커스텀 에이전트 구성 레퍼런스](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- [리포지토리 MCP 서버 구성](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/configure-mcp-servers)

### 다음 실습

**[실습 3 — 에이전틱 워크플로 (공식 실습)](https://github.com/skills/agentic-workflows-that-read-the-room)**
