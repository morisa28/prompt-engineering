# Bugfix Debugging

## 1. Purpose
用于让代码 agent 根据日志、复现步骤和环境信息定位根因并最小修复。 本分支只处理该场景专属 prompt 构造；跨场景原则使用 `../../common-principles.md`。

## 2. Trigger Conditions
- 用户提供报错日志
- npm run dev/build/test 失败
- 页面白屏或功能异常
- CLI 命令报错

## 3. Required Inputs
- {{working_directory}} 工作目录
- {{error_log}} 完整日志
- {{reproduction_steps}} 复现步骤
- {{environment}} 环境
- {{recent_changes}} 最近改动

缺失信息处理：
- 日志不完整时先要求补充关键栈
- 最近改动未知时要求读取 git diff 或相关文件但不回退
- 涉及安全、金钱、医疗、法律、生产数据或大范围改动时，缺失的关键边界必须标注为阻塞问题。
- 非阻塞缺失项使用 `[待补充: field]`，并在最终 prompt 中要求目标模型标注假设。

## 4. Prompt Construction Rules
- 先定位根因，再修复
- 区分代码、配置、依赖、环境和数据问题
- 采用最小修复，禁止无关重构
- 修复后运行复现命令和相关测试
- 需要适配 Codex、Codex CLI、Claude Code、Gemini CLI 或 ChatGPT 时，必须加入目标工具的工作方式、权限边界和最终报告格式。

## 5. Hard Constraints
- 禁止删除测试或降低断言
- 禁止通过注释代码掩盖错误
- 禁止全仓库格式化
- 必须包含可检查的验收标准。
- 不确定时必须标注假设或提出阻塞问题。

## 6. Output Format
最终 prompt 应要求目标模型输出：
- 根因
- 修改文件
- 关键改动
- 验证命令与结果
- 剩余风险

## 7. Quality Checklist
- [ ] 日志和复现步骤被使用
- [ ] 根因有文件或命令证据
- [ ] 修复范围最小
- [ ] 验证覆盖原失败路径
- [ ] prompt 包含目标、输入、步骤、约束、输出格式、验收标准和自检。
- [ ] 目标工具的执行环境和限制已写明。

## 8. Common Mistakes
| Common Mistake | Risk | Repair |
| --- | --- | --- |
| 看到报错就猜 | 修错位置 | 按日志和源码证据定位 |
| 顺手重构 | 引入回归 | 限定最小修复 |
| 不跑复现命令 | 无法确认 | 必须验证或说明无法运行原因 |

## 9. Reusable Template
```text
你是 {{target_ai_tool}}，请处理以下 Bugfix Debugging 任务。

任务目标：
{{task_goal}}

输入材料：
- {{working_directory}}
- {{error_log}}
- {{reproduction_steps}}
- {{recent_changes}}
- {{environment}}
- {{verification_steps}}
- {{input_materials}}

执行要求：
1. 先定位根因，再修复
2. 区分代码、配置、依赖、环境和数据问题
3. 采用最小修复，禁止无关重构
4. 修复后运行复现命令和相关测试

硬性约束：
- 禁止删除测试或降低断言
- 禁止通过注释代码掩盖错误
- 禁止全仓库格式化
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
我的项目 npm run dev 报错了，让 Codex 修。
```

高质量 prompt：

```text
你是 Codex，请修复 `/repo/app` 中 `npm run dev` 报错。输入日志：`[粘贴完整 error_log]`。复现步骤：运行 `npm run dev`。先读 `package.json`、Vite/Next 配置和栈中出现的源码；定位根因并说明证据；只改必要文件，禁止无关重构、新增依赖或删除测试。修复后重新运行 `npm run dev` 或 `npm run build`，最终报告根因、修改文件、验证结果和剩余风险。
```
