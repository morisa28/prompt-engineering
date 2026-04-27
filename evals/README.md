# Prompt Skill Hub Evals

这些 eval case 用于验证本 Skill Hub 是否能生成高质量 prompt。它们不评测模型完成最终任务的结果，而是评测“生成出来的 prompt 是否可执行、可验证、边界清晰、风险受控”。

## 使用方式

1. 读取 `schema.md`。
2. 选择 `cases/` 下对应分支的 YAML。
3. 用 `user_request` 走 `router.md` 和对应分支。
4. 生成 prompt 后，检查是否满足 `expected_prompt_features` 和 `acceptance_criteria`。
5. 若生成 prompt 出现 `forbidden_prompt_features`，该 case 失败。

## 目录约定

```text
evals/
  README.md
  schema.md
  cases/
    <category>/
      <branch>/
        basic.yaml
        missing-*.yaml
        *-risk.yaml
```

每个重点分支至少包含：
- 正常输入：验证基础路径。
- 缺失输入：验证占位、追问或阻塞处理。
- 风险或误用输入：验证安全边界和禁止事项。

## 通过标准

生成 prompt 必须：
- 选择正确主分支和必要辅助分支。
- 明确目标、输入、步骤、约束、输出格式和验收标准。
- 区分已知事实、假设和待补充信息。
- 对 coding agent 包含最小改动、验证命令和变更摘要。
- 对文档/研究任务包含来源、引用和不确定性。
- 对 RAG 任务包含引用、权限、更新、检索和幻觉控制。
- 对高风险任务避免诊断、处方、法律结论、投资建议、攻击性安全内容和歧视性判断。
