## Step 1: 계획을 요구하는 자리를 만든다

실습 1에서 우리는 에이전트의 계획을 읽고 리뷰했습니다. 그런데 그때 계획이 있었던 것은
운이 좋았기 때문입니다. 아무도 계획을 **요구**하지 않았습니다.

이번에는 계획 없는 PR 이 아예 통과하지 못하게 만듭니다.

<img width="180" alt="Inspectocat" src="../images/inspectocat.png" />

### 📖 이론: Pull Request 는 아키텍처 통제 지점이다

PR 을 협업 도구로만 보면 절반만 쓰는 것입니다. 에이전트 시스템에서 PR 은
**실행을 막는 관문**입니다.

```
에이전트가 브랜치 생성
        ↓
에이전트가 PR 을 연다 (계획 포함)
        ↓
필수 리뷰가 접근 방식을 검증
        ↓
GitHub Actions 가 필수 체크 실행
        ↓
체크 통과 + 승인 완료
        ↓
비로소 머지 가능
```

여기서 자주 나오는 안티패턴이 **planless execution** 입니다.
diff 는 있는데 왜 그렇게 했는지가 없는 상태입니다.
대응은 두 가지를 겹쳐서 합니다.

1. PR 템플릿으로 계획 칸을 만든다 — 무엇을 적어야 하는지 알려준다
2. 필수 체크로 계획을 강제한다 — 안 적으면 머지가 안 된다

1번만 하면 권고에 그칩니다. 이번 단계에서 1번을 하고, 다음 단계에서 2번을 합니다.

> [!IMPORTANT]
> GH-600 이 반복하는 원칙입니다. **플랫폼이 강제할 수 없으면 선택사항으로 취급하라.**
> 문서에만 있는 통제는 지켜지지 않습니다.

### ⌨️ 실습: PR 템플릿을 만든다

1. 리포지토리에서 **Add file → Create new file** 을 누릅니다.

1. 파일 이름에 정확히 이렇게 입력합니다.

    ```
    .github/pull_request_template.md
    ```

    > [!WARNING]
    > 앞의 점과 소문자를 꼭 지키세요. `Github/` 나 `.Github/` 로 쓰면 동작하지 않습니다.

1. 아래 내용을 붙여 넣습니다.

    ```markdown
    ## Plan
    - **Goal:**
    - **Scope (paths/files):**
    - **Steps:**
      1.
      2.
    - **Success criteria:**
      - [ ] 필수 체크 통과
      - [ ] 범위 밖 파일이 바뀌지 않음
    - **Risks and mitigations:**
    - **Rollback / escalation:**

    ## Evidence
    - Workflow run:
    - Scan results:

    ## Review checklist
    - [ ] 계획을 검토하고 승인했다
    - [ ] 필수 리뷰를 충족했다
    - [ ] 필수 체크를 충족했다
    ```

1. **Commit changes** 를 눌러 `main` 에 직접 커밋합니다.

<details>
<summary>문제가 있나요? 🤷</summary><br/>

- **채점이 통과하지 않습니다**
  - 파일 경로가 `.github/pull_request_template.md` 인지 확인하세요.
  - `Success criteria` 와 `Rollback` 이라는 단어가 파일 안에 있어야 합니다. 영어 그대로 두세요.

- **어디에 커밋해야 하나요**
  - 이번 단계는 `main` 에 직접 커밋합니다. 아직 보호 규칙을 걸지 않았습니다.

</details>

---
