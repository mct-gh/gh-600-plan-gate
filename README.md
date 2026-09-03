# 실습 2 — 계획 게이트와 도구 경계

_계획 없는 PR 이 머지되지 못하게 만들고, 파일을 못 고치는 계획 전용 에이전트를 만듭니다._

## 이 실습에 대하여

- **대상**: 실습 1을 마쳤고, 에이전트를 통제하는 방법을 배우려는 분
- **배우는 것**: PR 을 통제 지점으로 쓰는 법, 필수 체크로 계획을 강제하는 법,
  도구 허용목록으로 능력을 제한하는 법, MCP 서버를 에이전트에 붙이는 법
- **만드는 것**: PR 템플릿, plan-gate 워크플로, 읽기 전용 계획 에이전트, 계획 문서
- **필요한 것**:
  - 실습 1 완료 (권장)
  - **Copilot 유료 플랜**. Free 로는 진행할 수 없습니다
- **소요 시간**: 20분

진행 순서

1. PR 템플릿으로 계획을 적을 칸을 만든다
2. plan-gate 워크플로로 계획을 시스템이 강제하게 한다
3. 읽기 전용 계획 에이전트를 만들고 MCP 서버를 붙인다
4. 작업 계약 형태로 계획 문서를 산출한다

## 시작하는 법

아래 버튼으로 이 실습을 여러분 계정에 복사한 뒤, **20초 정도** 기다렸다가
페이지를 **새로고침** 하세요.

[![](https://img.shields.io/badge/Copy%20Exercise-%E2%86%92-1f883d?style=for-the-badge&logo=github&labelColor=197935)](https://github.com/new?template_owner=mct-gh&template_name=gh600-plan-gate&owner=%40me&name=gh600-plan-gate&description=GH-600+Lab+B+-+Plan+gates+and+tool+boundaries&visibility=public)

<details>
<summary>문제가 있나요? 🤷</summary><br/>

- 소유자는 개인 계정 또는 여러분이 관리하는 조직을 고르세요.
- **공개(public)** 로 만드는 것을 권장합니다.

20초 뒤에도 준비되지 않으면 [Actions](../../actions) 탭을 확인하세요.

</details>

> [!IMPORTANT]
> **Fork 하지 마세요.** 위 Copy Exercise 버튼을 눌러야 채점이 동작합니다.
> 그리고 `.github/workflows` 안의 **실습 진행용 워크플로**(`0-start-exercise.yml`,
> `1-step.yml` 등)는 수정하지 마세요. 실습에서 새로 만드는 `plan-gate.yml` 은 괜찮습니다.

## 시험 대응

- **영역 1** — 에이전트 아키텍처 및 SDLC 프로세스 준비 (15~20%)
- **영역 2** — 도구 사용 및 환경 상호 작용 구현 (20~25%)

## 관련 학습 자료

- [Designing Agent Architecture and SDLC Integration](https://learn.microsoft.com/ko-kr/training/modules/design-agent-architecture-integration/)
- [커스텀 에이전트 구성 레퍼런스](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- [실습 모음으로 돌아가기](https://github.com/mct-gh/gh-600-labs)
