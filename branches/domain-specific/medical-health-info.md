# Medical Health Info

## 1. Purpose
用于整理医学信息和就医问题，不替代医生诊断。 本分支只处理该场景专属 prompt 构造；跨场景原则使用 `../../common-principles.md`。

## 2. Trigger Conditions
- 用户要求解释症状或体检报告
- 用户要求准备看医生
- 用户要求医学资料总结

## 3. Required Inputs
- {{health_context}} 健康背景
- {{symptoms}} 症状
- {{timeline}} 时间线
- {{medical_records}} 检查资料
- {{urgent_warning}} 紧急情况提示

缺失信息处理：
- 严重症状信息缺失时先提醒急症边界
- 资料不足时只做整理和问题清单
- 涉及安全、金钱、医疗、法律、生产数据或大范围改动时，缺失的关键边界必须标注为阻塞问题。
- 非阻塞缺失项使用 `[待补充: field]`，并在最终 prompt 中要求目标模型标注假设。

## 4. Prompt Construction Rules
- 区分医学信息和诊断建议
- 提醒紧急情况
- 整理症状时间线
- 准备就医问题清单
- 标注不确定性
- 需要适配 Codex、Codex CLI、Claude Code、Gemini CLI 或 ChatGPT 时，必须加入目标工具的工作方式、权限边界和最终报告格式。

## 5. Hard Constraints
- 禁止替代医生判断
- 禁止给具体处方或停药建议
- 禁止弱化急症信号
- 必须包含可检查的验收标准。
- 不确定时必须标注假设或提出阻塞问题。

## 6. Output Format
最终 prompt 应要求目标模型输出：
- 安全提示
- 症状时间线
- 可能需要医生评估的问题
- 就医准备清单
- 需立即就医信号

## 7. Quality Checklist
- [ ] 非诊断边界明确
- [ ] 急症提醒覆盖
- [ ] 时间线完整
- [ ] 就医问题可直接使用
- [ ] prompt 包含目标、输入、步骤、约束、输出格式、验收标准和自检。
- [ ] 目标工具的执行环境和限制已写明。

## 8. Common Mistakes
| Common Mistake | Risk | Repair |
| --- | --- | --- |
| 直接诊断 | 医疗风险 | 只做信息整理和可能方向 |
| 忽略急症 | 延误就医 | 列红旗症状 |
| 建议用药 | 越界 | 建议咨询医生 |

## 9. Reusable Template
```text
你是 {{target_ai_tool}}，请处理以下 Medical Health Info 任务。

任务目标：
{{task_goal}}

输入材料：
- {{health_context}}
- {{symptoms}}
- {{timeline}}
- {{medical_records}}
- {{urgent_warning}}
- {{input_materials}}

执行要求：
1. 区分医学信息和诊断建议
2. 提醒紧急情况
3. 整理症状时间线
4. 准备就医问题清单
5. 标注不确定性

硬性约束：
- 禁止替代医生判断
- 禁止给具体处方或停药建议
- 禁止弱化急症信号
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
整理症状，帮我准备去看医生。
```

高质量 prompt：

```text
请把以下健康信息整理成就医准备材料：症状 `[symptoms]`，开始时间 `[timeline]`，既往病史/用药 `[health_context]`，检查资料 `[medical_records]`。请明确这不是诊断；先列需要立即就医的红旗信号；再输出症状时间线、可能需要医生评估的问题、就诊时要带的资料和要问医生的问题。不要给处方或替代医生判断。
```
