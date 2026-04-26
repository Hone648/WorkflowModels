````markdown
# AGENTS.md

## Project

This repo is the `budget-tracker` project.

The app is a Next.js App Router TypeScript application using MongoDB Atlas through the MongoDB Node.js driver, Zod validation, TanStack Query, Tailwind CSS, and MUI where approved for UI surfaces.

## Core Rule

Codex CLI is the local repo execution tool.

ChatGPT role chats manage scope, workflow, QA review, and repo operations, but Codex CLI is responsible for inspecting the local repo, making approved file changes, running commands, and reporting results.

Do not assume repo state from prior chat history. Inspect the local repo before implementation, QA, or repo operations.

## Required Startup Reads

Before doing implementation, QA, or repo operations, read these files when present:

1. `AGENTS.md`
2. `docs/snapshots/00-current-state.md`
3. `docs/workflow/handoff-procedures.md`
4. Any workflow document named in the task prompt
5. Relevant source files for the approved task

If any required file is missing, report that clearly before continuing.

If repo contents conflict with the task prompt, report the conflict before editing.

## Workflow Authority

The Manager chat is the decision source of truth.

Codex must not make Manager level decisions.

Codex must not decide:

1. Scope approval
2. Product acceptance
3. QA acceptance
4. Merge approval
5. Snapshot timing
6. Branch cleanup approval
7. Product direction outside the approved task

## Scope Rules

Work only on the Manager approved task.

Do not expand scope.

Do not perform unrelated cleanup.

Do not refactor unrelated files.

Do not introduce new libraries unless explicitly approved.

Do not add authentication unless explicitly approved.

Do not add MongoDB persistence changes unless explicitly approved.

Do not add CRUD behavior unless explicitly approved.

Do not create final production domain modeling unless explicitly approved.

Do not update `docs/snapshots/00-current-state.md` unless the task explicitly says snapshot update is approved.

Do not merge branches unless the task explicitly says merge is approved.

Do not delete or clean up branches unless the task explicitly says cleanup is approved.

## Branch Rules

For implementation tasks, start by creating a new Git branch unless the task explicitly says otherwise.

Use clear branch names that describe the approved slice.

Before making changes, run:

```bash
git status --short
git branch --show-current
```
````

If the working tree is dirty before starting, stop and report the dirty state unless the task explicitly instructs recovery work.

## Architecture Rules

Use server components by default.

Keep route handlers thin.

Put business logic in services.

Put database access in repositories.

Validate API boundaries with Zod.

Do not access MongoDB directly from client components.

Keep TypeScript strict.

Avoid `any` unless there is a clear, documented reason.

Prefer small, focused modules over large mixed responsibility files.

Keep naming consistent with existing repo conventions.

## UI Rules

Follow existing app styling and layout conventions.

Use MUI only where the approved task calls for it or where the existing surface already uses it.

Do not redesign unrelated pages.

Do not add broad navigation, layout, or theme changes unless explicitly approved.

For mocked UI slices, keep data clearly mocked and avoid implying persistence exists.

## Data Rules

Do not change database schema, seed data, repositories, or persistence behavior unless explicitly approved.

Do not alter API contracts unless explicitly approved.

When adding validation, keep Zod schemas near the relevant boundary or existing convention.

Money values should remain consistent with the project’s existing money handling rules.

Do not introduce floating point money persistence.

## Testing and Validation

Before returning a completed implementation report, run these commands unless the task says otherwise:

```bash
git status --short
npm run format:check
npm run lint
npm run typecheck
npm run build
```

Run additional tests, unit tests, or Playwright checks when required by the task.

If a command fails, report:

1. The command
2. The failure
3. Whether it appears related to the task
4. What was changed, if anything, to address it

Do not hide failed or skipped checks.

## QA Mode Rules

When performing QA, inspect and report findings only.

Do not change files.

Do not fix defects.

Do not reformat files.

Do not update snapshots.

Do not merge branches.

Categorize findings as:

1. Blocker
2. Major issue
3. Minor issue
4. Non blocking note
5. Validated behavior
6. Not tested because of access or runtime limits

## Repo Ops Mode Rules

When performing repo operations, inspect repo state before action.

Always report:

1. Current branch
2. `git status --short`
3. Relevant recent commits
4. Approved operation
5. Operations performed
6. Final working tree state

Do not use destructive Git commands unless explicitly approved.

Do not merge unless explicitly approved.

Do not update `docs/snapshots/00-current-state.md` unless explicitly approved.

Do not clean up branches unless explicitly approved.

## Pre Implementation Findings

For implementation tasks, before editing files, produce a short pre implementation findings summary that includes:

1. Files inspected
2. Relevant existing behavior
3. Planned change path
4. Risks or uncertainties
5. Whether the approved scope appears feasible

Then proceed only within the approved scope.

## Final Implementation Report Format

Return a concise final report with:

1. Branch name
2. Files changed
3. Pre implementation findings
4. Implementation summary
5. Scope compliance
6. Validation commands run
7. Command results
8. Known limitations
9. Risk notes
10. Whether the candidate is ready for QA
11. Working tree status

## Final QA Report Format

Return a concise QA report with:

1. Branch inspected
2. Scope inspected
3. Validated behavior
4. Confirmed issues
5. Non blocking notes
6. Runtime or access limits
7. Recommendation
8. Working tree status

Recommendation must be one of:

1. Ready for Manager acceptance
2. Needs implementation fix
3. Needs Manager decision
4. Needs Repo Ops review
5. Blocked

## Final Repo Ops Report Format

Return a compact Repo Ops report with exactly these sections:

1. Outcome
2. Key facts
3. Next move
4. Notes

The `Next move` section must contain exactly one recommended next move line.

Include:

```text
Approval boundary respected: yes/no
```

Only mark `yes` if all actions stayed within the Manager approved scope, merge authority, and snapshot authority.

## Drift Control

If a task prompt conflicts with `docs/workflow/handoff-procedures.md`, follow `docs/workflow/handoff-procedures.md` unless the task explicitly says Manager is overriding it for this specific task.

If a task prompt conflicts with `AGENTS.md`, report the conflict before proceeding.

If the approved task is missing required scope, exclusions, validation commands, or return format, report the gap before editing.

## Completion Standard

A task is not complete until:

1. Approved scope is implemented or inspected
2. Required commands are run or clearly explained as skipped
3. Working tree state is reported
4. Risks and limitations are disclosed
5. The correct final report format is returned

```

```
