# Prompt Engineering Skill Hub

这是 `prompt-engineering` skill 的可分发副本，用于把模糊需求转化为可直接交给 Codex、Codex CLI、Claude Code、Gemini CLI、ChatGPT 等工具执行的高质量 prompt。

## 内容概览

- `SKILL.md`：总入口，说明用途、触发条件、分支分类和质量标准。
- `router.md`：路由系统，根据用户需求选择主分支和辅助分支。
- `common-principles.md`：跨分支通用 prompt 编写原则。
- `templates.md`：跨场景可复制模板。
- `checklists.md`：可执行质量检查表。
- `examples.md`：跨分支完整示例。
- `branches/`：场景化分支库，共 45 个分支。
- `scripts/`：扩展管理脚本，用于新增分支、验证结构、统计规模和输出能力说明。

## 分支分类

- `general-prompt`：prompt 评审、改写、扩写、压缩、模板化。
- `software-engineering`：plan mode、功能开发、debug、重构、测试、代码审查、仓库分析、CLI agent、安全建模、DevOps/CI、数据库迁移、API 设计。
- `documents-research`：文档分析、PDF to skill、研究综合、学术写作、报告写作。
- `data-analytics`：数据分析、表格分析、可视化仪表盘。
- `product-design-business`：PRD、UX/UI、商业策略、营销内容。
- `ai-systems`：知识库 / RAG。
- `business-operations`：客服质检、招聘评估。
- `education`：课程设计。
- `creative-design`：游戏设计。
- `multimodal`：图片、音视频、3D/交互视觉任务。
- `domain-specific`：法律、医疗、金融、教育辅导。
- `communication`：翻译本地化、角色模拟。
- `automation`：自动化工作流。
- `meta`：meta skill builder。

## 使用方式

1. 先读取 `SKILL.md`。
2. 读取 `router.md`，判断主分支和辅助分支。
3. 应用 `common-principles.md` 的通用约束。
4. 使用 `templates.md` 和对应分支模板生成 prompt。
5. 用 `checklists.md` 和分支检查表自检。
6. 必要时参考 `examples.md`。

## 扩展管理脚本

副本内置 `scripts/skill_hub_manager.py`，用于维护 Skill Hub：

```bash
python3 scripts/skill_hub_manager.py validate
python3 scripts/skill_hub_manager.py stats
python3 scripts/skill_hub_manager.py capabilities
python3 scripts/skill_hub_manager.py init-spec --output <branch-spec.json>
python3 scripts/skill_hub_manager.py add-branch --spec <branch-spec.json> --dry-run
python3 scripts/skill_hub_manager.py add-branch --spec <branch-spec.json>
```

脚本能力：

- `validate`：检查根文件、分支 10 节结构、路由引用、模板占位符、检查表和空泛表达。
- `stats`：统计分支、分类、模板、检查表和示例数量。
- `capabilities`：输出当前 Skill Hub 能力说明。
- `init-spec`：生成新分支 JSON spec 示例。
- `add-branch`：根据 JSON spec 新增分支，并同步更新 `SKILL.md`、`router.md`、`templates.md`、`checklists.md` 和 `examples.md`。

## 维护规则

- 普通 prompt 生成、评审、改写、扩写、压缩、路由或模板查找时，不调用脚本。
- 只有当用户明确要求“添加 / 创建 / 注册新分支”时，才允许询问并调用 `add-branch`。
- 调用 `add-branch` 前，必须确认分支分类、slug、用途、触发条件、必需输入、构造规则、硬约束、输出格式、检查表和示例。
- 新增分支前优先运行 `--dry-run`；确认无误后再正式写入。
- 新增分支后必须运行 `validate`、`stats` 和 `capabilities`，并汇报修改文件、统计结果和剩余问题。

## 质量标准

最终 prompt 必须具备：明确目标、充分上下文、清晰输入、具体步骤、强约束、输出格式、验收标准、风险控制、不确定性标注，并能直接交给目标 AI 工具执行。

## 当前规模

- 分支：45 个。
- 分类：14 个。
- 通用模板：23 个。
- 通用检查表：28 个。
- 跨分支示例：28 个。
- 路由示例：23 个。

## 分发位置

- 将整个 `prompt-engineering` 目录复制到目标 Codex skills 目录或团队约定的分发目录。
