# Common Prompt Engineering Principles

## 1. Clear Objective
A prompt must state what to complete, why it matters, what success looks like, and what is outside scope. Use an observable verb such as read, inspect, compare, implement, fix, refactor, test, summarize, extract, convert, validate, or report. Replace weak wording with action, object, and result.

## 2. Context Completion
A prompt must name the background, input materials, current state, constraints, user preferences, and target tool or execution environment. If the information is missing, mark it as `[待补充: field]` or as an assumption. Ask a question only when the missing answer blocks safe execution.

## 3. Task Decomposition
Complex tasks must be staged: read context, analyze the problem, design a solution, execute the task, verify the result, and report. Each stage must have an input, action, output, and acceptance condition.

## 4. Strong Constraints
Use `必须` for non-negotiable requirements, `禁止` for prohibited actions, `优先` for ranking, `除非` for exceptions, `完成后检查` for validation, and `不确定时标注` for assumptions. Constraints must mention file scope, dependencies, network access, destructive commands, data handling, and approval boundaries when relevant.

## 5. Output Format Control
The prompt must choose the output shape before execution starts: Markdown, JSON, table, checklist, file tree, code block, report, patch summary, command log, visual check, or final handoff. State required sections and field names, not just “format clearly.”

## 6. Acceptance Criteria
Every high-quality prompt must include checkable criteria: task complete, constraints satisfied, output format correct, tests or checks run, unrelated changes avoided, uncertainty marked, and user-visible deliverables present.

## 7. Risk Control
Control hallucination, over-editing, wrong assumptions, ignored constraints, generic output, missing verification, and high-risk domain harm. Bind claims to files, logs, screenshots, PDFs, data sources, URLs, or pasted text. Require the agent to stop or mark uncertainty when evidence is missing.

## 8. Self-Check
Final prompts must require a self-check for missing input, ambiguity, conflicting constraints, direct executability, need for human confirmation, and fit with the target tool. If verification fails, the agent must report the failing check and next repair step.

## 9. Tool-Specific Adaptation
- Codex: emphasize working directory, file reading order, modification boundaries, tests, and final changed-file report.
- Codex CLI: emphasize command permissions, execution order, failure handling, and final command/result report.
- Claude Code: emphasize code context, staged edits, minimal patches, and regression checks.
- Gemini CLI: emphasize working directory, file scope, terminal commands, and environment assumptions.
- ChatGPT: emphasize response structure, reasoning boundaries, source limits, and output format because it may not have local file access.

## 10. High-Risk Domain Rules
For legal, medical, finance, security, and similar domains, the prompt must label the information type, avoid replacing professional advice, require sources or evidence, mark uncertainty, avoid overconfident conclusions, and recommend consulting a qualified professional when decisions carry real-world consequences. Defensive security prompts must not ask for exploit payloads, bypass steps, or operational abuse instructions.
