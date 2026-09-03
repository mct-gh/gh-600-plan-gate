## Step 3: 계획만 하는 에이전트를 만든다

계획을 요구하는 것과, 계획하는 주체를 실행하는 주체와 **분리**하는 것은 다른 일입니다.
이번에는 파일을 고칠 수 없는 에이전트를 만듭니다.

<img width="180" alt="Jetpacktocat" src="../images/jetpacktocat.png" />

### 📖 이론: 능력 경계가 진짜 통제다

"수정하지 마세요" 라고 지시문에 쓰는 것은 **안내**입니다.
도구 목록에서 편집 도구를 빼는 것은 **강제**입니다.

GH-600 은 이 둘을 명확히 구분합니다.

| 방법 | 성격 | 신뢰도 |
| --- | --- | --- |
| 지시문에 "고치지 마" 라고 쓴다 | 안내 | 모델이 따를 수도, 안 따를 수도 |
| `tools` 에서 편집 도구를 뺀다 | 강제 | 애초에 불가능 |

커스텀 에이전트는 `.github/agents/` 아래 `.agent.md` 파일로 정의합니다.
YAML frontmatter 로 능력을 규정하고, 본문으로 행동을 설명합니다.

frontmatter 에서 이번에 쓸 것들입니다.

- `description` — 필수입니다. 이 에이전트가 무엇을 하는지
- `tools` — 쓸 수 있는 도구 목록. 여기에 없는 것은 못 씁니다
- `mcp-servers` — 외부 도구를 MCP 로 붙입니다
- `disable-model-invocation` — 다른 에이전트가 이 에이전트를 자동 호출하지 못하게 합니다

> [!WARNING]
> `disable-model-invocation: true` 로 두면 오케스트레이터가 이 에이전트를 서브에이전트로
> 부를 수 없습니다. 멀티 에이전트가 실패하는 흔한 원인입니다. 실습 5에서 다시 만납니다.

### ⌨️ 실습: 읽기 전용 계획 에이전트를 만든다

1. 아래 경로에 파일을 만듭니다.

    ```
    .github/agents/planner.agent.md
    ```

1. 아래 내용을 붙여 넣습니다.

    ```markdown
    ---
    name: planner
    description: 구현 계획만 작성하는 읽기 전용 에이전트. 파일을 수정하지 않는다.
    tools: ["read", "search"]
    mcp-servers:
      github:
        type: http
        url: https://api.githubcopilot.com/mcp/
    disable-model-invocation: false
    ---

    당신은 구현 계획 전문가입니다. 코드를 작성하지 않고 계획만 만듭니다.

    계획 문서에는 항상 다음 다섯 개를 제목으로 포함합니다.

    - 입력 — 이 작업에 필요한 것
    - 출력 — 이 작업이 만들어 낼 것
    - 성공 기준 — 검증 가능한 형태로
    - 범위 밖 — 건드리지 않을 것
    - 롤백 — 문제가 생겼을 때 되돌리는 방법

    성공 기준은 "테스트 통과" 같은 대리 지표가 아니라 실제 의도를 적습니다.
    ```

1. **Commit changes** 로 `main` 에 커밋합니다.

1. 리포지토리 **Settings → Copilot → Cloud agent** 에서 MCP 설정 화면도 열어 보세요.

    cloud agent 의 MCP 설정은 파일이 아니라 이 화면의 JSON 입력란에 넣습니다.
    채점하지는 않지만 시험에 나오는 구분이니 위치를 눈에 익혀 두세요.

<details>
<summary>문제가 있나요? 🤷</summary><br/>

- **채점이 통과하지 않습니다**
  - 파일이 `.github/agents/planner.agent.md` 인지 확인하세요. 확장자가 `.agent.md` 입니다.
  - frontmatter 에 `description:`, `tools:`, `mcp-servers:` 세 줄이 모두 있어야 합니다.

- **frontmatter 가 뭔가요**
  - 파일 맨 위 `---` 세 줄 사이에 들어가는 YAML 설정 블록입니다.
    본문은 그 아래에 씁니다.

- **MCP 설정 화면이 안 보입니다**
  - 조직 정책으로 막혀 있을 수 있습니다. 이 단계는 채점하지 않으니 넘어가도 됩니다.

</details>

---
