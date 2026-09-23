---
name: execution-efficiency
description: Use when implementing, fixing, debugging, reviewing, auditing, testing, refactoring, or modifying software in a Git repository. Provides an efficient execution workflow that minimizes redundant investigation, repeated validation, unnecessary tool calls, and excessive iteration while preserving correctness and safety.
---

# Efficient Software Engineering

Work thoroughly and correctly while minimizing unnecessary model, tool, terminal, and reasoning turns.

Efficiency never overrides correctness, security, data integrity, compatibility, repository policy, tool or confirmation requirements, or explicit user instructions.

## Execution loop

Prefer this sequence:

1. Understand the request and acceptance criteria.
2. Inspect the relevant implementation, repository instructions, and tests.
3. Identify concrete gaps and form a plan proportional to the task.
4. Implement logically related changes together.
5. Run focused validation that proves the changed behavior.
6. Treat failures as evidence, fix confirmed causes, and rerun the affected checks.
7. Perform sufficient final validation.
8. Review the final diff and repository state.
9. Complete requested Git operations.
10. Stop and report the result.

When the next required action is clear, permitted by active policy, and does not require user input, perform it directly.

Avoid routine progress narration. Communicate during execution when a meaningful milestone, blocker, changed assumption, or actionable result matters to the user.

### Task mode

Determine whether the task is read-only or modifying before acting.

For review, audit, evaluation, explanation, investigation, or analysis tasks, do not modify files, repository state, branches, commits, or pull requests unless the user explicitly requests changes.

For implementation, fixing, refactoring, or modification tasks, make only the changes required to satisfy the request.

Instructions later in this skill about implementation, validation, or Git operations apply only when relevant to the task mode.

## Evidence and scope

User requirements and acceptance criteria define the desired outcome and constraints.

For claims about technical state, use the authoritative source closest to the claim when practical:

1. repository or configuration state for what is defined,
2. runtime state or observations for what actually occurred,
3. focused tests for expected behavior,
4. reliable conclusions already established in the current task that remain valid,
5. external or upstream investigation when local evidence cannot answer the question.

Do not rediscover a fact from a more expensive source when valid existing evidence already establishes it. If state may have changed, re-check the authoritative source rather than trusting stale context.

When new evidence contradicts an earlier conclusion, investigate the contradiction specifically rather than restarting discovery.

### Repeat guard

Before repeating a search, file read, status check, test, build, audit, or external investigation, identify what new unresolved fact the repetition can establish.

If it cannot establish a materially new fact, do not repeat it.

Repeat or broaden investigation only when evidence is incomplete or contradictory, state changed, the previous method was insufficient, or a concrete correctness, security, compatibility, or requirement question remains.

Exploratory investigation is appropriate when the user explicitly requests a deep, exhaustive, architectural, or security review, or when the change affects a clearly high-risk surface. Even then, tie each investigation to a distinct risk or question.

For review and audit tasks, establish the relevant review dimensions once and inspect each relevant surface sufficiently to answer them. Revisit an already-reviewed surface only when a concrete finding, contradiction, or dependency creates a new unresolved question.

### Scope discipline

Solve the task the user requested.

Do not silently expand implementation work into a repository-wide audit, dependency audit, architecture review, security review, performance investigation, upstream investigation, or unrelated cleanup unless the requested work requires it.

Do not investigate unrelated findings. Briefly note only material, actionable findings discovered incidentally unless they must be fixed for the requested implementation to work correctly.

## Implementation

Follow existing project architecture, conventions, abstractions, and patterns unless the task requires changing them.

Prefer simple, maintainable solutions over unnecessary abstractions. Understand relationships between coupled files first, then make coherent related changes together instead of many tiny edit/test cycles.

Do not change code merely to address hypothetical problems unsupported by requirements, evidence, or a concrete identified risk.

Prefer modifying authoritative source files rather than generated artifacts, compiled bundles, caches, vendored output, installed package copies, or runtime copies unless the task specifically concerns those artifacts or no source-level path exists.

If a generated or runtime artifact must be changed, determine how it is produced or replaced so the fix is not unintentionally lost.

## Testing and validation

Validation should prove the behavior that changed.

Prefer:

1. existing tests nearest the changed behavior,
2. targeted new or updated tests when coverage is missing,
3. relevant linting, formatting, type checking, schema/config validation, or builds,
4. broader suites when repository instructions, coupling, regression risk, or the task justify them.

During implementation, use the smallest relevant validation that provides useful feedback. Batch related edits before rerunning checks when practical.

Do not finish an implementation without meaningful validation unless it is unavailable, impossible in the environment, or genuinely unnecessary for the type of change. Report any validation that could not be performed and why.

When a test fails after a change, determine whether the implementation or the test conflicts with the intended contract. Do not weaken, delete, skip, bypass, or rewrite tests merely to obtain a passing result.

A successful check remains valid if no later modification could affect what it validated. Rerun it only when later changes may invalidate it, the earlier run did not cover the final implementation, repository instructions require it, or new evidence creates a concrete uncertainty.

## Failure-driven debugging

When a command, test, build, CI job, or runtime check fails:

1. Read the relevant failure output.
2. Identify the most likely concrete cause supported by that output.
3. Inspect only what is needed to confirm or reject that cause.
4. Make the appropriate fix.
5. Rerun the smallest validation capable of proving the fix.
6. Broaden investigation only if the focused approach fails or reveals a wider problem.

Do not respond to a focused failure by restarting repository discovery or rerunning unrelated checks.

If a failure is environmental, external, flaky, or unrelated to the implementation, establish that with reasonable evidence and report it rather than modifying unrelated code to make it disappear.

## Tool and context efficiency

Treat every tool call as purposeful: it should answer an unresolved question or perform a required action.

Prefer batched related searches and file reads, targeted searches, focused tests, filtered output, and parallel independent operations when safe.

Keep operations sequential when ordering affects state, later work depends on earlier results, failure determines the next action, or batching would hinder diagnosis.

Avoid large full-file reads, logs, diffs, dependency trees, or generated bundles when a targeted range or query answers the question. Read small or structurally important files in full when doing so reduces uncertainty or prevents fragmented investigation.

Do not repeatedly retrieve output that remains available and valid in context. Preserve conclusions and current working state rather than reconstructing the full investigation after context condensation.

## Long tasks

For long or complex work, compare progress with the user's explicit requirements at natural milestones or after a meaningful failure.

Identify what is complete, what remains, any concrete blocker, and the next action that closes a remaining gap. Do not reopen completed requirements without contradictory evidence.

As execution grows longer, become more selective about optional investigation. Prioritize remaining explicit requirements and necessary validation.

Stop with partial completion only when a real tool, environment, execution, or policy limit prevents safe completion. Preserve the working state and report the concrete remaining work.

## Workspace and Git safety

Preserve pre-existing user work and never include unrelated changes in the task.

Before finishing repository changes, inspect the final diff and relevant repository state, confirm only intended changes were made, and ensure required validation passes or unavailable validation is explicitly identified.

If the user requested a commit, push, existing branch, or pull request workflow, complete it after applicable verification is complete. If required validation cannot run, report the limitation rather than repeatedly investigating or silently treating it as successful.

For substantial permanent-workspace preparation, cleanup, branch handling, stash management, or repository hygiene, use the `workspace-hygiene` skill when available rather than duplicating that workflow here.

## Completion gate

Enter the completion phase when all applicable conditions are true:

- every explicit user requirement is addressed,
- the intended implementation is present,
- relevant edge cases identified by the requirements, changed behavior, evidence, or a concrete risk have been considered,
- appropriate validation passes, or unavailable validation has been identified and reported,
- the final diff contains only intended task changes,
- validation is sufficient to safely perform any requested final Git operations,
- and no concrete unresolved blocker or correctness concern remains.

Once the gate is satisfied, stop exploratory investigation. Do not begin another audit pass, reread unchanged files merely to reconfirm conclusions, search for hypothetical additional problems, or rerun successful validation without new evidence.

Complete any remaining requested Git operations, report the result, and finish.

Treat final Git operations such as commit, push, or pull-request creation as terminal completion actions. Resume investigation only if a Git operation itself produces concrete new evidence of a problem.

## Completion report

Keep the final report concise. Include:

- what changed,
- what validation was performed,
- any concrete remaining limitation or follow-up that matters,
- and relevant Git or pull-request status when applicable.

Do not reproduce large logs, diffs, previous reasoning, or a chronology of routine actions.
