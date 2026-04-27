# Skill Hub Scripts

This directory contains maintenance scripts for the Prompt Engineering Skill Hub.

## Important Usage Policy

Do not run `add-branch` during normal prompt generation, prompt review, routing, or template lookup.

Only use `add-branch` when all of these are true:

1. The user explicitly asks to add, create, or register a new Prompt Engineering Skill Hub branch.
2. The requested branch is not already covered by an existing branch.
3. The user confirms the branch name, category, purpose, trigger conditions, required inputs, constraints, output format, checklist, and example.
4. A JSON spec has been prepared and reviewed.

For all other cases, use the existing router and branch files manually.

## Script

`skill_hub_manager.py`

Commands:

```bash
python3 scripts/skill_hub_manager.py stats
python3 scripts/skill_hub_manager.py validate
python3 scripts/skill_hub_manager.py capabilities
python3 scripts/skill_hub_manager.py init-spec --output <branch-spec.json>
python3 scripts/skill_hub_manager.py add-branch --spec <branch-spec.json> --dry-run
python3 scripts/skill_hub_manager.py add-branch --spec <branch-spec.json>
```

## What `add-branch` Updates

- Creates `branches/<category>/<slug>.md`.
- Updates `SKILL.md` branch categories and branch index.
- Updates `router.md` branch selection rules, auxiliary routing rules, and routing examples.
- Appends a template to `templates.md`.
- Appends a checklist to `checklists.md`.
- Appends an example to `examples.md`.
- Runs structural validation after writing.

## Validation Scope

`validate` checks:

- Required root files exist.
- Every branch has the required 10-section structure.
- Branch files are referenced by `SKILL.md` and `router.md`.
- Branch templates include placeholders.
- Branches include checklist items.
- Known vague phrases are reported as warnings.
