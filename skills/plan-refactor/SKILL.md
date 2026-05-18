---
name: plan-refactor
description: 'Create a concrete plan before starting a multi-file refactor. Use when the user asks to plan, sequence, scope, or safely execute a refactor across multiple files; always investigate first, output the plan to the issue document, and wait for confirmation before delegating implementation.'
---

# Refactor Plan

Create a detailed plan before making any code changes.

## Instructions

1. Do not edit files while preparing the plan.
2. Search the codebase to understand the current state. Read enough implementation, tests, configuration, and docs to make the plan specific to the repository.
3. Identify affected files, ownership boundaries, dependencies, and likely hidden coupling.
4. Plan changes in a safe sequence. Prefer contracts and types first, then implementations, then callers, then tests, then cleanup.
5. Include verification steps between phases and a final validation command.
6. Include rollback or recovery steps for the riskiest phases.
7. Output the complete plan using the format below.
8. Write the plan to the issue `plan` document using `PUT /api/issues/{issueId}/documents/plan`. If a plan already exists, fetch it first via `GET /api/issues/{issueId}/documents/plan` and include its `baseRevisionId` when updating.
9. After writing the plan, open a `request_confirmation` interaction tied to the latest plan revision and set the issue to `in_review`. Do not proceed to implementation or subtask creation until confirmation is received.

If the request is too ambiguous to plan safely, post clarifying questions as issue comments instead of editing files.

## Output Format

```markdown
## Refactor Plan: [title]

### Current State
[Brief description of how things work now]

### Target State
[Brief description of how things will work after]

### Affected Files
| File | Change Type | Dependencies |
|------|-------------|--------------|
| path | modify/create/delete | blocks X, blocked by Y |

### Execution Plan

#### Phase 1: Types and Interfaces
- [ ] Step 1.1: [action] in `file.ts`
- [ ] Verify: [how to check it worked]

#### Phase 2: Implementation
- [ ] Step 2.1: [action] in `file.ts`
- [ ] Verify: [how to check]

#### Phase 3: Tests
- [ ] Step 3.1: Update tests in `file.test.ts`
- [ ] Verify: Run `npm test`

#### Phase 4: Cleanup
- [ ] Remove deprecated code
- [ ] Update documentation

### Rollback Plan
If something fails:
1. [Step to undo]
2. [Step to undo]

### Risks
- [Potential issue and mitigation]
```

## Mapping to Paperclip Issues

The refactor plan's phases map to Paperclip subtask issues:

- Each **Phase** can become a subtask issue with `parentId` linking to the parent refactor issue
- Steps within a phase become descriptions for individual subtask issues
- Phase dependencies (e.g., Phase 2 depends on Phase 1) map to `blockedByIssueIds`
- Use the `paperclip-converting-plans-to-tasks` skill for detailed guidance on decomposing plans into executable issues

## Hand Off

Plan-mode agents do not implement refactors themselves. After the plan is confirmed:

- Reassign the issue to the appropriate build-mode agent or create subtask issues with `parentId`, `goalId`, and `blockedByIssueIds` to express phase dependencies.
- Include the issue document link `/<prefix>/issues/<issue-identifier>#document-plan` in subtask descriptions so implementers can reference the full plan.