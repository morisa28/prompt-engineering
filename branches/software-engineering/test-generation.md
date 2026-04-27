# Test Generation

## 1. Purpose
用于让 agent 为指定行为补充可运行、可维护的测试。 本分支只处理该场景专属 prompt 构造；跨场景原则使用 `../../common-principles.md`。

## 2. Trigger Conditions
- 用户要求写测试
- 用户要求提升覆盖率
- 修复后需要回归测试
- 需要 E2E 或集成测试

## 3. Required Inputs
- {{working_directory}} 工作目录
- {{test_target}} 被测对象
- {{test_framework}} 测试框架
- {{behaviors}} 行为列表
- {{test_command}} 测试命令

缺失信息处理：
- 测试框架未知时从配置文件识别
- 覆盖率目标未知时覆盖关键行为而非追求数字
- 涉及安全、金钱、医疗、法律、生产数据或大范围改动时，缺失的关键边界必须标注为阻塞问题。
- 非阻塞缺失项使用 `[待补充: field]`，并在最终 prompt 中要求目标模型标注假设。

## 4. Prompt Construction Rules
- 覆盖正常路径、异常路径、边界情况
- 测试必须可运行
- 避免过度耦合实现细节
- 输出运行命令和无法测试的部分
- 需要适配 Codex、Codex CLI、Claude Code、Gemini CLI 或 ChatGPT 时，必须加入目标工具的工作方式、权限边界和最终报告格式。

## 5. Hard Constraints
- 禁止删除或弱化现有测试
- 禁止为通过测试而改业务逻辑，除非发现真实 bug 并说明
- 禁止依赖外部不稳定服务
- 必须包含可检查的验收标准。
- 不确定时必须标注假设或提出阻塞问题。

## 6. Output Format
最终 prompt 应要求目标模型输出：
- 测试策略
- 新增/修改测试文件
- 覆盖场景表
- 测试命令与结果
- 未覆盖原因

## 7. Quality Checklist
- [ ] 框架和命令明确
- [ ] 正常/异常/边界均覆盖
- [ ] 断言面向行为
- [ ] 测试可重复运行
- [ ] prompt 包含目标、输入、步骤、约束、输出格式、验收标准和自检。
- [ ] 目标工具的执行环境和限制已写明。

## 8. Common Mistakes
| Common Mistake | Risk | Repair |
| --- | --- | --- |
| 只测实现细节 | 重构时易碎 | 按公开行为断言 |
| 不运行测试 | 无法验证 | 必须跑命令或说明原因 |
| 只测 happy path | 缺缺陷保护 | 列异常和边界 |

## 9. Reusable Template
```text
你是 {{target_ai_tool}}，请处理以下 Test Generation 任务。

任务目标：
{{task_goal}}

输入材料：
- {{working_directory}}
- {{test_target}}
- {{test_framework}}
- {{behaviors}}
- {{test_command}}
- {{input_materials}}

执行要求：
1. 覆盖正常路径、异常路径、边界情况
2. 测试必须可运行
3. 避免过度耦合实现细节
4. 输出运行命令和无法测试的部分

硬性约束：
- 禁止删除或弱化现有测试
- 禁止为通过测试而改业务逻辑，除非发现真实 bug 并说明
- 禁止依赖外部不稳定服务
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
给登录接口生成测试 prompt。
```

高质量 prompt：

```text
你是 Codex，请在 `/repo/api` 为登录接口补充测试。先读取测试配置、登录路由、认证服务和现有测试风格；使用项目已有测试框架覆盖：正确账号登录、错误密码、缺少字段、锁定用户、速率限制或等价边界。禁止删除现有测试或降低断言。运行 `[test_command]`，最终报告新增测试文件、覆盖场景和无法覆盖的原因。
```
