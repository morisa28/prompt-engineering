# Legal Policy Review

## 1. Purpose
用于提取义务、风险和需咨询专业人士的问题，避免输出确定性法律建议。 本分支只处理该场景专属 prompt 构造；跨场景原则使用 `../../common-principles.md`。

## 2. Trigger Conditions
- 用户要求分析合同风险
- 用户要求政策/合规/隐私条款审查
- 用户要求法律文件摘要

## 3. Required Inputs
- {{jurisdiction}} 司法辖区
- {{document_type}} 文件类型
- {{document_text}} 文本
- {{risk_focus}} 风险关注
- {{not_legal_advice_notice}} 非法律建议声明

缺失信息处理：
- 司法辖区未知时必须标注为待补充
- 无法判断法律效力时仅做条款信息分析
- 涉及安全、金钱、医疗、法律、生产数据或大范围改动时，缺失的关键边界必须标注为阻塞问题。
- 非阻塞缺失项使用 `[待补充: field]`，并在最终 prompt 中要求目标模型标注假设。

## 4. Prompt Construction Rules
- 明确司法辖区
- 区分法律信息和法律建议
- 提取关键义务、权利、期限和违约后果
- 标注风险条款和需律师确认事项
- 需要适配 Codex、Codex CLI、Claude Code、Gemini CLI 或 ChatGPT 时，必须加入目标工具的工作方式、权限边界和最终报告格式。

## 5. Hard Constraints
- 禁止给确定性法律结论
- 禁止替代律师建议
- 禁止忽略适用法律和辖区
- 必须包含可检查的验收标准。
- 不确定时必须标注假设或提出阻塞问题。

## 6. Output Format
最终 prompt 应要求目标模型输出：
- 非法律建议声明
- 文件概览
- 关键义务
- 风险条款
- 谈判点
- 需专业确认问题

## 7. Quality Checklist
- [ ] 辖区已标注
- [ ] 义务和期限明确
- [ ] 风险等级有依据
- [ ] 没有替代法律意见
- [ ] prompt 包含目标、输入、步骤、约束、输出格式、验收标准和自检。
- [ ] 目标工具的执行环境和限制已写明。

## 8. Common Mistakes
| Common Mistake | Risk | Repair |
| --- | --- | --- |
| 直接判定违法 | 越界 | 写风险和需律师确认 |
| 不问辖区 | 结论失准 | 辖区作为必填 |
| 只摘要不找义务 | 用户无法行动 | 抽取义务/期限/后果 |

## 9. Reusable Template
```text
你是 {{target_ai_tool}}，请处理以下 Legal Policy Review 任务。

任务目标：
{{task_goal}}

输入材料：
- {{jurisdiction}}
- {{document_type}}
- {{risk_focus}}
- {{not_legal_advice_notice}}
- {{document_text}}
- {{input_materials}}

执行要求：
1. 明确司法辖区
2. 区分法律信息和法律建议
3. 提取关键义务、权利、期限和违约后果
4. 标注风险条款和需律师确认事项

硬性约束：
- 禁止给确定性法律结论
- 禁止替代律师建议
- 禁止忽略适用法律和辖区
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
分析一份合同里的风险。
```

高质量 prompt：

```text
请审阅 `[contract_text]`，文件类型：服务合同，司法辖区：`[jurisdiction]`。这不是法律意见，请只做法律信息和风险识别。提取付款、交付、保密、违约、终止、责任限制和争议解决条款；按 High/Medium/Low 标注风险、原因、可能后果和建议询问律师的问题。不要给确定性法律结论。
```
