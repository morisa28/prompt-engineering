# Finance Investment Analysis

## 1. Purpose
用于生成信息分析和风险框架，不承诺收益，不替代持牌建议。 本分支只处理该场景专属 prompt 构造；跨场景原则使用 `../../common-principles.md`。

## 2. Trigger Conditions
- 用户要求金融/投资/财务分析
- 用户要求股票基金加密资产研究
- 用户要求预算或财务规划

## 3. Required Inputs
- {{asset_or_topic}} 标的或主题
- {{risk_profile}} 风险偏好
- {{time_horizon}} 时间范围
- {{data_sources}} 数据来源
- {{not_financial_advice_notice}} 非投资建议声明

缺失信息处理：
- 最新数据可能变化时必须要求查证当前价格和日期
- 风险偏好未知时输出多情景而非单一建议
- 涉及安全、金钱、医疗、法律、生产数据或大范围改动时，缺失的关键边界必须标注为阻塞问题。
- 非阻塞缺失项使用 `[待补充: field]`，并在最终 prompt 中要求目标模型标注假设。

## 4. Prompt Construction Rules
- 区分信息分析和投资建议
- 使用最新数据并标注来源和日期
- 输出风险、假设和限制
- 避免承诺收益
- 需要适配 Codex、Codex CLI、Claude Code、Gemini CLI 或 ChatGPT 时，必须加入目标工具的工作方式、权限边界和最终报告格式。

## 5. Hard Constraints
- 禁止保证收益或给确定买卖指令
- 禁止使用过期数据不标日期
- 禁止忽略波动性和下行风险
- 必须包含可检查的验收标准。
- 不确定时必须标注假设或提出阻塞问题。

## 6. Output Format
最终 prompt 应要求目标模型输出：
- 非投资建议声明
- 数据来源
- 基本面/指标
- 情景分析
- 风险
- 待验证数据

## 7. Quality Checklist
- [ ] 数据日期明确
- [ ] 风险偏好明确或已假设
- [ ] 结论非确定买卖建议
- [ ] 限制已标注
- [ ] prompt 包含目标、输入、步骤、约束、输出格式、验收标准和自检。
- [ ] 目标工具的执行环境和限制已写明。

## 8. Common Mistakes
| Common Mistake | Risk | Repair |
| --- | --- | --- |
| 给买卖结论 | 合规风险 | 输出分析框架和风险 |
| 数据无日期 | 过时误导 | 标来源和日期 |
| 忽略下行 | 风险失衡 | 多情景分析 |

## 9. Reusable Template
```text
你是 {{target_ai_tool}}，请处理以下 Finance Investment Analysis 任务。

任务目标：
{{task_goal}}

输入材料：
- {{asset_or_topic}}
- {{risk_profile}}
- {{time_horizon}}
- {{data_sources}}
- {{not_financial_advice_notice}}
- {{input_materials}}

执行要求：
1. 区分信息分析和投资建议
2. 使用最新数据并标注来源和日期
3. 输出风险、假设和限制
4. 避免承诺收益

硬性约束：
- 禁止保证收益或给确定买卖指令
- 禁止使用过期数据不标日期
- 禁止忽略波动性和下行风险
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
写一个股票分析 prompt。
```

高质量 prompt：

```text
请对 `[ticker]` 做信息性投资研究，这不是投资建议。请使用最新可用数据并标注来源和日期；分析业务、财务指标、估值、行业、催化因素、下行风险和多情景假设。不要给确定买入/卖出指令或承诺收益。输出需要进一步核验的数据和适合不同风险偏好的关注点。
```
