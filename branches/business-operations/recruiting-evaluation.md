# Recruiting Evaluation

## 1. Purpose
用于为简历筛选、面试评估、候选人打分、岗位匹配、面试题设计、面试记录总结、招聘决策辅助和人才画像对比生成 prompt。本分支强调证据、岗位匹配、偏见控制和合规边界。

## 2. Trigger Conditions
- 用户提到简历筛选、候选人评估、岗位匹配、招聘打分、人才画像。
- 用户要求根据 JD、简历或面试记录设计面试问题。
- 用户要求总结面试记录或辅助招聘决策。

## 3. Required Inputs
- {{job_description}} 岗位 JD。
- {{must_have_requirements}} 必备条件。
- {{nice_to_have_requirements}} 加分项。
- {{candidate_resume}} 候选人简历。
- {{interview_notes}} 面试记录。
- {{evaluation_rubric}} 评估维度和评分标准。
- {{seniority_level}} 职级。
- {{location_or_work_authorization_constraints}} 地点或工作授权限制。
- {{bias_control_rules}} 偏见控制规则。

缺失信息处理：
- 缺少 JD 时只做材料摘要，不做岗位匹配结论。
- 缺少证据时标注“材料不足”，并提出后续验证问题。
- 涉及受保护属性时不得推断或使用，必要时建议咨询 HR/法律合规。

## 4. Prompt Construction Rules
- 必须基于岗位要求评估，而不是泛泛评价候选人。
- 必须区分必备条件、加分项、可训练缺口和阻塞风险。
- 必须要求证据引用，引用简历项目、经历或面试记录。
- 必须标注信息不足和需要进一步验证的问题。
- 必须避免基于年龄、性别、种族、婚育、宗教、残疾、国籍等受保护属性推断。
- 必须输出结构化评分、岗位匹配说明和后续面试问题。
- 必须声明 AI 输出不应作为唯一招聘决策依据。

## 5. Hard Constraints
- 禁止基于受保护属性做推断、排序或建议。
- 禁止给出绝对录用/淘汰结论。
- 禁止凭主观印象评价候选人。
- 禁止忽略岗位 must-have 要求。
- 必须引用证据并给后续验证问题。

## 6. Output Format
最终 prompt 应要求目标模型输出：
- 岗位要求摘要。
- 候选人证据摘要。
- must-have 匹配表。
- nice-to-have 匹配表。
- 结构化评分。
- 风险和信息不足。
- 后续面试问题。
- 决策辅助说明和合规提醒。

## 7. Quality Checklist
- [ ] JD、必备条件和加分项明确。
- [ ] 候选人材料和面试记录明确。
- [ ] 每个判断绑定简历或面试证据。
- [ ] 必备条件和加分项分开评分。
- [ ] 信息不足被标注。
- [ ] 未使用年龄、性别、种族、婚育、宗教、残疾等受保护属性。
- [ ] 输出后续面试问题。
- [ ] 明确 AI 不是唯一决策依据。

## 8. Common Mistakes
| Common Mistake | Risk | Repair |
| --- | --- | --- |
| 凭主观印象判断 | 不公平且不可复核 | 只基于 JD 和材料证据 |
| 忽略岗位要求 | 匹配结论失真 | 先拆 must-have 和 nice-to-have |
| 未引用证据 | 评估不可审计 | 每项评分引用简历或面试片段 |
| 推断受保护属性 | 歧视和合规风险 | 明确禁止使用受保护属性 |
| 绝对录用/淘汰 | 过度依赖 AI | 输出风险、验证问题和辅助建议 |
| 没有后续问题 | 无法补证 | 设计针对缺口的面试问题 |

## 9. Reusable Template
```text
你是 {{target_ai_tool}}，请进行招聘评估。

岗位 JD：{{job_description}}
必备条件：{{must_have_requirements}}
加分项：{{nice_to_have_requirements}}
候选人简历：{{candidate_resume}}
面试记录：{{interview_notes}}
评分标准：{{evaluation_rubric}}
职级：{{seniority_level}}
地点/工作授权限制：{{location_or_work_authorization_constraints}}
偏见控制：{{bias_control_rules}}

执行要求：
1. 先拆解岗位 must-have、nice-to-have 和职级期望。
2. 只基于简历和面试记录证据评估，材料不足必须标注。
3. 禁止基于受保护属性做任何推断或建议。
4. 输出结构化评分、匹配证据、风险和后续面试问题。
5. 明确 AI 输出只用于辅助，不作为唯一招聘决策依据。

输出格式：
岗位摘要、证据摘要、匹配表、评分表、信息不足、后续问题、合规提醒。
```

## 10. Example
用户原始需求：

```text
帮我根据岗位 JD 和简历评估候选人，但要避免偏见。
```

高质量 prompt：

```text
请基于 `[job_description]` 和 `[candidate_resume]` 评估候选人与高级后端工程师岗位的匹配度。先拆分 must-have：Go/Java 服务经验、数据库设计、分布式系统、线上故障处理；再拆分 nice-to-have：Kubernetes、可观测性、团队协作。每个评分必须引用简历或面试记录证据；材料不足标注“待验证”。禁止推断或使用年龄、性别、婚育、种族、宗教、残疾等受保护属性。不要给绝对录用/淘汰结论；输出匹配表、风险、后续面试问题和合规提醒。
```
