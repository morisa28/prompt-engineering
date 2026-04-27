# Prompt Engineering Skill Hub

One-line positioning: route vague user requests to the right scenario branch and generate high-quality prompts for Codex, Codex CLI, Claude Code, Gemini CLI, ChatGPT, and other AI tools.

## Who It Is For

- Users who need to turn informal requests into executable prompts.
- People writing task prompts for coding, research, data, document, or automation agents.
- Teams maintaining prompt routers, branch templates, eval cases, and reusable AI workflows.
- Designers who need safe boundaries for medical, legal, finance, security, recruiting, privacy, and other high-risk domains.

## Fast Path

1. Read `router.md` and choose one primary branch by final deliverable.
2. Use `branch-composition.md` to add only necessary auxiliary branches.
3. Use `templates.md` and the selected branch file to draft the prompt.
4. Validate with `checklists.md`.

## Full Path

1. Follow `prompt-generation-protocol.md`.
2. Route with `router.md`.
3. Apply `common-principles.md`.
4. Compose with `branch-composition.md`.
5. Draft with `templates.md` and branch templates.
6. Check with `checklists.md`.
7. Learn patterns from `examples.md`.
8. Validate prompt quality with `evals/`.
9. Use `branches/manifest.yaml` for machine-readable branch metadata.

## Example

User request:

```text
Write a prompt for Codex to fix an npm run build error and avoid unrelated changes.
```

Routing:

```text
Primary branch: software-engineering/bugfix-debugging
Auxiliary branches: software-engineering/test-generation, software-engineering/cli-agent
Risk level: medium
Missing inputs: error log, reproduction steps, working directory, environment, verification command
```

Generated prompt:

```text
You are Codex. In `[pending: working_directory]`, fix the `npm run build` failure. Error log: `[pending: error_log]`. First read `package.json`, build config, stack-trace files, and related tests. Reproduce the failure or explain why it cannot be reproduced, then identify the root cause. Make the smallest safe fix. Do not perform unrelated refactors, repository-wide formatting, new dependencies, test deletion, assertion weakening, or error-hiding changes. After the fix, run `npm run build` and `[pending: test_command]`. Report root cause, evidence, changed files, key changes, verification command results, unverified items, and remaining risks.
```

## Primary Branch Rule

Choose the primary branch by the final deliverable:

- Fix an error: `branches/software-engineering/bugfix-debugging.md`
- Understand a repository: `branches/software-engineering/repository-analysis.md`
- Implement a feature: `branches/software-engineering/coding-feature-development.md`
- Add tests: `branches/software-engineering/test-generation.md`
- Write a PRD: `branches/product-design-business/product-requirements.md`
- Design RAG: `branches/ai-systems/knowledge-base-rag.md`
- Analyze data: `branches/data-analytics/data-analysis.md`
- Write a report: `branches/documents-research/report-writing.md`
- Handle medical/legal/finance: corresponding `branches/domain-specific/` branch
- Improve a prompt system: `branches/meta/meta-skill-builder.md`

## Eval Cases

Eval cases check prompt-generation quality, not downstream task results. Each YAML case includes expected branches, missing inputs, expected prompt features, forbidden prompt features, and acceptance criteria.

```bash
python3 scripts/skill_hub_manager.py validate
python3 scripts/skill_hub_manager.py stats
```

## High-Risk Domains

- Medical: no diagnosis, no prescription, no stop/switch medication advice.
- Legal: no final legal conclusion, require jurisdiction and source text.
- Finance: no personalized investment advice, no guaranteed returns, no buy/sell instruction.
- Security: defensive only; no exploit chains, payloads, bypass, stealth, or persistence.
- Recruiting: no protected-attribute judgment; use job-related evidence only.
- Privacy: minimize, redact, limit purpose, and define retention boundaries.
