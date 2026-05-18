# Skills

Project-specific skills for this Paperclip instance. Pre-installed Paperclip skills (`paperclip`, `para-memory-files`, `paperclip-converting-plans-to-tasks`, `paperclip-create-agent`, `diagnose-why-work-stopped`) are available to all agents automatically and are not duplicated here.

## Agent-to-Skill Mapping

| Skill | Architect | Frontend Lead | Backend Lead | Frontend Dev | Backend Dev |
|-------|-----------|---------------|--------------|--------------|-------------|
| `plan-mode` | ✅ | ✅ | ✅ | | |
| `plan-create` | | ✅ | ✅ | | |
| `plan-update` | | ✅ | ✅ | | |
| `plan-refactor` | | ✅ | ✅ | | |
| `grill` | | | | | |
| `grill-with-docs` | ✅ | | | | |
| `caveman` | | | | | |

Unassigned skills (`grill`, `caveman`) are available for ad-hoc use but not referenced by any agent's AGENTS.md.

## Skill Descriptions

### plan-mode
Holistic, system-aware planning before implementing non-trivial tasks. 6-step doctrine: Explore → Clarify → Design → Review → Present → Hand Off. Plan-mode agents (Architect, Leads) produce plans and delegate; build-mode agents (Devs) receive plans and implement.

### plan-create
Creates structured implementation plan documents with front matter, requirements, phased tasks (REQ-/TASK- identifiers), dependencies, files, testing, and risks. Used by Leads to produce detailed implementation plans for Dev agents.

### plan-update
Updates existing implementation plan documents with new or modified requirements. Fetches the current plan, determines whether to revise or overwrite, and updates the issue `plan` document with `baseRevisionId` for conflict safety.

### plan-refactor
Creates concrete refactor plans with current/target state, affected files table, phased execution plan, rollback plan, and risks. Emphasizes safe sequencing: types → implementations → callers → tests → cleanup.

### grill
Relentless interview mode — asks one question at a time about a plan or design, resolving each branch of the decision tree. Questions posted as issue comments.

### grill-with-docs
Same as `grill` but adds domain awareness: challenges terminology against `CONTEXT.md`, creates ADRs sparingly, updates the glossary inline as decisions crystallize. Used by the Architect for design stress-testing.

### caveman
Ultra-compressed communication mode. Drops filler, articles, and pleasantries while keeping full technical accuracy. Persists once triggered. Not structurally used by any agent but available for token-sensitive contexts.

## Paperclip Integration Notes

All plan-producing skills store their output in Paperclip issue `plan` documents via `PUT /api/issues/{issueId}/documents/plan`, not in repository files. Human review gates use `request_confirmation` interactions. Phase/task structures map to Paperclip subtask issues via the `paperclip-converting-plans-to-tasks` skill.