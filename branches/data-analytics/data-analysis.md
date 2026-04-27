# Data Analysis

## 1. Purpose
用于设计可复现的数据分析流程并输出结论、限制和下一步建议。 本分支只处理该场景专属 prompt 构造；跨场景原则使用 `../../common-principles.md`。

## 2. Trigger Conditions
- 用户要求分析数据集
- 用户要求趋势/异常/统计分析
- 用户要求业务指标分析

## 3. Required Inputs
- {{data_source}} 数据来源
- {{analysis_goal}} 分析目标
- {{variables}} 变量说明
- {{method_constraints}} 方法限制
- {{deliverable}} 交付物

缺失信息处理：
- 字段含义未知时先做数据字典和待确认项
- 数据质量未知时先做缺失、重复、异常检查
- 涉及安全、金钱、医疗、法律、生产数据或大范围改动时，缺失的关键边界必须标注为阻塞问题。
- 非阻塞缺失项使用 `[待补充: field]`，并在最终 prompt 中要求目标模型标注假设。

## 4. Prompt Construction Rules
- 明确数据来源和分析目标
- 识别变量和口径
- 设计可复现步骤
- 输出结论、限制和下一步建议
- 需要适配 Codex、Codex CLI、Claude Code、Gemini CLI 或 ChatGPT 时，必须加入目标工具的工作方式、权限边界和最终报告格式。

## 5. Hard Constraints
- 禁止跳过数据质量检查
- 禁止把相关性写成因果
- 禁止隐藏过滤和清洗规则
- 必须包含可检查的验收标准。
- 不确定时必须标注假设或提出阻塞问题。

## 6. Output Format
最终 prompt 应要求目标模型输出：
- 数据概览
- 数据质量
- 方法
- 结果
- 图表建议
- 限制
- 下一步

## 7. Quality Checklist
- [ ] 口径明确
- [ ] 清洗规则可复现
- [ ] 结论有数据支持
- [ ] 限制已说明
- [ ] prompt 包含目标、输入、步骤、约束、输出格式、验收标准和自检。
- [ ] 目标工具的执行环境和限制已写明。

## 8. Common Mistakes
| Common Mistake | Risk | Repair |
| --- | --- | --- |
| 先下结论 | 分析偏见 | 先数据质量和方法 |
| 混淆因果 | 误导决策 | 标注相关性/因果边界 |
| 不可复现 | 无法复查 | 要求代码和参数 |

## 9. Reusable Template
```text
你是 {{target_ai_tool}}，请处理以下 Data Analysis 任务。

任务目标：
{{task_goal}}

输入材料：
- {{data_source}}
- {{analysis_goal}}
- {{variables}}
- {{method_constraints}}
- {{verification_steps}}
- {{input_materials}}

执行要求：
1. 明确数据来源和分析目标
2. 识别变量和口径
3. 设计可复现步骤
4. 输出结论、限制和下一步建议

硬性约束：
- 禁止跳过数据质量检查
- 禁止把相关性写成因果
- 禁止隐藏过滤和清洗规则
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
帮我写 prompt，让 AI 分析一份用户行为数据。
```

高质量 prompt：

```text
请分析 `[data_source]` 的用户行为数据，目标是找出留存和转化的关键趋势。先读取字段并建立数据字典；检查缺失、重复、异常和时间范围；定义转化率、留存率等指标口径；用可复现代码完成分析。输出数据质量报告、关键图表、结论、限制和下一步建议。禁止把相关性写成因果。
```
