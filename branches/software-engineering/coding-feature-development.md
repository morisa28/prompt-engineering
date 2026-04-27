# Coding Feature Development

## 1. Purpose
用于把功能目标转化为代码 agent 可执行的开发任务。 本分支只处理该场景专属 prompt 构造；跨场景原则使用 `../../common-principles.md`。

## 2. Trigger Conditions
- 用户要求新增功能
- 用户要求改前端页面或组件
- 用户要求新增接口、交互、状态逻辑

## 3. Required Inputs
- {{working_directory}} 工作目录
- {{feature_goal}} 功能目标
- {{user_flow}} 用户流程
- {{tech_stack}} 技术栈
- {{modification_scope}} 修改边界

缺失信息处理：
- 技术栈未知时从配置文件识别
- 测试命令未知时从项目配置推断并报告
- 涉及安全、金钱、医疗、法律、生产数据或大范围改动时，缺失的关键边界必须标注为阻塞问题。
- 非阻塞缺失项使用 `[待补充: field]`，并在最终 prompt 中要求目标模型标注假设。

## 4. Prompt Construction Rules
- 先读相关文件和配置
- 明确用户流程、数据流、状态和错误状态
- 保持现有功能，按最小可行改动实现
- 修改后运行测试、构建或浏览器验证
- 需要适配 Codex、Codex CLI、Claude Code、Gemini CLI 或 ChatGPT 时，必须加入目标工具的工作方式、权限边界和最终报告格式。

## 5. Hard Constraints
- 禁止破坏现有路由、公开 API 和用户数据
- 禁止新增依赖，除非说明必要性并获许可
- 禁止无关重构
- 必须包含可检查的验收标准。
- 不确定时必须标注假设或提出阻塞问题。

## 6. Output Format
最终 prompt 应要求目标模型输出：
- 实现方案
- 修改文件
- 关键逻辑
- 验证结果
- 剩余风险

## 7. Quality Checklist
- [ ] 功能目标可验证
- [ ] 用户流程覆盖正常和错误状态
- [ ] 修改边界明确
- [ ] 验证命令或手动检查明确
- [ ] prompt 包含目标、输入、步骤、约束、输出格式、验收标准和自检。
- [ ] 目标工具的执行环境和限制已写明。

## 8. Common Mistakes
| Common Mistake | Risk | Repair |
| --- | --- | --- |
| 只描述功能不约束边界 | 改动扩大 | 列允许/禁止文件 |
| 不写验收 | 功能是否完成不清楚 | 写用户路径和验证命令 |
| 忽略现有风格 | 代码不一致 | 要求沿用项目模式 |

## 9. Reusable Template
```text
你是 {{target_ai_tool}}，请处理以下 Coding Feature Development 任务。

任务目标：
{{task_goal}}

输入材料：
- {{working_directory}}
- {{feature_goal}}
- {{user_flow}}
- {{related_files}}
- {{verification_steps}}
- {{input_materials}}

执行要求：
1. 先读相关文件和配置
2. 明确用户流程、数据流、状态和错误状态
3. 保持现有功能，按最小可行改动实现
4. 修改后运行测试、构建或浏览器验证

硬性约束：
- 禁止破坏现有路由、公开 API 和用户数据
- 禁止新增依赖，除非说明必要性并获许可
- 禁止无关重构
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
帮我写一个 prompt，让 Codex 给设置页加暗色模式开关。
```

高质量 prompt：

```text
你是 Codex，请在 `/repo/web` 为设置页添加暗色模式开关。先读取 `package.json`、设置页组件、主题/样式文件和现有状态管理；实现一个可切换并持久化的开关，保持现有设置项不变。禁止新增依赖和重写页面结构。完成后运行 `npm run build`，若有测试运行相关测试。最终报告修改文件、状态流、验证结果和未覆盖风险。
```
