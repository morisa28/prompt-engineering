# Security Threat Modeling

## 1. Purpose
用于识别资产、攻击面、权限边界和防护措施，不生成攻击性利用步骤。 本分支只处理该场景专属 prompt 构造；跨场景原则使用 `../../common-principles.md`。

## 2. Trigger Conditions
- 用户要求安全审查或威胁建模
- 用户关注权限、数据泄露、认证授权
- 用户要求应用安全检查

## 3. Required Inputs
- {{system_scope}} 系统范围
- {{assets}} 资产
- {{trust_boundaries}} 信任边界
- {{threat_focus}} 威胁关注点
- {{code_or_docs}} 代码或文档

缺失信息处理：
- 资产未知时先从数据、凭证、权限、用户角色抽取
- 风险等级采用 High/Medium/Low 并说明依据
- 涉及安全、金钱、医疗、法律、生产数据或大范围改动时，缺失的关键边界必须标注为阻塞问题。
- 非阻塞缺失项使用 `[待补充: field]`，并在最终 prompt 中要求目标模型标注假设。

## 4. Prompt Construction Rules
- 识别资产、入口、攻击面、权限边界
- 输出风险等级和防护建议
- 只提供防御性检查和修复建议
- 标注证据和不确定性
- 需要适配 Codex、Codex CLI、Claude Code、Gemini CLI 或 ChatGPT 时，必须加入目标工具的工作方式、权限边界和最终报告格式。

## 5. Hard Constraints
- 禁止生成可操作攻击步骤、payload 或绕过方案
- 禁止把未经验证的风险写成已存在漏洞
- 必须建议安全测试和审计边界
- 必须包含可检查的验收标准。
- 不确定时必须标注假设或提出阻塞问题。

## 6. Output Format
最终 prompt 应要求目标模型输出：
- 范围
- 资产表
- 攻击面
- 风险清单
- 防护建议
- 验证建议
- 不确定性

## 7. Quality Checklist
- [ ] 资产和边界明确
- [ ] 风险有影响和可能性
- [ ] 建议可防御执行
- [ ] 无攻击性步骤
- [ ] prompt 包含目标、输入、步骤、约束、输出格式、验收标准和自检。
- [ ] 目标工具的执行环境和限制已写明。

## 8. Common Mistakes
| Common Mistake | Risk | Repair |
| --- | --- | --- |
| 列漏洞不列资产 | 缺优先级 | 先资产和边界 |
| 给利用细节 | 安全风险 | 只写防御验证和修复 |
| 风险无证据 | 误导团队 | 标注推断依据 |

## 9. Reusable Template
```text
你是 {{target_ai_tool}}，请处理以下 Security Threat Modeling 任务。

任务目标：
{{task_goal}}

输入材料：
- {{system_scope}}
- {{assets}}
- {{trust_boundaries}}
- {{threat_focus}}
- {{not_offensive_security_notice}}
- {{input_materials}}

执行要求：
1. 识别资产、入口、攻击面、权限边界
2. 输出风险等级和防护建议
3. 只提供防御性检查和修复建议
4. 标注证据和不确定性

硬性约束：
- 禁止生成可操作攻击步骤、payload 或绕过方案
- 禁止把未经验证的风险写成已存在漏洞
- 必须建议安全测试和审计边界
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
写一个 prompt，让 AI 给我的 API 做威胁建模。
```

高质量 prompt：

```text
请对 `/repo/api` 做防御性威胁建模。先读取认证、授权、输入校验、日志和数据访问相关文件；识别资产、入口、信任边界和角色权限；输出 High/Medium/Low 风险、影响、证据、修复建议和验证方式。禁止生成攻击 payload、绕过步骤或可直接利用的说明；未经证据支持的内容必须标注为假设。
```
