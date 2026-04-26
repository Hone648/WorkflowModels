````markdown
# Handoff Procedures

## Purpose

This document defines the standard handoff procedure for the `budget-tracker` workflow.

The goal is to prevent drift between Manager, Scope Lock, Implementation Review, QA Review, Repo Ops Review, and Codex CLI.

## Core Model

ChatGPT role chats manage the process.

Codex CLI inspects and changes the local repo.

The local repo is the implementation source of truth.

The Manager chat is the decision source of truth.

## Role Summary

| Role                  | Owns                                                                                           | Does Not Own                                     |
| --------------------- | ---------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| Manager               | Scope decisions, sequencing, acceptance decisions, routing, merge approval, snapshot timing    | Direct implementation                            |
| Scope Lock            | Turning ideas into bounded slices                                                              | Code changes, QA, merge decisions                |
| Implementation Review | Codex CLI implementation prompts and returned implementation report review                     | QA acceptance, merge decisions, snapshot updates |
| QA Review             | Codex CLI QA prompts and returned QA finding review                                            | Fixes, merge decisions, product decisions        |
| Repo Ops Review       | Git operation prompts, branch state review, merge and cleanup execution after Manager approval | Scope, QA acceptance, product behavior           |
| Codex CLI             | Local repo inspection, approved file changes, command execution, report generation             | Manager level decisions                          |

## Universal Rules

1. Do not assume repo contents from memory.
2. Codex CLI must inspect the local repo before implementation, QA, or repo operations.
3. All implementation work must be tied to a Manager approved slice.
4. Scope must not expand during implementation.
5. QA reports findings only.
6. QA does not fix code.
7. Repo Ops does not decide product acceptance.
8. Snapshot updates require explicit Manager approval.
9. Merge execution requires explicit Manager approval.
10. Branch cleanup requires explicit Manager approval.
11. Every returned report must clearly state validation limits.
12. If a required file, command, or context item is unavailable, report it clearly.

## Standard Workflow

1. Manager identifies a need.
2. Scope Lock defines the slice.
3. Manager approves, rejects, or revises the slice.
4. Implementation Review creates the Codex CLI implementation prompt.
5. Codex CLI implements locally.
6. Implementation Review reviews the returned implementation report.
7. Manager routes to QA Review when QA is required.
8. QA Review creates the Codex CLI QA prompt.
9. Codex CLI inspects and reports QA findings without changing files.
10. QA Review reviews the returned QA report.
11. Manager accepts, rejects, requests fixes, or routes to Repo Ops.
12. Repo Ops creates a Codex CLI repo operation prompt when needed.
13. Codex CLI performs approved repo operations.
14. Repo Ops reviews the returned repo operation report.
15. Manager decides the final next move.

## Handoff Format

Every handoff must include:

1. Project name
2. Source chat
3. Destination chat or tool
4. Approved task
5. Context
6. Included scope
7. Explicit exclusions
8. Required files to read
9. Required commands
10. Expected return format
11. Approval boundaries

## Prompt Header Rule

Every prompt created for another chat or Codex CLI must include:

```text
Project: budget-tracker
Chat: [source chat name]
Destination: [destination chat or Codex CLI]
```
````

When creating a prompt for another ChatGPT workflow chat, the prompt must begin with a plain text code block containing only the destination chat name.

Example:

```text
Implementation Review
```

When creating a prompt for Codex CLI, the prompt must begin with a plain text code block containing the source workflow chat name.

Example:

```text
Implementation Review
```

## Manager To Scope Lock Handoff

Use this handoff when an idea needs to become a bounded slice.

Required sections:

1. Product idea or problem
2. Current known context
3. Desired user visible outcome
4. Known constraints
5. Explicitly forbidden scope
6. Questions or assumptions to resolve
7. Expected Scope Lock report format

Scope Lock must return:

1. Slice name
2. Goal
3. Included scope
4. Excluded scope
5. User visible acceptance criteria
6. Technical acceptance criteria
7. Suggested Codex inspection targets
8. Risks and guardrails
9. Recommended next move

## Scope Lock To Manager Return

Scope Lock returns a recommendation only.

Scope Lock does not approve implementation.

Required sections:

1. Scope Lock Report
2. Recommended action
3. Whether the slice is ready for Manager approval
4. Any unresolved assumptions

Manager must explicitly approve the slice before Implementation Review creates a Codex CLI prompt.

## Manager To Implementation Review Handoff

Use this handoff only after Manager approval of a slice.

Required sections:

1. Approved slice name
2. Approved goal
3. Included scope
4. Explicit exclusions
5. Acceptance criteria
6. Required docs to read
7. Suggested code paths to inspect
8. Required validation commands
9. Expected Codex CLI return format

Implementation Review must not expand the approved scope.

## Implementation Review To Codex CLI Handoff

Required sections:

1. Branch instruction
2. Files and docs to read
3. Local repo inspection requirement
4. Pre implementation findings requirement
5. Approved scope
6. Explicit exclusions
7. Architecture constraints
8. UI constraints
9. Data constraints
10. Validation commands
11. Final report format

The Codex CLI prompt must instruct Codex to:

1. Create a new branch unless Manager explicitly says otherwise.
2. Read `AGENTS.md`.
3. Read `docs/snapshots/00-current-state.md` if present.
4. Read `docs/workflow/handoff-procedures.md` if present.
5. Read any workflow file named in the prompt.
6. Inspect relevant code paths before editing.
7. Return a short pre implementation findings summary before changing files.
8. Keep changes limited to the approved scope.
9. Run required validation commands.
10. Return the required implementation report.

## Codex CLI Implementation Return

Codex CLI must return:

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

## Implementation Review To Manager Return

Implementation Review must review the Codex CLI report and return one Manager facing report.

Required sections:

1. Outcome
2. Scope compliance
3. Validation status
4. Risks or blockers
5. QA recommendation
6. Recommended next move

Implementation Review must not make QA acceptance decisions.

Implementation Review must not merge.

Implementation Review must not update snapshots.

## Manager To QA Review Handoff

Use this handoff when a candidate needs QA inspection.

Required sections:

1. Candidate branch
2. Approved slice name
3. Approved scope
4. Acceptance criteria
5. Implementation report summary
6. Known risks or limitations
7. Required docs to read
8. Required validation commands
9. Expected QA report format

## QA Review To Codex CLI Handoff

Required sections:

1. Candidate branch
2. Approved scope
3. Diff inspection requirement
4. Validation commands
5. No file changes rule
6. Finding categories
7. Final QA report format

The Codex CLI prompt must instruct Codex to:

1. Read `AGENTS.md`.
2. Read `docs/snapshots/00-current-state.md` if present.
3. Read `docs/workflow/handoff-procedures.md` if present.
4. Inspect the implementation branch.
5. Inspect the diff against the appropriate base branch.
6. Compare behavior against the approved scope and acceptance criteria.
7. Run relevant checks.
8. Report findings only.
9. Make no file changes.
10. Return the required QA report.

## Codex CLI QA Return

Codex CLI must return:

1. Branch inspected
2. Scope inspected
3. Validated behavior
4. Confirmed issues
5. Non blocking notes
6. Runtime or access limits
7. Recommendation
8. Working tree status

Finding categories must be:

1. Blocker
2. Major issue
3. Minor issue
4. Non blocking note
5. Validated behavior
6. Not tested because of access or runtime limits

Recommendation must be one of:

1. Ready for Manager acceptance
2. Needs implementation fix
3. Needs Manager decision
4. Needs Repo Ops review
5. Blocked

## QA Review To Manager Return

QA Review must review the Codex CLI QA return and send one Manager facing report.

Required sections:

1. Outcome
2. Validated behavior
3. Confirmed issues
4. Non blocking notes
5. Runtime or access limits
6. Recommendation
7. Recommended next move

QA Review must not fix code.

QA Review must not merge.

QA Review must not update snapshots.

## Manager To Repo Ops Handoff

Use this handoff only after Manager explicitly approves repo operations.

Required sections:

1. Approved repo operation
2. Current known branch or candidate branch
3. Current known status, if available
4. Merge authority status
5. Snapshot authority status
6. Branch cleanup authority status
7. Required inspection commands
8. Allowed operations
9. Forbidden operations
10. Expected compact Repo Ops return format

## Repo Ops Review To Codex CLI Handoff

Required sections:

1. Approved repo operation
2. Current known branch or state, if available
3. Inspection commands
4. Allowed operations
5. Forbidden operations
6. Snapshot authority status
7. Merge authority status
8. Branch cleanup authority status
9. Final compact report format

The Codex CLI prompt must instruct Codex to:

1. Read `AGENTS.md`.
2. Read `docs/snapshots/00-current-state.md` if present.
3. Read `docs/workflow/handoff-procedures.md` if present.
4. Inspect git state before taking action.
5. Report current branch.
6. Report `git status --short`.
7. Report relevant recent commits.
8. Avoid destructive commands unless explicitly approved.
9. Perform only the approved repo operation.
10. Return the required compact Repo Ops report.

## Codex CLI Repo Ops Return

Codex CLI must return exactly these sections:

1. Outcome
2. Key facts
3. Next move
4. Notes

The `Next move` section must contain exactly one recommended next move line.

The report must include:

```text
Approval boundary respected: yes/no
```

Only mark `yes` if all actions stayed within the Manager approved scope, merge authority, snapshot authority, and branch cleanup authority.

## Repo Ops Review To Manager Return

Repo Ops Review must review the Codex CLI repo operation return and send one compact Manager facing report.

Required sections:

1. Outcome
2. Key facts
3. Next move
4. Notes

The `Next move` section must contain exactly one recommended next move line.

## Snapshot Rule

`docs/snapshots/00-current-state.md` is updated only after Manager approval.

A completed implementation does not automatically authorize snapshot updates.

QA passing does not automatically authorize snapshot updates.

Merge completion does not automatically authorize snapshot updates unless Manager explicitly says snapshot update is part of the approved Repo Ops task.

Snapshot update prompts must include:

1. What slice was completed
2. What changed
3. What user visible behavior is now accepted
4. What technical state changed
5. What remains intentionally incomplete
6. Validation evidence
7. Whether the update should replace, append, or revise existing snapshot sections

## Merge Rule

No chat may authorize merge except Manager.

Repo Ops may prepare a merge plan, but actual merge execution requires explicit Manager approval.

A merge prompt must state:

```text
Merge authority: approved
```

If that line is absent, Codex CLI must not merge.

## Branch Cleanup Rule

No branch cleanup happens automatically.

Branch cleanup requires explicit Manager approval after merge or abandonment is confirmed.

A branch cleanup prompt must state:

```text
Branch cleanup authority: approved
```

If that line is absent, Codex CLI must not delete or clean branches.

## Dirty Working Tree Rule

If Codex CLI detects a dirty working tree before starting a task, it must stop and report the dirty state unless the task is explicitly a recovery task.

The report must include:

1. Current branch
2. `git status --short`
3. Whether changes are tracked, untracked, or staged
4. Recommended next move

## Validation Rule

Required validation commands must be run unless unavailable or explicitly excluded.

If a command is skipped, the report must state:

1. Command skipped
2. Reason skipped
3. Whether this creates a validation gap
4. Whether Manager or QA must decide next

## Drift Control

This document is the canonical handoff contract.

If a chat response conflicts with this document, this document wins unless Manager explicitly overrides it for a specific task.

If `AGENTS.md` conflicts with this document, Codex CLI must report the conflict before proceeding.

If a handoff is missing required scope, exclusions, validation, or return format, the receiving chat must return a blocked or needs correction report rather than guessing.

## Completion Standard

A workflow step is complete only when:

1. The assigned role stayed inside its authority.
2. Required files were read or missing files were reported.
3. Required inspection or validation occurred, or skipped steps were explained.
4. Scope compliance was addressed.
5. Working tree state was reported when Codex CLI was involved.
6. The correct return format was used.
7. The next move is explicit.

```

```
