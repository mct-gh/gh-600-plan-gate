## Step 2: 계획을 시스템이 강제하게 만든다

템플릿은 칸을 만들어 줄 뿐입니다. 비워 두고 머지해도 아무도 막지 않습니다.
이제 그것을 검사하는 워크플로를 만듭니다.

<img width="180" alt="Inflatocat" src="../images/inflatocat.png" />

### 📖 이론: 기대를 보장으로 바꾼다

"PR 에 계획을 넣어 주세요" 는 **프로세스 기대**입니다.
필수 상태 체크(required status check)는 **시스템 보장**입니다.

둘의 차이는 사람이 바쁠 때 드러납니다. 기대는 무시할 수 있고 보장은 무시할 수 없습니다.

에이전트가 개입하면 이 차이가 더 커집니다. 에이전트는 사람보다 훨씬 빠르고 훨씬 많이
PR 을 만듭니다. 사람의 성실함에 기대는 통제는 그 속도에서 무너집니다.

> [!NOTE]
> 여기서 만드는 워크플로는 PR 본문을 검사합니다. 실무에서는 계획 파일 존재 여부,
> 특정 섹션 유무, 최소 길이 등 원하는 조건으로 확장합니다.

### ⌨️ 실습: plan-gate 워크플로를 만든다

1. **Add file → Create new file** 로 아래 경로에 파일을 만듭니다.

    ```
    .github/workflows/plan-gate.yml
    ```

1. 아래 내용을 붙여 넣습니다.

    ```yaml
    name: Plan Gate

    on:
      pull_request:
        branches: [ main ]
        types: [ opened, edited, synchronize, reopened ]

    permissions:
      contents: read

    jobs:
      require-plan:
        # 방어적 게이팅. PR 컨텍스트일 때만 돈다
        if: github.event_name == 'pull_request'
        runs-on: ubuntu-latest
        steps:
          - name: Require a plan section in the PR body
            env:
              BODY: ${{ github.event.pull_request.body }}
            run: |
              echo "$BODY" | grep -q "## Plan" || {
                echo "PR 본문에 '## Plan' 섹션이 필요합니다."
                exit 1
              }
              echo "$BODY" | grep -q "Success criteria" || {
                echo "PR 본문에 'Success criteria' 가 필요합니다."
                exit 1
              }
              echo "계획 섹션을 확인했습니다."
    ```

1. **Commit changes** 로 `main` 에 커밋합니다.

1. 커밋한 뒤 **Settings → Rules → Rulesets** 에서 이 체크를 필수로 등록해 보세요.

    이 단계는 채점하지 않지만 실무에서 반드시 필요한 마무리입니다.
    등록하지 않으면 체크는 빨간 X 만 띄울 뿐 머지를 막지 못합니다.

<details>
<summary>문제가 있나요? 🤷</summary><br/>

- **YAML 오류가 납니다**
  - 들여쓰기가 어긋났을 가능성이 큽니다. 위 내용을 그대로 다시 붙여 넣으세요.

- **채점이 통과하지 않습니다**
  - 파일이 `.github/workflows/plan-gate.yml` 인지 확인하세요.
  - `pull_request` 라는 단어가 파일에 있어야 합니다.

- **왜 방어적 게이팅이 필요한가요**
  - 워크플로는 의도치 않은 다른 이벤트로도 실행될 수 있습니다.
    PR 전용 동작을 조건 없이 두면 PR 이 아닌 상황에서 오작동합니다.

</details>

---
