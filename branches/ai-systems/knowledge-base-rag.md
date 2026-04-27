# Knowledge Base RAG

## 1. Purpose
用于为知识库、RAG 系统、文档切分、embedding、向量数据库、检索、reranking、引用溯源、内部文档助手、防幻觉和检索评估生成 prompt。本分支关注知识边界、检索质量、引用和权限控制。

## 2. Trigger Conditions
- 用户提到 RAG、知识库、向量数据库、embedding、retrieval、rerank、内部文档助手。
- 用户要求问答系统必须带引用、避免幻觉、回答仅基于知识库。
- 用户要求设计 chunking、metadata、权限、更新机制或 retrieval evaluation。

## 3. Required Inputs
- {{knowledge_sources}} 知识源。
- {{document_types}} 文档类型。
- {{target_users}} 目标用户。
- {{qa_scope}} 问答范围和非范围。
- {{chunking_strategy}} chunk 策略。
- {{embedding_model}} embedding 模型。
- {{vector_database}} 向量数据库。
- {{retrieval_strategy}} 检索策略。
- {{reranking_strategy}} 重排策略。
- {{citation_requirement}} 引用要求。
- {{access_control}} 权限控制。
- {{evaluation_questions}} 评估问题集。
- {{freshness_requirement}} 更新频率和时效要求。

缺失信息处理：
- 知识源未知时必须先要求列出文档库、路径、URL、所有者和更新频率。
- chunk、embedding 或向量库未知时可以输出方案选型，但必须标注假设。
- 权限未知时默认按最小权限设计，不允许跨用户或跨部门泄露文档。

## 4. Prompt Construction Rules
- 必须明确知识库回答范围、禁止回答范围和不命中时的答复策略。
- 必须要求建立文档索引，包含来源、标题、路径、版本、更新时间、权限和 metadata。
- 必须设计 chunking、overlap、metadata、去重和版本管理。
- 必须说明检索、重排、上下文组装、生成和引用输出流程。
- 必须要求回答带引用，并能回溯到文档、段落或 chunk。
- 必须要求不确定或检索不到依据时说明找不到依据，不得脱离知识库编造。
- 必须设计评估问题、命中率、引用准确率、拒答准确率和人工抽检流程。
- 必须考虑权限、数据更新、失效文档、敏感信息和审计日志。

## 5. Hard Constraints
- 禁止要求模型无引用回答内部知识问题。
- 禁止允许模型在检索不到依据时编造答案。
- 禁止忽略权限控制、文档更新和版本管理。
- 禁止把所有文档一刀切 chunk 而不定义 metadata。
- 必须包含评估集、检索指标、引用格式和不命中策略。

## 6. Output Format
最终 prompt 应要求目标模型输出：
- 知识源和范围。
- 文档索引字段。
- chunking 与 metadata 方案。
- embedding、向量库、检索和 rerank 流程。
- 生成回答规则和引用格式。
- 权限、更新和审计策略。
- 评估集、指标和测试流程。
- 风险和待确认问题。

## 7. Quality Checklist
- [ ] 知识源、文档类型和目标用户明确。
- [ ] 问答范围和非范围明确。
- [ ] chunking、overlap、metadata 和版本策略明确。
- [ ] embedding、向量库、检索和 rerank 策略明确。
- [ ] 回答必须带引用，引用格式明确。
- [ ] 不命中或不确定时有拒答/澄清规则。
- [ ] 权限控制、更新机制和审计被覆盖。
- [ ] 评估问题和检索/引用指标明确。

## 8. Common Mistakes
| Common Mistake | Risk | Repair |
| --- | --- | --- |
| 只说做知识库 | 范围不清 | 列知识源、用户、问答范围和非范围 |
| 没有 chunk 策略 | 检索质量差 | 定义 chunk size、overlap、结构化切分和 metadata |
| 没有引用 | 无法验证答案 | 要求文档、标题、段落或 chunk 引用 |
| 没有检索评估 | 系统质量不可衡量 | 设计问题集、命中率、引用准确率和拒答准确率 |
| 放任模型编造 | 高幻觉 | 设置仅基于检索证据回答和不命中策略 |
| 忽略权限和更新 | 泄露或过期答案 | 加访问控制、版本、刷新和审计 |

## 9. Reusable Template
```text
你是 {{target_ai_tool}}，请设计 Knowledge Base / RAG prompt。

知识源：{{knowledge_sources}}
文档类型：{{document_types}}
目标用户：{{target_users}}
问答范围：{{qa_scope}}
chunk 策略：{{chunking_strategy}}
embedding 模型：{{embedding_model}}
向量数据库：{{vector_database}}
检索策略：{{retrieval_strategy}}
rerank 策略：{{reranking_strategy}}
引用要求：{{citation_requirement}}
权限控制：{{access_control}}
评估问题：{{evaluation_questions}}
时效要求：{{freshness_requirement}}

执行要求：
1. 建立文档索引字段：来源、标题、路径、版本、更新时间、权限、metadata。
2. 设计 chunking、embedding、检索、rerank、上下文组装和生成流程。
3. 回答必须带引用；没有检索依据时必须说明找不到依据。
4. 设计权限控制、更新机制、过期文档处理和审计日志。
5. 设计评估集和指标：命中率、引用准确率、拒答准确率、人工抽检。

输出格式：
范围、架构、索引字段、chunk/metadata、检索流程、回答规则、权限更新、评估方案、风险。
```

## 10. Example
用户原始需求：

```text
帮我设计一个内部文档 RAG 知识库 prompt，要求回答带引用。
```

高质量 prompt：

```text
请设计公司内部文档 RAG 知识库。知识源包括 Confluence 产品文档、Notion SOP 和 `docs/internal/*.md`；目标用户是客服和产品经理；回答范围只限内部流程、产品功能和故障排查。请建立文档索引字段，设计 chunk size、overlap、metadata、embedding、向量库、top-k 检索、rerank 和上下文组装流程。回答必须带文档标题、路径和段落/chunk 引用；检索不到依据时必须回复“知识库未找到依据”。必须包含权限控制、文档更新机制、过期文档处理、评估问题集、命中率和引用准确率指标。
```
