# Product Requirements

## 1. Purpose
用于把产品想法转化为用户、场景、功能、验收和迭代计划。 本分支只处理该场景专属 prompt 构造；跨场景原则使用 `../../common-principles.md`。

## 2. Trigger Conditions
- 用户要求写 PRD/MVP
- 用户要求产品需求或功能规格
- 用户要求用户故事

## 3. Required Inputs
- {{product_goal}} 产品目标
- {{user_personas}} 用户画像
- {{scenarios}} 使用场景
- {{constraints}} 约束
- {{success_metrics}} 成功指标

缺失信息处理：
- 画像未知时用目标用户假设并标注
- 商业目标未知时先以用户问题和 MVP 验证为主
- 涉及安全、金钱、医疗、法律、生产数据或大范围改动时，缺失的关键边界必须标注为阻塞问题。
- 非阻塞缺失项使用 `[待补充: field]`，并在最终 prompt 中要求目标模型标注假设。

## 4. Prompt Construction Rules
- 明确用户画像和场景
- 拆分核心功能
- 编写用户故事
- 设计验收标准
- 区分 MVP 和后续迭代
- 需要适配 Codex、Codex CLI、Claude Code、Gemini CLI 或 ChatGPT 时，必须加入目标工具的工作方式、权限边界和最终报告格式。

## 5. Hard Constraints
- 禁止把愿景写成不可验收需求
- 禁止混合 MVP 和长期功能
- 禁止缺少非目标范围
- 必须包含可检查的验收标准。
- 不确定时必须标注假设或提出阻塞问题。

## 6. Output Format
最终 prompt 应要求目标模型输出：
- 背景
- 用户画像
- 问题与目标
- MVP 范围
- 用户故事
- 验收标准
- 指标
- 后续迭代

## 7. Quality Checklist
- [ ] 每个功能有用户价值
- [ ] MVP 范围明确
- [ ] 验收标准可测试
- [ ] 非目标已列出
- [ ] prompt 包含目标、输入、步骤、约束、输出格式、验收标准和自检。
- [ ] 目标工具的执行环境和限制已写明。

## 8. Common Mistakes
| Common Mistake | Risk | Repair |
| --- | --- | --- |
| 功能清单无用户 | 需求失焦 | 先画像和场景 |
| 没有验收 | 开发难执行 | 写 Given/When/Then |
| MVP 过大 | 无法验证 | 分首版和后续 |

## 9. Reusable Template
```text
你是 {{target_ai_tool}}，请处理以下 Product Requirements 任务。

任务目标：
{{task_goal}}

输入材料：
- {{product_goal}}
- {{user_personas}}
- {{scenarios}}
- {{success_metrics}}
- {{acceptance_criteria}}
- {{input_materials}}

执行要求：
1. 明确用户画像和场景
2. 拆分核心功能
3. 编写用户故事
4. 设计验收标准
5. 区分 MVP 和后续迭代

硬性约束：
- 禁止把愿景写成不可验收需求
- 禁止混合 MVP 和长期功能
- 禁止缺少非目标范围
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
帮我写一个 PRD。
```

高质量 prompt：

```text
请为 `[product_idea]` 写 PRD。目标用户是 `[user_personas]`，核心场景是 `[scenarios]`。请先明确用户问题和成功指标，再拆分 MVP 功能、非目标、用户故事和验收标准。每个用户故事使用 Given/When/Then。区分首版必须做和后续迭代，不要写无法验收的愿景描述。
```
