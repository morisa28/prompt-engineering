# Prompt Engineering Skill Hub

This is a distributable copy of the `prompt-engineering` skill. It turns vague requests into high-quality prompts that can be executed by Codex, Codex CLI, Claude Code, Gemini CLI, ChatGPT, and other AI tools.

## Contents

- `SKILL.md`: Main entry point with purpose, trigger rules, branch categories, and quality standards.
- `router.md`: Routing system for selecting a primary branch and optional auxiliary branches.
- `common-principles.md`: Shared prompt engineering principles.
- `templates.md`: Copy-ready cross-branch templates.
- `checklists.md`: Executable quality checklists.
- `examples.md`: Complete cross-branch examples.
- `branches/`: Scenario-specific branch library with 45 branches.
- `scripts/`: Extension management scripts for adding branches, validating structure, collecting statistics, and reporting capabilities.

## Branch Categories

- `general-prompt`: prompt review, rewrite, expansion, compression, and template building.
- `software-engineering`: plan mode, feature development, debugging, refactoring, testing, code review, repository analysis, CLI agents, security threat modeling, DevOps/CI, database migration, and API design.
- `documents-research`: documentation analysis, PDF to skill, research synthesis, academic writing, and report writing.
- `data-analytics`: data analysis, spreadsheet analysis, and visualization dashboards.
- `product-design-business`: product requirements, UX/UI design, business strategy, and marketing content.
- `ai-systems`: knowledge base and RAG prompts.
- `business-operations`: customer service QA and recruiting evaluation.
- `education`: curriculum design.
- `creative-design`: game design.
- `multimodal`: image, video/audio, and visual 3D interaction tasks.
- `domain-specific`: legal, medical, finance, and tutoring prompts.
- `communication`: translation/localization and roleplay simulation.
- `automation`: automation workflows.
- `meta`: meta skill builder.

## How To Use

1. Read `SKILL.md`.
2. Read `router.md` and choose the primary and auxiliary branches.
3. Apply the shared rules in `common-principles.md`.
4. Use `templates.md` and the selected branch template to draft the prompt.
5. Validate the prompt with `checklists.md` and the branch checklist.
6. Refer to `examples.md` when a concrete pattern is useful.

## Extension Management Scripts

This copy includes `scripts/skill_hub_manager.py` for Skill Hub maintenance:

```bash
python3 scripts/skill_hub_manager.py validate
python3 scripts/skill_hub_manager.py stats
python3 scripts/skill_hub_manager.py capabilities
python3 scripts/skill_hub_manager.py init-spec --output <branch-spec.json>
python3 scripts/skill_hub_manager.py add-branch --spec <branch-spec.json> --dry-run
python3 scripts/skill_hub_manager.py add-branch --spec <branch-spec.json>
```

Script capabilities:

- `validate`: checks root files, the required 10-section branch structure, router references, template placeholders, checklist items, and vague phrase warnings.
- `stats`: counts branches, categories, templates, checklists, and examples.
- `capabilities`: prints the current Skill Hub capability summary.
- `init-spec`: creates a sample JSON spec for a new branch.
- `add-branch`: adds a branch from a JSON spec and updates `SKILL.md`, `router.md`, `templates.md`, `checklists.md`, and `examples.md`.

## Maintenance Rules

- Do not call scripts during normal prompt generation, review, rewrite, expansion, compression, routing, or template lookup.
- Only ask about and call `add-branch` when the user explicitly requests adding, creating, or registering a new branch.
- Before calling `add-branch`, confirm the branch category, slug, purpose, trigger conditions, required inputs, construction rules, hard constraints, output format, checklist, and example.
- Run `add-branch` with `--dry-run` first; write changes only after the spec is confirmed.
- After adding a branch, run `validate`, `stats`, and `capabilities`, then report changed files, statistics, and remaining issues.

## Quality Standard

A final prompt must include a clear objective, sufficient context, named inputs, concrete execution steps, strong constraints, explicit output format, testable acceptance criteria, risk controls, uncertainty handling, and target-tool adaptation.

## Current Scale

- Branches: 45.
- Categories: 14.
- Reusable templates: 23.
- Quality checklists: 28.
- Cross-branch examples: 28.
- Router examples: 23.

## Distribution Location

- Copy the entire `prompt-engineering` directory into the target Codex skills directory or your team's chosen distribution directory.
- This README does not retain local absolute paths, user names, drive letters, or repository remote information.
