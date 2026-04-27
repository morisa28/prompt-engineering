# Meta Skill Builder

## 1. Purpose
用于设计领域专用 skill，包括触发条件、规则、模板、检查表、示例和维护说明。 本分支只处理该场景专属 prompt 构造；跨场景原则使用 `../../common-principles.md`。

## 2. Trigger Conditions
- 用户要求把流程做成 skill
- 用户要求把书籍/文档/经验提炼为 skill
- 用户要求创建新的 AI 工作流

## 3. Required Inputs
- {{source_materials}} 来源材料
- {{skill_name}} skill 名称
- {{target_users}} 目标使用者
- {{use_cases}} 使用场景
- {{maintenance_rules}} 维护规则

缺失信息处理：
- skill 名未知时提出短横线命名候选
- 材料不足时先输出骨架和待补充清单
- 涉及安全、金钱、医疗、法律、生产数据或大范围改动时，缺失的关键边界必须标注为阻塞问题。
- 非阻塞缺失项使用 `[待补充: field]`，并在最终 prompt 中要求目标模型标注假设。

## 4. Prompt Construction Rules
- 提炼领域知识
- 转化为规则
- 生成模板、checklist、示例
- 设计触发和不适用场景
- 生成维护说明
- 需要适配 Codex、Codex CLI、Claude Code、Gemini CLI 或 ChatGPT 时，必须加入目标工具的工作方式、权限边界和最终报告格式。

## 5. Hard Constraints
- 禁止只写教程
- 禁止缺少可调用触发条件
- 禁止缺少示例和验收标准
- 禁止复制来源长文
- 必须包含可检查的验收标准。
- 不确定时必须标注假设或提出阻塞问题。

## 6. Output Format
最终 prompt 应要求目标模型输出：
- skill 目标
- 目录结构
- SKILL.md
- 模板
- 检查表
- 示例
- 维护说明
- 模拟测试

## 7. Quality Checklist
- [ ] 触发条件明确
- [ ] 规则能指导行为
- [ ] 模板可复制
- [ ] 示例具体
- [ ] 维护边界明确
- [ ] prompt 包含目标、输入、步骤、约束、输出格式、验收标准和自检。
- [ ] 目标工具的执行环境和限制已写明。

## 8. Common Mistakes
| Common Mistake | Risk | Repair |
| --- | --- | --- |
| 写成知识笔记 | 不能触发使用 | 加 When to Use/Not Use |
| 无模板 | 不可复用 | 输出变量化模板 |
| 无模拟 | 路由不稳 | 用调用样例测试 |

## 9. Reusable Template
```text
你是 {{target_ai_tool}}，请处理以下 Meta Skill Builder 任务。

任务目标：
{{task_goal}}

输入材料：
- {{source_materials}}
- {{skill_name}}
- {{target_users}}
- {{use_cases}}
- {{maintenance_rules}}
- {{output_path}}
- {{input_materials}}

执行要求：
1. 提炼领域知识
2. 转化为规则
3. 生成模板、checklist、示例
4. 设计触发和不适用场景
5. 生成维护说明

硬性约束：
- 禁止只写教程
- 禁止缺少可调用触发条件
- 禁止缺少示例和验收标准
- 禁止复制来源长文
- 必须按 {{output_format}} 输出。
- 必须满足 {{acceptance_criteria}}。

验证与自检：
- 完成后检查目标、输入、约束、输出格式和验收标准是否全部满足。
- 对缺失或不确定的信息标注“假设”或“待补充”。
- 如果无法完成，说明阻塞原因和下一步需要的输入。
```

## 10. Example
用户原始需求：

```text
把这个工作流程做成一个 Codex skill。
```

高质量 prompt：

```text
请把 `[source_materials]` 提炼为 Codex skill：`[skill_name]`。先抽取适用场景、不适用场景、核心原则、执行流程、风险控制；再生成 `SKILL.md`、`templates.md`、`checklists.md`、`examples.md`。禁止复制来源长段原文。最后用 3 个用户请求模拟触发，检查是否能指导 Codex 行动。
```
