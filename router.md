# Prompt Engineering Router

## 1. Routing Goal
The router converts a raw user request into one primary branch and optional auxiliary branches. The primary branch solves the core scenario; auxiliary branches add execution mode, target-tool adaptation, domain safety, data/report format, or reusable-template behavior.

## 2. Routing Process
1. 判断用户是否在请求 prompt 相关任务：生成 prompt、优化 prompt、扩写 prompt、压缩 prompt、审查 prompt、创建模板、创建 skill、给 AI 工具写任务说明。
2. 判断目标任务领域：通用 prompt、软件工程、文档研究、数据分析、产品设计与商业、AI systems、业务运营、教育课程、创意/游戏设计、多模态、专业领域、沟通与语言、自动化、元技能构建。
3. 判断执行模式：review only、rewrite、expansion、compression、template generation、plan only、direct execution、multi-stage workflow、research synthesis、high-risk domain safe mode。
4. 选择主分支：主分支负责用户任务的核心场景。
5. 选择辅助分支：辅助分支只补充执行模式、工具环境、领域安全要求或输出格式。
6. 输出路由结果：主分支、辅助分支、使用原因、需要收集的关键输入、最终 prompt 生成策略。

## 3. Branch Selection Rules
- `branches/general-prompt/prompt-review.md`：用户说“看看这个 prompt 哪里不好”; 用户要求评分、诊断、找歧义点; 用户已有 prompt 但不确定为什么结果不稳定。
- `branches/general-prompt/prompt-rewrite.md`：用户说“改成 Codex/Claude/Gemini 能执行的 prompt”; 用户要求把口语化需求变成正式 prompt; 用户给出模糊需求但希望交给 AI 处理。
- `branches/general-prompt/prompt-expansion.md`：用户说“写详细一点”; 用户提供短 prompt 并要求增强执行力; 任务复杂但原 prompt 只有目标，没有上下文和验收。
- `branches/general-prompt/prompt-compression.md`：用户说 prompt 太长; 用户要求保留核心约束但减少 token; 用户需要短版/中版/极简版。
- `branches/general-prompt/prompt-template-builder.md`：用户要求做 prompt 模板; 用户要把某类任务固化; 用户想复用一套 AI 工作流。
- `branches/software-engineering/plan-mode.md`：用户要求先 plan; 用户说不要直接改代码; 任务涉及大范围修改、迁移或架构决策。
- `branches/software-engineering/coding-feature-development.md`：用户要求新增功能; 用户要求改前端页面或组件; 用户要求新增接口、交互、状态逻辑。
- `branches/software-engineering/bugfix-debugging.md`：用户提供报错日志; npm run dev/build/test 失败; 页面白屏或功能异常; CLI 命令报错。
- `branches/software-engineering/refactor-architecture.md`：用户要求重构; 用户要求模块拆分或降低耦合; 用户要求提升可维护性但不改变功能。
- `branches/software-engineering/test-generation.md`：用户要求写测试; 用户要求提升覆盖率; 修复后需要回归测试; 需要 E2E 或集成测试。
- `branches/software-engineering/code-review.md`：用户要求 review 代码; 用户要求找 bug、性能问题或安全问题; 用户给 PR/diff 要审查。
- `branches/software-engineering/repository-analysis.md`：用户要求分析整个 repo; 用户想理解项目结构; 用户要找入口文件、技术栈、核心模块。
- `branches/software-engineering/cli-agent.md`：用户目标是 Codex CLI、Gemini CLI、Claude Code; 用户要求写命令行智能体任务说明; 任务需要明确可执行命令和禁止操作。
- `branches/software-engineering/security-threat-modeling.md`：用户要求安全审查或威胁建模; 用户关注权限、数据泄露、认证授权; 用户要求应用安全检查。
- `branches/software-engineering/devops-ci.md`：用户提到 CI/CD、GitHub Actions、GitLab CI、Docker build、pipeline、部署、发布、环境变量、secrets、runner、build failed、deploy failed。
- `branches/software-engineering/database-migration.md`：用户提到 database migration、schema、migration、Prisma、Alembic、Django migration、TypeORM、字段变更、索引、回滚、数据回填。
- `branches/software-engineering/api-design.md`：用户提到 REST API、GraphQL、OpenAPI、接口设计、请求响应、鉴权、错误码、分页、版本控制、联调。
- `branches/ai-systems/knowledge-base-rag.md`：用户提到 RAG、知识库、向量数据库、embedding、检索、rerank、内部文档助手、引用来源、防幻觉。
- `branches/business-operations/customer-service-qa.md`：用户提到客服质检、工单分析、通话转写、客户情绪、服务评分、违规话术、坐席培训。
- `branches/business-operations/recruiting-evaluation.md`：用户提到简历筛选、候选人评估、面试记录、岗位匹配、面试题、招聘打分。
- `branches/education/curriculum-design.md`：用户提到课程大纲、教学计划、学习路径、训练营、作业设计、评估标准、教学活动。
- `branches/creative-design/game-design.md`：用户提到游戏设计、核心玩法、关卡、机制、角色、数值、GDD、玩法循环、原型。
- `branches/documents-research/documentation-analysis.md`：用户要求总结文档; 用户要求整理会议记录或项目资料; 用户要求多文档归纳。
- `branches/documents-research/pdf-to-skill.md`：用户要求 PDF 转 skill; 用户要求从书籍/论文提炼方法并固化; 用户说不要普通摘要而要可复用工作流。
- `branches/documents-research/research-synthesis.md`：用户要求综合多来源研究; 用户要求论文比较或研究报告; 用户要求观点对比。
- `branches/documents-research/academic-writing.md`：用户要求写论文或文献综述; 用户要求研究计划、学术报告; 用户有课程作业写作要求。
- `branches/documents-research/report-writing.md`：用户要求写项目报告或正式文稿; 用户要求实习/课程/阶段总结; 用户要求技术文档。
- `branches/data-analytics/data-analysis.md`：用户要求分析数据集; 用户要求趋势/异常/统计分析; 用户要求业务指标分析。
- `branches/data-analytics/spreadsheet-analysis.md`：用户要求分析 Excel/CSV/表格; 用户要求生成销售/财务/成绩报告; 用户要求透视分析。
- `branches/data-analytics/visualization-dashboard.md`：用户要求做图表或仪表盘; 用户要求 BI 报告; 用户要求指标展示。
- `branches/product-design-business/product-requirements.md`：用户要求写 PRD/MVP; 用户要求产品需求或功能规格; 用户要求用户故事。
- `branches/product-design-business/ux-ui-design.md`：用户要求 UI/UX 设计; 用户要求优化交互流程; 用户要求组件规范或设计稿说明。
- `branches/product-design-business/business-strategy.md`：用户要求商业策略或商业计划; 用户要求市场/竞品/增长分析; 用户要求变现路径。
- `branches/product-design-business/marketing-content.md`：用户要求营销文案/SEO/短视频脚本; 用户要求小红书或邮件营销; 用户要求 A/B 测试文案。
- `branches/multimodal/visual-image-analysis.md`：用户要求分析图片/截图/设计稿; 用户要求定位页面视觉问题; 用户要求看图解题。
- `branches/multimodal/video-audio-analysis.md`：用户要求视频/音频分析; 用户要求会议录音整理; 用户要求课程视频时间轴笔记。
- `branches/multimodal/visual-3d-interaction.md`：用户要求 3D/Spline/Blender 交互; 用户要求旋钮拖拽、模型对齐、动画调整; 用户要求视觉验证节点。
- `branches/domain-specific/legal-policy-review.md`：用户要求分析合同风险; 用户要求政策/合规/隐私条款审查; 用户要求法律文件摘要。
- `branches/domain-specific/medical-health-info.md`：用户要求解释症状或体检报告; 用户要求准备看医生; 用户要求医学资料总结。
- `branches/domain-specific/finance-investment-analysis.md`：用户要求金融/投资/财务分析; 用户要求股票基金加密资产研究; 用户要求预算或财务规划。
- `branches/domain-specific/education-tutoring.md`：用户要求学习辅导或讲题; 用户要求复习计划; 用户要求知识点拆解。
- `branches/communication/translation-localization.md`：用户要求翻译/本地化; 用户要求润色或语气改写; 用户要求双语对照。
- `branches/communication/roleplay-simulation.md`：用户要求面试/谈判/客服模拟; 用户要求销售话术演练; 用户要求英语口语练习。
- `branches/automation/automation-workflow.md`：用户要求自动化流程/脚本; 用户要求 n8n/Zapier/Make 工作流; 用户要求定时数据同步或报告生成。
- `branches/meta/meta-skill-builder.md`：用户要求把流程做成 skill; 用户要求把书籍/文档/经验提炼为 skill; 用户要求创建新的 AI 工作流。

## 3.1 New Branch Auxiliary Routing Rules
- DevOps / CI：主分支 `branches/software-engineering/devops-ci.md`；常见辅助分支 `branches/software-engineering/bugfix-debugging.md`、`branches/software-engineering/cli-agent.md`、`branches/software-engineering/security-threat-modeling.md`。
- 数据库迁移：主分支 `branches/software-engineering/database-migration.md`；常见辅助分支 `branches/software-engineering/plan-mode.md`、`branches/software-engineering/bugfix-debugging.md`、`branches/software-engineering/security-threat-modeling.md`。
- API 设计：主分支 `branches/software-engineering/api-design.md`；常见辅助分支 `branches/product-design-business/product-requirements.md`、`branches/software-engineering/coding-feature-development.md`、`branches/software-engineering/test-generation.md`。
- 知识库 / RAG：主分支 `branches/ai-systems/knowledge-base-rag.md`；常见辅助分支 `branches/documents-research/documentation-analysis.md`、`branches/documents-research/research-synthesis.md`、`branches/software-engineering/api-design.md`、`branches/software-engineering/security-threat-modeling.md`。
- 客服质检：主分支 `branches/business-operations/customer-service-qa.md`；常见辅助分支 `branches/documents-research/documentation-analysis.md`、`branches/data-analytics/data-analysis.md`、涉及合规时使用 `branches/domain-specific/legal-policy-review.md`。
- 招聘评估：主分支 `branches/business-operations/recruiting-evaluation.md`；常见辅助分支 `branches/communication/roleplay-simulation.md`、`branches/documents-research/documentation-analysis.md`、涉及合规或歧视风险时使用 `branches/domain-specific/legal-policy-review.md`。
- 课程设计：主分支 `branches/education/curriculum-design.md`；常见辅助分支 `branches/domain-specific/education-tutoring.md`、`branches/documents-research/report-writing.md`、`branches/general-prompt/prompt-template-builder.md`。
- 游戏设计：主分支 `branches/creative-design/game-design.md`；常见辅助分支 `branches/product-design-business/ux-ui-design.md`、需要实现原型时使用 `branches/software-engineering/coding-feature-development.md`、需要分析视觉素材时使用 `branches/multimodal/visual-image-analysis.md`、需要 GDD 文档时使用 `branches/documents-research/report-writing.md`。

## 4. Multi-Branch Combination Rules
- 用户要求“先 plan，再修 bug”：主分支 `branches/software-engineering/bugfix-debugging.md`；辅助分支 `branches/software-engineering/plan-mode.md`、`branches/software-engineering/cli-agent.md`。
- 用户要求“分析 repo 并给 Codex CLI 写 prompt”：主分支 `branches/software-engineering/repository-analysis.md`；辅助分支 `branches/software-engineering/cli-agent.md`、`branches/general-prompt/prompt-rewrite.md`。
- 用户要求“读取 PDF 并提炼为 skill”：主分支 `branches/documents-research/pdf-to-skill.md`；辅助分支 `branches/meta/meta-skill-builder.md`、`branches/documents-research/documentation-analysis.md`。
- 用户要求“分析合同并指出风险”：主分支 `branches/domain-specific/legal-policy-review.md`；辅助分支 `branches/documents-research/documentation-analysis.md`。
- 用户要求“根据表格每天自动生成销售报告”：主分支 `branches/automation/automation-workflow.md`；辅助分支 `branches/data-analytics/spreadsheet-analysis.md`、`branches/documents-research/report-writing.md`。
- 用户要求“生成营销文案并做 A/B 测试”：主分支 `branches/product-design-business/marketing-content.md`；辅助分支按重点选择 `branches/product-design-business/business-strategy.md` 或 `branches/data-analytics/visualization-dashboard.md`。
- 用户要求“做 3D 交互并让 Codex 实现”：主分支 `branches/multimodal/visual-3d-interaction.md`；辅助分支 `branches/software-engineering/coding-feature-development.md`、`branches/software-engineering/cli-agent.md`。
- 用户要求“法律/医疗/金融材料总结成报告”：主分支使用对应 `domain-specific` 分支；辅助分支 `branches/documents-research/report-writing.md`。
- 用户要求“CI 构建失败，让 Codex 修”：主分支 `branches/software-engineering/devops-ci.md`；辅助分支 `branches/software-engineering/bugfix-debugging.md`、`branches/software-engineering/cli-agent.md`。
- 用户要求“线上数据库要改 schema，先给迁移方案”：主分支 `branches/software-engineering/database-migration.md`；辅助分支 `branches/software-engineering/plan-mode.md`、`branches/software-engineering/security-threat-modeling.md`。
- 用户要求“把这个产品功能设计成 API”：主分支 `branches/software-engineering/api-design.md`；辅助分支 `branches/product-design-business/product-requirements.md`。
- 用户要求“做一个内部知识库问答系统”：主分支 `branches/ai-systems/knowledge-base-rag.md`；辅助分支 `branches/documents-research/documentation-analysis.md`、`branches/software-engineering/api-design.md`、`branches/software-engineering/security-threat-modeling.md`。
- 用户要求“分析一批客服对话并统计问题类型”：主分支 `branches/business-operations/customer-service-qa.md`；辅助分支 `branches/data-analytics/data-analysis.md`、`branches/documents-research/report-writing.md`。
- 用户要求“根据候选人简历设计面试问题并模拟面试”：主分支 `branches/business-operations/recruiting-evaluation.md`；辅助分支 `branches/communication/roleplay-simulation.md`。
- 用户要求“设计一个训练营课程，并输出营销文案”：主分支 `branches/education/curriculum-design.md`；辅助分支 `branches/product-design-business/marketing-content.md`、`branches/documents-research/report-writing.md`。
- 用户要求“设计游戏玩法并让 Codex 实现原型”：主分支 `branches/creative-design/game-design.md`；辅助分支 `branches/software-engineering/coding-feature-development.md`、`branches/product-design-business/ux-ui-design.md`。

## 5. Routing Examples
### Example 1
- 用户原始需求：帮我看看这个 prompt 哪里不好。
- 路由判断：已有 prompt 诊断
- 主分支：`branches/general-prompt/prompt-review.md`
- 辅助分支：无，若要求改写再加 prompt-rewrite
- 需要补充的信息：原 prompt、目标工具、期望结果、坏输出
- prompt 生成策略：先按八维评分，再给优化版和改动理由。
### Example 2
- 用户原始需求：把这句话改成 Codex 可以直接执行的 prompt。
- 路由判断：口语需求转 Codex 指令
- 主分支：`branches/general-prompt/prompt-rewrite.md`
- 辅助分支：branches/software-engineering/cli-agent.md
- 需要补充的信息：工作目录、任务领域、允许命令、禁止操作
- prompt 生成策略：补齐 Codex 工作目录、步骤、约束、验证和报告格式。
### Example 3
- 用户原始需求：让 Codex 先 plan，不要直接改代码。
- 路由判断：plan only
- 主分支：`branches/software-engineering/plan-mode.md`
- 辅助分支：branches/software-engineering/cli-agent.md
- 需要补充的信息：工作目录、目标、先读文件、风险关注
- prompt 生成策略：写 no-edit prompt，要求读取、分析、计划、验证和待确认问题。
### Example 4
- 用户原始需求：我的项目 npm run dev 报错了，让 Codex 修。
- 路由判断：debug 修复
- 主分支：`branches/software-engineering/bugfix-debugging.md`
- 辅助分支：branches/software-engineering/cli-agent.md
- 需要补充的信息：完整日志、复现步骤、环境、最近改动
- prompt 生成策略：定位根因、最小修复、运行原命令验证。
### Example 5
- 用户原始需求：帮我写一个 prompt，让 AI 分析整个 repo。
- 路由判断：仓库理解
- 主分支：`branches/software-engineering/repository-analysis.md`
- 辅助分支：branches/general-prompt/prompt-expansion.md
- 需要补充的信息：仓库路径、关注点、输出深度
- prompt 生成策略：只读分析，读取 README/配置/入口并输出项目报告。
### Example 6
- 用户原始需求：读取这本 PDF，把方法提炼成 skill。
- 路由判断：PDF 转 skill
- 主分支：`branches/documents-research/pdf-to-skill.md`
- 辅助分支：branches/meta/meta-skill-builder.md; branches/documents-research/documentation-analysis.md
- 需要补充的信息：PDF 路径、阅读范围、目标 skill
- prompt 生成策略：多轮阅读，规则/模板/checklist/示例化。
### Example 7
- 用户原始需求：帮我分析 Excel 表格并生成销售报告。
- 路由判断：表格分析加报告
- 主分支：`branches/data-analytics/spreadsheet-analysis.md`
- 辅助分支：branches/documents-research/report-writing.md
- 需要补充的信息：表格路径、字段、汇总维度、报告读者
- prompt 生成策略：先做表结构和数据质量，再透视汇总并写报告。
### Example 8
- 用户原始需求：帮我写一个 PRD。
- 路由判断：产品需求
- 主分支：`branches/product-design-business/product-requirements.md`
- 辅助分支：branches/general-prompt/prompt-template-builder.md
- 需要补充的信息：产品目标、用户画像、场景、成功指标
- prompt 生成策略：输出 MVP、用户故事、验收标准和非目标。
### Example 9
- 用户原始需求：生成小红书营销文案。
- 路由判断：营销内容
- 主分支：`branches/product-design-business/marketing-content.md`
- 辅助分支：可加 business-strategy 做定位
- 需要补充的信息：产品、受众、渠道、转化目标、语气
- prompt 生成策略：生成多版本、标题、标签和 A/B 测试。
### Example 10
- 用户原始需求：分析一份合同里的风险。
- 路由判断：法律高风险
- 主分支：`branches/domain-specific/legal-policy-review.md`
- 辅助分支：branches/documents-research/documentation-analysis.md
- 需要补充的信息：司法辖区、合同类型、文本、风险关注
- prompt 生成策略：信息性风险识别，非法律意见，列义务和律师问题。
### Example 11
- 用户原始需求：整理症状，帮我准备去看医生。
- 路由判断：医疗高风险
- 主分支：`branches/domain-specific/medical-health-info.md`
- 辅助分支：无
- 需要补充的信息：症状、时间线、病史、用药、检查资料
- prompt 生成策略：整理时间线和就医问题，提醒急症，不诊断。
### Example 12
- 用户原始需求：设计一个 n8n 自动化流程。
- 路由判断：自动化流程
- 主分支：`branches/automation/automation-workflow.md`
- 辅助分支：可加 data-analytics 或 report-writing
- 需要补充的信息：触发、输入、输出、工具、失败处理
- prompt 生成策略：节点、字段映射、重试、日志、通知和测试步骤。
### Example 13
- 用户原始需求：给登录接口写单元测试。
- 路由判断：测试生成
- 主分支：`branches/software-engineering/test-generation.md`
- 辅助分支：branches/software-engineering/cli-agent.md
- 需要补充的信息：接口路径、测试框架、行为、命令
- prompt 生成策略：覆盖正常、异常、边界并运行测试。
### Example 14
- 用户原始需求：帮我把客服对话做成角色扮演训练。
- 路由判断：角色模拟
- 主分支：`branches/communication/roleplay-simulation.md`
- 辅助分支：branches/communication/translation-localization.md 若跨语言
- 需要补充的信息：角色、场景、目标、难度、反馈方式
- prompt 生成策略：多轮互动，一轮一问，按维度反馈。
### Example 15
- 用户原始需求：帮我审查这个 API 的权限设计。
- 路由判断：防御性安全
- 主分支：`branches/software-engineering/security-threat-modeling.md`
- 辅助分支：branches/software-engineering/code-review.md
- 需要补充的信息：系统范围、资产、角色、边界、代码路径
- prompt 生成策略：资产/边界/攻击面/风险/防护建议，不给利用步骤。
### Example 16
- 用户原始需求：帮我写一个 prompt，让 Codex 修复 GitHub Actions 构建失败。
- 路由判断：DevOps / CI 构建失败修复。
- 主分支：`branches/software-engineering/devops-ci.md`
- 辅助分支：`branches/software-engineering/bugfix-debugging.md`, `branches/software-engineering/cli-agent.md`
- 需要补充的信息：仓库路径、workflow 文件、失败日志、构建/测试命令、runner 环境、secrets 名称。
- prompt 生成策略：先读 CI 配置和日志，区分本地与 runner 失败，最小修改并输出重跑 workflow 和 rollback。
### Example 17
- 用户原始需求：我需要给线上 PostgreSQL 做 schema migration，先让 Codex plan。
- 路由判断：生产数据库迁移，需要 plan 和安全边界。
- 主分支：`branches/software-engineering/database-migration.md`
- 辅助分支：`branches/software-engineering/plan-mode.md`, `branches/software-engineering/security-threat-modeling.md`
- 需要补充的信息：schema、migration 工具、数据量、备份、停机容忍、受影响服务。
- prompt 生成策略：只规划不执行，识别破坏性变更，输出分阶段迁移、备份、rollback 和 staging 验证。
### Example 18
- 用户原始需求：帮我设计一个 REST API，要有鉴权、错误码、分页和 OpenAPI。
- 路由判断：API 契约设计。
- 主分支：`branches/software-engineering/api-design.md`
- 辅助分支：`branches/product-design-business/product-requirements.md`, `branches/software-engineering/test-generation.md`
- 需要补充的信息：调用方、资源模型、字段、鉴权、错误场景、版本策略。
- prompt 生成策略：定义接口表、schema、错误响应、鉴权、分页、OpenAPI、示例和测试场景。
### Example 19
- 用户原始需求：帮我做一个公司内部文档 RAG 知识库，回答必须带引用。
- 路由判断：RAG 系统设计。
- 主分支：`branches/ai-systems/knowledge-base-rag.md`
- 辅助分支：`branches/documents-research/documentation-analysis.md`, `branches/software-engineering/api-design.md`, `branches/software-engineering/security-threat-modeling.md`
- 需要补充的信息：知识源、用户、问答范围、chunk、embedding、向量库、权限、评估集。
- prompt 生成策略：建立文档索引，设计检索/rerank/生成流程，强制引用、不命中拒答和评估指标。
### Example 20
- 用户原始需求：帮我质检客服对话，输出评分和改进建议。
- 路由判断：客服质检和培训反馈。
- 主分支：`branches/business-operations/customer-service-qa.md`
- 辅助分支：`branches/documents-research/documentation-analysis.md`, `branches/data-analytics/data-analysis.md`
- 需要补充的信息：对话、业务场景、渠道、评分标准、合规规则、隐私要求。
- prompt 生成策略：脱敏后按 rubric 评分，每项引用证据，输出改进建议和替代话术。
### Example 21
- 用户原始需求：帮我根据 JD 和简历评估候选人，并设计面试问题。
- 路由判断：招聘评估与面试问题生成。
- 主分支：`branches/business-operations/recruiting-evaluation.md`
- 辅助分支：`branches/communication/roleplay-simulation.md`
- 需要补充的信息：JD、must-have、nice-to-have、简历、面试记录、评分标准、偏见控制。
- prompt 生成策略：基于岗位证据评分，排除受保护属性，输出待验证风险和面试问题。
### Example 22
- 用户原始需求：帮我设计一个 6 周的 Python 数据分析课程。
- 路由判断：课程设计。
- 主分支：`branches/education/curriculum-design.md`
- 辅助分支：`branches/domain-specific/education-tutoring.md`, `branches/documents-research/report-writing.md`
- 需要补充的信息：学习者画像、先修知识、目标、授课形式、练习要求、最终项目。
- prompt 生成策略：按周设计目标、模块、活动、作业、评估和最终项目。
### Example 23
- 用户原始需求：帮我设计一个 roguelike 小游戏，并输出可开发原型 prompt。
- 路由判断：游戏设计加原型开发。
- 主分支：`branches/creative-design/game-design.md`
- 辅助分支：`branches/software-engineering/coding-feature-development.md`, `branches/product-design-business/ux-ui-design.md`
- 需要补充的信息：类型、玩家、平台、核心循环、机制、关卡目标、原型范围、成功指标。
- prompt 生成策略：先输出 GDD 和可测试原型范围，再把实现任务交给开发分支。
