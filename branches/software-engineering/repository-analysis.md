# Repository Analysis

## 1. Purpose
用于让 agent 在不修改文件的前提下建立项目理解报告。 本分支只处理该场景专属 prompt 构造；跨场景原则使用 `../../common-principles.md`。

## 2. Trigger Conditions
- 用户要求分析整个 repo
- 用户想理解项目结构
- 用户要找入口文件、技术栈、核心模块

## 3. Required Inputs
- {{working_directory}} 仓库目录
- {{analysis_goal}} 分析目标
- {{focus_areas}} 关注模块
- {{output_depth}} 深度

缺失信息处理：
- 未指定关注点时覆盖结构、技术栈、入口、运行、测试、风险
- 大型仓库先做顶层和核心路径抽样
- 涉及安全、金钱、医疗、法律、生产数据或大范围改动时，缺失的关键边界必须标注为阻塞问题。
- 非阻塞缺失项使用 `[待补充: field]`，并在最终 prompt 中要求目标模型标注假设。

## 4. Prompt Construction Rules
- 先读 README，再读配置文件
- 建立文件树
- 识别核心模块和数据流
- 不立即修改
- 输出结构化项目报告
- 需要适配 Codex、Codex CLI、Claude Code、Gemini CLI 或 ChatGPT 时，必须加入目标工具的工作方式、权限边界和最终报告格式。

## 5. Hard Constraints
- 禁止修改文件
- 禁止把未读目录写成确定结论
- 禁止生成与项目无关的教程
- 必须包含可检查的验收标准。
- 不确定时必须标注假设或提出阻塞问题。

## 6. Output Format
最终 prompt 应要求目标模型输出：
- 项目概览
- 技术栈
- 目录树
- 入口和运行命令
- 核心模块
- 数据流
- 测试与构建
- 风险和建议

## 7. Quality Checklist
- [ ] README 和配置已纳入
- [ ] 核心模块有证据
- [ ] 运行命令明确
- [ ] 推断和事实分开
- [ ] prompt 包含目标、输入、步骤、约束、输出格式、验收标准和自检。
- [ ] 目标工具的执行环境和限制已写明。

## 8. Common Mistakes
| Common Mistake | Risk | Repair |
| --- | --- | --- |
| 只列文件树 | 缺项目理解 | 解释模块职责和入口 |
| 不读配置 | 技术栈误判 | 读取 package/pyproject 等 |
| 立即改代码 | 偏离分析任务 | 明确 read-only |

## 9. Reusable Template
```text
你是 {{target_ai_tool}}，请处理以下 Repository Analysis 任务。

任务目标：
{{task_goal}}

输入材料：
- {{working_directory}}
- {{analysis_goal}}
- {{focus_areas}}
- {{output_depth}}
- {{input_materials}}

执行要求：
1. 先读 README，再读配置文件
2. 建立文件树
3. 识别核心模块和数据流
4. 不立即修改
5. 输出结构化项目报告

硬性约束：
- 禁止修改文件
- 禁止把未读目录写成确定结论
- 禁止生成与项目无关的教程
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
帮我写一个 prompt，让 AI 分析整个 repo。
```

高质量 prompt：

```text
你是 Codex，请只读不改地分析 `/repo/app`。先读取 README、依赖配置、构建/测试配置和顶层目录；再识别入口文件、核心模块、数据流、运行命令和测试方式。输出项目概览、目录结构、技术栈证据、关键模块表、潜在风险、后续可执行任务。未读取的路径不得写成确定结论。
```
