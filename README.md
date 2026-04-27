# Prompt Engineering Skill Hub

一句话定位：把用户的模糊需求路由到合适场景分支，并生成可交给 Codex、Codex CLI、Claude Code、Gemini CLI、ChatGPT 等工具执行的高质量 prompt。

## 适用人群

- 想把口语需求改成强执行 prompt 的用户。
- 为 coding agent、研究 agent、数据分析 agent 或文档 agent 编写任务说明的人。
- 维护 Prompt Skill Hub、模板库、路由系统和 eval cases 的团队。
- 需要在医疗、法律、金融、安全、招聘、隐私等高风险领域保持边界的 AI workflow 设计者。

## 典型使用场景

- 修复代码 bug、补测试、生成 Codex CLI 任务 prompt。
- 分析仓库结构、输出项目理解报告。
- 编写 PRD，并转成开发 Agent prompt。
- 设计 RAG 知识库，包含引用、权限、更新和评估。
- 做数据分析 prompt，包含字段、清洗、指标、图表和结论边界。
- 将合同、体检报告、金融资料等高风险材料整理为安全的信息性 prompt。
- 审计和改进 Prompt 系统本身。

## 最短使用路径

1. 读 `router.md`，按最终交付物选择唯一主分支。
2. 必要时用 `branch-composition.md` 选择 1 到 3 个辅助分支。
3. 用 `templates.md` 和对应 `branches/` 文件生成 prompt。
4. 用 `checklists.md` 自检。

## 完整使用路径

1. `prompt-generation-protocol.md`：按 Intake、Normalize、Route、Missing Inputs、Risk Check、Construct、Self Check、Finalize 执行。
2. `router.md`：输出主分支、辅助分支、命中原因、缺失输入、风险等级和构造策略。
3. `common-principles.md`：统一缺失输入、约束优先级、防幻觉、高风险边界和工具适配。
4. `templates.md`：选择通用模板、分支模板或多分支组合模板。
5. `checklists.md`：用通用、路由、分支、高风险和最终交付前检查表验收。
6. `examples.md`：参考从模糊需求到最终 prompt 的全过程示例。
7. `evals/`：用 eval case 检查生成 prompt 的质量。
8. `branches/manifest.yaml`：读取机器可用的重点分支索引和 eval 覆盖。

## 路由示例

用户输入：

```text
帮我写一个 prompt，让 Codex 修复这个项目 npm run build 报错，并确保不要乱改。
```

路由结果：

```text
主分支：software-engineering/bugfix-debugging
辅助分支：software-engineering/test-generation, software-engineering/cli-agent
风险等级：medium
缺失输入：错误日志、复现步骤、工作目录、环境信息、验证命令
```

最终 prompt：

```text
你是 Codex，请在 `[待补充: working_directory]` 修复 `npm run build` 报错。错误日志：`[待补充: error_log]`。先读取 `package.json`、构建配置、错误栈指向文件和相关测试；先复现失败或说明无法复现原因，再定位根因。只做最小修复，禁止无关重构、全仓库格式化、新增依赖、删除测试、降低断言或注释掉失败逻辑。修复后运行 `npm run build` 和 `[待补充: test_command]`。最终输出根因、证据、修改文件、关键改动、验证命令与结果、未验证项和剩余风险。
```

自检结果：

- [x] 主分支由最终交付物决定。
- [x] 缺失输入已标注。
- [x] 包含最小修复、禁止事项和验证命令。
- [x] 输出格式可直接交给 Codex 执行。

## 如何选择主分支

主分支只看用户最终想得到的交付物：

- 修复错误：`branches/software-engineering/bugfix-debugging.md`
- 理解仓库：`branches/software-engineering/repository-analysis.md`
- 写新功能：`branches/software-engineering/coding-feature-development.md`
- 补测试：`branches/software-engineering/test-generation.md`
- 写 PRD：`branches/product-design-business/product-requirements.md`
- 做 RAG：`branches/ai-systems/knowledge-base-rag.md`
- 做数据分析：`branches/data-analytics/data-analysis.md`
- 写报告：`branches/documents-research/report-writing.md`
- 医疗/法律/金融：对应 `branches/domain-specific/`
- 改进 Prompt 系统：`branches/meta/meta-skill-builder.md`

## 如何组合辅助分支

辅助分支只在能提升执行质量时使用：

- CLI 工具：`software-engineering/cli-agent`
- 验证或测试：`software-engineering/test-generation`
- 正式报告：`documents-research/report-writing`
- 来源引用：`documents-research/documentation-analysis` 或 `ai-systems/knowledge-base-rag`
- 安全边界：`software-engineering/security-threat-modeling` 或对应高风险分支

普通任务 0 到 1 个辅助分支，中等复杂任务 1 到 2 个，高风险或跨域任务最多 3 个。超过 3 个时拆成多阶段 prompt。

## 如何使用 Eval Cases

`evals/cases/` 里的 YAML 用来验证“生成的 prompt 是否高质量”，不是验证模型最终任务结果。

每个 case 包含：
- `expected_primary_branch`
- `expected_auxiliary_branches`
- `required_missing_inputs`
- `expected_prompt_features`
- `forbidden_prompt_features`
- `acceptance_criteria`

使用方式：

```bash
python3 scripts/skill_hub_manager.py validate
python3 scripts/skill_hub_manager.py stats
```

## 如何扩展新分支

新增分支时应同步更新：
- `SKILL.md`
- `router.md`
- `templates.md`
- `checklists.md`
- `examples.md`
- `branches/manifest.yaml`
- `evals/cases/<category>/<branch>/`

每个分支必须包含 10 节：Purpose、Trigger Conditions、Required Inputs、Prompt Construction Rules、Hard Constraints、Output Format、Quality Checklist、Common Mistakes、Reusable Template、Example。

## 高风险领域说明

- 医疗：不诊断、不处方、不停药换药，只做症状整理、红旗信号和就医准备。
- 法律：不替代律师，不给最终法律结论，必须要求司法辖区和文本。
- 金融：不构成个性化投资建议，不保证收益，不给买卖指令。
- 安全：只支持防御性安全，不提供攻击链、payload、绕过或持久化。
- 招聘/人事：不使用受保护属性，必须基于岗位相关证据。
- 隐私/个人数据：最小化、脱敏、限定用途和保留边界。
